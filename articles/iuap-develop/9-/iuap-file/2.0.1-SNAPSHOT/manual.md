# 文件组件 #

## 简介 ##
文件组件提供对文件上传、下载、删除等功能，提供API对FastDFS、阿里云OSS等文件服务器上的文件进行操作，支持集群形式连接，支持OSS直传，支持对OSS请求的安全性校验等。

## 配置和使用方式 ##
**文件组件目前适配了三种文件系统，本地文件存储、阿里云、FastDfs**

文件组件支持三套文件系统，在使用不同的系统时接口保持统一，通过FileManager类下的静态方法，提供对文件上传下载删除的服务。

**1:工程中引入对iuap-file组件的依赖**

	<dependency>
		<groupId>com.yonyou.iuap</groupId>
		<artifactId>iuap-file</artifactId>
		<version>${iuap.modules.version}</version>
	</dependency>

${iuap.modules.version} 为平台在maven私服上发布的组件的version。

**2:在application.properties文件中配置工程所使用的文件存储系统**


	#使用哪种文件存储系统（AliOss阿里云，Local本地文件存储，FastDfs）
    storeType=AliOss
    #storeType=Local
    #storeType=FastDfs

	#默认存储Bucket(private权限)
    defaultBucket=your bucket
	#默认存储Bucket(read权限)
    defaultBucketRead=your bucket

    #使用本地文件系统时的存储路径
    storeDir=/etc/filetest
	
	#使用FastDfs文件系统时Fdfs系统的配置
    connect_timeout = 10
	network_timeout = 30
	charset = ISO8859-1
	tracker_server = 20.12.6.91:22122
	#处理FastDfs文件系统的nginx服务器ip（文件url获取以及图片缩放功能）
	fdfsread_server = 172.20.7.24

    
    #使用阿里云文件系统
	#阿里云endpoint、accessKeyId、accessKeySecret
    endpoint=oss-cn-beijing.aliyuncs.com
    accessKeyId=your accessKeyId
    accessKeySecret=your accessKeySecret
    #接受阿里云上传回调的主机ip和端口，该主机需要外网能直接访问（直传功能）
    callbackTarget=http://localhost
    #处理上传回调的servlet截取的地址，直传成功后阿里云oss将会对上面配置的主机地址发送url为改配置url的post请求（直传功能）
    callbackUrl=/oss
    #需要上传回调所带的参数，上面配置的的请求将会携带callbackBody配置的参数（直传功能）
    callbackBody=filename=${object}&bucket=${bucket}&size=${size}




**3:功能调用**


**3.1:阿里云oss**


**非直传**

组件提供的API，操作阿里云oss，所有的api都通过FileManager类静态调用，例子：FileManager.uploadFile("yourbucket","test.txt",content)。
    
	//上传，参数（bucket名，文件名，上传文件字节数组），返回（上传文件的文件名）
    String uploadFile(String bucketName, String fileName,byte[] fileContent)
	

 	//下载，参数（bucket名，文件名），返回（下载文件的字节数组）
 	byte[] downLoadFile(String bucketName,String fileName) ;
	

	//删除，参数（bucket名，文件名），返回（删除是否成功标志）
    boolean deleteFile(String bucketName,String fileName)

    //获取文件url，参数（bucket名，文件名，过期时间），返回（上传文件的url）目前expired参数只支持阿里云oss私有bucket，其他系统可置为0
	String getUrl(String bucketName, String fileName,int expired)
	
	//获取图片url，参数（bucket名，文件名，过期时间），返回（图片的url）与getUrl的区别是文件名可以使用类似example.jpg@100h_100w这种格式产生略缩图，）目前expired参数只支持阿里云oss私有bucket，其他系统可置为0
	String getImgUrl(String bucketName,String fileName,int expired)
	
	

example测试类


	/**
	 * 阿里云上传下载删除测试
	 * 测试前请设置application.properties的storeType=AliOss
	 * @throws Exception
	 */
	@Test
	public void testAliUpload() throws Exception {
		String filename;
		//借用文件流生成文件的二进制数组
		LocalClient client =LocalClient.getInstance();		
		byte[] content =client.download("/etc/filetest/test.txt");
		//阿里云上传（功能测试）
		filename=FileManager.uploadFile(null,"test.txt",content);
		//阿里云下载（功能测试）
		byte[] downloadContent=FileManager.downLoadFile("zhukai",filename);
		
		//用文件流将阿里云下载到的文件二进制数组转化为文件
		client.upload("/etc/filetest/aliyuntest.txt", "aliyuntest.txt", downloadContent);
		//阿里云删除（功能测试）
		boolean flag=FileManager.deleteFile("zhukai",filename);
		//删除
		client.deleteFile("/etc/filetest/aliyuntest.txt");
		
		
		System.out.println(filename);
		System.out.println("删除状态"+flag);
		Assert.isTrue(flag);
	}

