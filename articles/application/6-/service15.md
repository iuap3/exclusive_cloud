#打印功能开发：
	
##1.1		环境配置：

		本地tomcat下，必须部署cloud_print_service,print_service, iuap-print。

cloud_print_service：云打印业务对象维护及打印模板设计的客户端代理服务。

print_service：本地打印服务客户端代理。

iuap-print：本地打印服务

![avatar](print1.png)
 

打印客户端认证文件authfile_print.txt这个要向云打印申请，并配置在 cloud_print_service和print_service 的application.properties中

application.properties如下示例
### 设计态云服务地址
print.server.name=http://172.20.8.30:8891
print.client.credential.path=/iuap/authfile_print.txt
UAP.AUTH.ALG=HMAC
UAP.KDF.PRF=HmacSHA1
print_token=Y0hKcGJuUmZkRzlyWlc0PQ==

	

## 1.2	模板设计：

### 1.2.1	加打印业务对象

在业务对象管理节点
 ![avatar](print2.png)


点击新增，跳转到业务对象设计界面

![avatar](print3.png)
 
		
### 1.2.2	增加打印模板

在打印模板管理节点
 ![avatar](print4.png)
点击新增模板，选择已经建好的业务对象，输入模板编码，保存，生成一个新打印模板
 ![avatar](print5.png)
### 1.2.3	设计打印模板
点击保存后，弹出新页面进行模板设计。
![avatar](print6.png)
设计模板的教程：http://print.yonyoucloud.com ，上面有视频教程。
### 1.2.4	模板分配
管理员登录应用平台，打开节点【应用资源分配】，应用与模板号关联。如下图：
![avatar](print7.png)
## 1.3	运行态：
### 1.3.1	前端代码开发：
增加打印按钮。

示例代码ygdemo_yw_info.html、ygdemo_yw_info.js

### 1.3.2	后端代码开发：
开发取数接口。

示例Ygdemo_yw_infoController. getDataForPrint()
			