**直传**

直传功能目前只支持阿里云oss，该功能使文件上传请求和流量不再经过应用服务器而直接交给oss处理，直传功能由阿里云官方文档推荐的框架搭建(相关连接：<a href="https://help.aliyun.com/document_detail/31927.html?spm=5176.doc31923.2.3.VjMHHN">web端直传</a>)

流程

用户请求->Servlet获取签名->js装配获得的签名并上传文件->oss上传文件后向Servlet发送回调->Servlet对oss回调请求处理后向oss发送处理结果->oss向用户返回上传结果。

首先建立一个Servlet类继承CallbackServer，通过该Servlet装配oss直传需要的签名数据，并处理oss只直传完毕后的回调
    
    package com.yonyou.iuap.file;
    
    import java.io.IOException;
    import java.util.Map;
    
    import javax.servlet.ServletException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    //SaasCallbackServer类中的get方法完成了oss直传需要的签名动作，如果有其他需求也可以直接写自己的CallbackServer
    @WebServlet(asyncSupported = true,name = "Oss",urlPatterns = { "/oss" })
    public class OssDirectServlet extends CallbackServer {
    	
    	/**
    	 * 
    	 */
    	private static final long serialVersionUID = 1L;
    
    	protected void doPost(HttpServletRequest request, HttpServletResponse response)
    			throws ServletException, IOException {
    		String ossCallbackBody = GetPostBody(request.getInputStream(), Integer.parseInt(request.getHeader("content-length")));//获取回调
    		boolean ret = VerifyOSSCallbackRequest(request, ossCallbackBody);//验证回调
    		
    		if (ret)
    		{
    			//验证通过进行操作
    			Map<String, Object> map = getUrlParams(ossCallbackBody);
    			String filename=(String) map.get("filename");//获取存储的文件名
    			String bucket=(String) map.get("bucket");//获取存储的bucket
    			System.out.println(filename);
    			System.out.println(bucket);
    			
    			response(request, response, "{\"Status\":\"OK\"}", HttpServletResponse.SC_OK);
    
    		}
    		else
    		{
    			response(request, response, "{\"Status\":\"verdify not ok\"}", HttpServletResponse.SC_BAD_REQUEST);
    		}
    	}
    
    }
    
通过js进行上传

    
    function init(){
    
    	//提交
    	//获取oss签名参数
    var ret = get_signature()
    	//装配发送到oss文件上传url   
    var formData = new FormData(); 
    var file=$("#file").prop('files');
       
    var filepath=$("#file").val();
    var arr=filepath.split('\\');
    var filename=arr[arr.length-1];//===================获得本地文件名（需要你提供）
    
    if (ret == true)
    {
    	//装配oss签名参数
    	formData.append("name",filename);
    	formData.append("key", key);
    	formData.append("policy", policyBase64);
    	formData.append("OSSAccessKeyId", accessid);
    	formData.append("success_action_status", '200');
    	formData.append("callback", callbackbody);
    	formData.append("signature", signature);
    	formData.append("file",document.getElementById('file').files[0])//===================获得文件数据（需要你提供）
    }
    	//发送文件上传请求   
    	$.ajax({  
    url: host,  
    type: 'POST',  
    data: formData,  
    async: false,  
    contentType: false, //必须
    	processData: false, //必须
    success: function (returndata) {  
    alert(returndata);  
    },  
    error: function (returndata) {  
    alert(returndata);  
    }  
    });  
    }
    
    //发送到应用服务器 获取oss签名参数
    function send_request(){
    	var obj
    	$.ajax({
    		type : 'GET',
    		async : false,
    		url :'http://localhost/example_iuap_saas_file/oss
    		success : function(data){
    		obj=$.parseJSON(data);
    		} 
    	});
    	return obj;
    }
    
    //获取到签名参数后将参数填入变量待用
    function get_signature()
    {
    //可以判断当前expire是否超过了当前时间,如果超过了当前时间,就重新取一下.3s 做为缓冲
    expire = 0
    now = timestamp = Date.parse(new Date()) / 1000; 
    console.log('get_signature ...');
    console.log('expire:' + expire.toString());
    console.log('now:', + now.toString())
    if (expire < now + 3)
    {
    console.log('get new sign')
    var obj = send_request(obj)
    policyBase64 = obj['policy']
    accessid = obj['accessid']
    signature = obj['signature']
    expire = parseInt(obj['expire'])
    callbackbody = obj['callback'] 
    host =obj['host']
    key = obj['perfix']+'${filename}'
    return true;
    }
    return false;
    };
    
    $("#submit").attr("onclick","init();");


发送上传请求后会在上文中的Servlet类dopost方法处理oss回调，最终oss将结果发送回前台js的ajax回调。

**临时文件url与永久文件url**


oss文件系统能够通过getUrl方法返回文件的url，返回的url有临时和永久两种。该接口如下


public static String getUrl(BucketPermission permission,String fileName,int expired) 

三个参数为ossbucket的权限，文件名，过期时间。其中permission的值可以为BucketPermission.private、BucketPermission.read、BucketPermission.full。如果需要临时url的话permission值需要为private，并且传入expired值为过期时长。如果需要永久的文件url前提是上传文件bucket需要为read或full权限（不推荐使用），设置bucket权限可以在oss控制台(相关连接：<a href="https://help.aliyun.com/document_detail/31885.html?spm=5176.doc31886.6.101.vSWwVY">建立新的bucket</a>)，选择需要修改权限的bucket，Bucket属性标签有读写权限修改。在geturl时参数传入BucketPermission.read或者BucketPermission.full，expired可以为0。

**略缩图功能**

首先在oss控制台，选择存储略缩图文件的bucket，在左边的菜单中选择图片处理，选择开通图片处理。
然后通过下面的接口可以返回略缩图url：

public static String getImgUrl(BucketPermission permission,String fileName,int expired)

略缩图也是分为临时与永久url，方法与上文相同。通过该方法获得url以后附加类似@100h的参数产生略缩图。

例子：

将图缩略成高度为100，宽度按比例处理。

http://image-demo.img-cn-hangzhou.aliyuncs.com/example.jpg@100h

将图缩略成宽度为100，高度按比例处理。

http://image-demo.img-cn-hangzhou.aliyuncs.com/example.jpg@100w


**3.2:FastDfs**


组件提供的API，操作FastDfs

FastDfs API与使用oss系统时是相同的，所有的api都通过FileManager类静态调用，例子：FileManager.uploadFile("private","test.txt",content)。

在使用FastDfs模式时，bucketName参数代表文件在操作FastDfs系统存储的权限（private、read、full）该权限将存入FastDfs系统文件的metadata中，目前组件还没有对不同权限的文件进行访问限制处理，请等待以后的更新。

	//上传，参数（bucket名，文件名，上传文件字节数组），返回（上传文件的文件名）
    String uploadFile(String bucketName, String fileName,byte[] fileContent)
	

 	//下载，参数（bucket名，文件名），返回（下载文件的字节数组）
 	byte[] downLoadFile(String bucketName,String fileName) ;
	

	//删除，参数（bucket名，文件名），返回（删除是否成功标志）
    boolean deleteFile(String bucketName,String fileName)

    //获取文件url，参数（bucket名，文件名，过期时间），返回（上传文件的url）目前expired参数只支持阿里云oss私有bucket，其他系统可置为0
	String getUrl(String bucketName, String fileName,int expired)
	
	//获取图片url，参数（bucket名，文件名，过期时间），返回（图片的url）与getUrl的区别是文件名可以使用类似example.jpg@100h_100w这种格式产生略缩图，）目前expired参数只支持阿里云oss私有bucket，其他系统可置为0
	String getImgUrl(String bucketName,String fileName,int expired)

example测试类
    
    	/**
    	 * fastdfs上传下载删除测试
    	 * 测试前请设置application.properties的storeType=FastDfs
    	 * @throws Exception
    	 */
    	@Test
    	public void testFdfsUpload() throws Exception {
    		String filename;
    		//借用文件流生成文件的二进制数组
    		LocalClient client =LocalClient.getInstance();		
    		byte[] content =client.download("/etc/filetest/test.txt");
    		//fastdfs上传
    		filename=FileManager.uploadFile(null, "test.txt",content);
    		//fastdfs下载
    		byte[] downloadContent=FileManager.downLoadFile(null,filename);
    		
    		//用文件流将fastdfs下载到的文件二进制数组转化为文件
    		client.upload("/etc/filetest/fdfstest.txt","fdfstest.txt", downloadContent);
    		//删除
    		boolean flag=FileManager.deleteFile(null,filename);	
    		client.deleteFile("/etc/filetest/fdfstest.txt");
    		
    		System.out.println(filename);
    		System.out.println("删除状态"+flag);
    		Assert.isTrue(flag);
    	}

**略缩图功能**

fdfs使用略缩图功能时 需要搭建nginx提供文件访问服务

安装nginx
    
    1：安装前准备

	yum -y install zlib zlib-devel openssl openssl-devel pcre-devel gcc gcc-c++ autoconf automake gd-devel

	groupadd -r nginx

	useradd -s /sbin/nologin -g nginx -r nginx

	mkdir /var/tmp/nginx/client -pv

	touch /var/lock/nginx.lock

	2：nginx配置和安装

	cd /usr/local

	wget http://nginx.org/download/nginx-1.8.0.tar.gz

	tar -zxvf nginx-1.8.0.tar.gz

	cd nginx-1.8.0


	./configure \
	--prefix=/usr \
	--sbin-path=/usr/sbin/nginx \
	--conf-path=/etc/nginx/nginx.conf \
	--error-log-path=/media/disk1/nginx/logs/error.log \
	--pid-path=/var/run/nginx/nginx.pid \
	--lock-path=/var/lock/nginx.lock \
	--user=nginx \
	--group=nginx \
	--with-pcre=/usr/local/src/pcre-8.35 \
	--with-http_ssl_module \
	--with-http_flv_module \
	--with-http_gzip_static_module \
	--http-log-path=/media/disk1/nginx/logs/access.log \
	--http-client-body-temp-path=/media/disk1/nginx/client \
	--http-proxy-temp-path=/media/disk1/nginx/proxy \
	--http-fastcgi-temp-path=/media/disk1/nginx/fcgi \
	--with-http_stub_status_module \
	--with-poll_module \
	--with-http_realip_module \
	--add-module=/usr/local/src/ngx_cache_purge-2.3 \
	--add-module=/usr/local/src/fastdfs-nginx-module/src \
	--with-http_image_filter_module \
	--with-cc-opt=-Wno-error 

	nginx configure时需要注意带上fastdfs-nginx-module、http_image_filter_module等模块

	3：nginx安装检查

	检查是否启动
	ps -ef |grep nginx

	启动
	nginx -c /etc/nginx/nginx.conf

	查看80端口是否被监听
	netstat -tulnp |grep :80

4: nginx 服务配置

	将nginx文件复制到/etc/init.d下并修改权限

	chmod +x nginx

5：配置nginx.conf
	
    	location /g1/M00 {
    root /data/fdfs;
    ngx_fastdfs_module;
    
    }
    location  ~* /g1/M00/(.*)@(.*)$ {
    	set $img_name $1;
    	set $img_param $2;
    	
    	if ( $img_param ~* "([\d]+)h_([\d]+)w" ){
    	set $img_height $1;
    		set $img_width $2;
    	}
    	
    	if ( $img_param ~* "([\d]+)h" ){
    	set $img_height $1;
    		set $img_width -;
    	}
    	
    	if ( $img_param ~* "([\d]+)w" ){
    	set $img_height -;
    		set $img_width $1;
    	}
    	
    root /data/fdfs;
    ngx_fastdfs_module;
    
    rewrite /g1/M00/(.*)@(.*)$ /g1/M00/$1 break;
    	
    	image_filter resize $img_width $img_height;
    image_filter_buffer 5M;
    image_filter_jpeg_quality 80;
    image_filter_transparency on;
    
    proxy_cache content_image;
    proxy_cache_key $request_uri$is_args$args;
    proxy_cache_valid 200 304 12h;
    expires 30d;
    }


5：服务启动

	服务方式启动
	service nginx start

	停止
	service nginx stop

	重新加载
	service nginx reload

**接口调用**

然后通过下面的接口可以返回略缩图url：

public static String getImgUrl(BucketPermission permission,String fileName,int expired)

fileName后面附加类似@100h的参数产生略缩图。

例子：

将图缩略成高度为100，宽度按比例处理。

example.jpg@100h

将图缩略成宽度为100，高度按比例处理。

example.jpg@100w

将图缩略成高度为100，宽度为100。
example.jpg@100h_100w


**4.2:本地文件系统**

本地文件系统一般用于开发测试，不建议部署为生产系统

组件提供的API，操作本地文件系统


    //上传，参数（bucket名传null，文件名，上传文件字节数组），返回（上传文件的文件名）
    filename=FileManager.uploadFile(null,"test.txt", content);
	//下载，参数（bucket名传null，文件名），返回（下载文件的字节数组）
    byte[] content =client.download("/etc/filetest/test.txt");
    //删除，参数（bucket名传null，文件名），返回（删除是否成功标志）
    boolean flag=FileManager.deleteFile(null,filename);


example测试类


	/**
	 * 文件系统上传下载删除测试
	 * 测试前请设置application.properties的storeType=Local
	 * @throws Exception
	 */
	@Test
	public void testFSUpload() throws Exception {
			String filename;
			//下载并将文件转换为二进制数组
			//借用文件流生成文件的二进制数组
			LocalClient client =LocalClient.getInstance();		
			byte[] content =client.download("/etc/filetest/test.txt");
			
			//上传	
			filename=FileManager.uploadFile(null,"test.txt", content);
			//删除
			boolean flag=FileManager.deleteFile(null,filename);
			
			System.out.println(filename);
			System.out.println("删除状态"+flag);
			Assert.isTrue(flag);

	}
	
**5:更多详细的使用方法，请参考示例工程(DevTool/examples/example_iuap_file)**