# authSDK使用说明

开发者如需进行ak/as认证功能开发，微服务提供了authSDK功能加以实现。authSDK分为认证中心SDK加签客户端和认证中心SDK验签服务端两部分。

下文将分别演示加签客户端开发及验签服务端开发具体实现方法。

## 认证中心SDK加签客户端

第一步，引入maven依赖：

	<groupId>com.yonyou.cloud</groupId>
	<artifactId>auth-sdk-client</artifactId>
	<version>1.0.16-SNAPSHOT</version>

第二步，在Spring中初始化com.yonyou.cloud.auth.sdk.client.AuthSDKClient对象。

第三步，使用Spring容器AuthSDKClient的实例的execute(String, Map<String, List<String>>, Map<String, List<String>>, EnumRequestType) 方法进行请求发送。

第四步，处理响应结果。

### 加签客户端的使用示例

	@Test
	public void testAuthSDKClient() {
		String accessKey = "accessKeyTest";
		String accessSecret = "accessSecretTest";
		// 初始化AuthSDKClient对象, 单利模式或使用Spring初始化(推荐)
		AuthSDKClient authSDKClient = new AuthSDKClient(accessKey, accessSecret);
		// 请求URL
		String reqUrl = "http://localhost:9080/sample/api/someApi?a=1&q=11";
		// 请求参数Map, 可为空
		Map<String, List<String>> paramMap = Maps.newHashMap();
		paramMap.put("name", Lists.newArrayList("这是参数值"));
		paramMap.put("age", Lists.newArrayList("11", "44"));
		// 请求Header信息, 可为空
		Map<String, List<String>> headerMap = Maps.newHashMap();
		headerMap.put("headerTest1", Lists.newArrayList("testValue1"));
		headerMap.put("headerTest2", Lists.newArrayList("testValue2", "testValue3"));
		// 请求类型(get/post)
		EnumRequestType requestType = EnumRequestType.POST;
		// 请求结果
		HttpResult ret = authSDKClient.execute(reqUrl, paramMap, headerMap, requestType);
		// Http状态码和返回的内容字符串
		System.out.println(ret.getStatusCode() + "  " + ret.getResponseString());
	}

### 更新extendedAttributes扩展属性的值示例

	@Test
	public void testExtendedAttributes() {
		String accessKey = "accessKeyTest";//特权的sdk
		String accessSecret = "accessSecretTest";//特权的secret
		
		AuthSDKClient authSDKClient = new AuthSDKClient(accessKey, accessSecret);
		
		String reqUrl = "https://developer.yonyoucloud.com/accesscenter/api/v1/access/updateExtendedAttributes";

		Map<String, List<String>> paramMap = Maps.newHashMap();

		paramMap.put("accessKey", Lists.newArrayList("accessKeyTest"));//需要更改扩展属性的key
		paramMap.put("extendedAttributes", Lists.newArrayList("测试扩展"));//需要扩展属性的值

		EnumRequestType requestType = EnumRequestType.POST;

		HttpResult ret = authSDKClient.execute(reqUrl, paramMap, headerMap, requestType);
		// Http状态码和返回的内容字符串
		System.out.println(ret.getStatusCode() + "  " + ret.getResponseString());
	}

### 根据accessKey和accessSecret创建accessKey和accessSecret的接口(1.0.15-SNAPSHOT 版本以上可用):

	@Test
     public void testCreateAkAs() {
         String accessKey = "accessKeyTest";
         String accessSecret = "accessSecretTest ";
         AuthSDKClient authSDKClient = new AuthSDKClient(accessKey, accessSecret);
             // 请求地址
         String reqUrl = "https://developer.yonyoucloud.com/accesscenter/api/v1/access/createKey"; 
         Map<String, List<String>> paramMap = Maps.newHashMap();
         paramMap.put("userid", Lists.newArrayList("userid value")); // 用户ID
         paramMap.put("sysid", Lists.newArrayList("sysid value")); // 系统ID(可为空)
         EnumRequestType requestType = EnumRequestType.GET;
         HttpResult ret = authSDKClient.execute(reqUrl, paramMap, null, requestType);
         System.out.println(ret.getStatusCode() + "  " + ret.getResponseString());
     }

打印结果如下：

	200  {"success":"true","message":"创建accessKey成功.","detailMsg":           {"accessKey":此处为返回的ak,"accessSecret":此处为返回的as}}

### 各项Http超时设置(通过系统属性即通过JVM的-D参数设置)：

<table>
<tr>
    <th>配置项目名称</th>
    <th>默认值</th>
	<th>描述</th>
</tr>
<tr>
    <th>http.connectreq.timeout</th>
    <th>2000(毫秒)</th>
	<th>从ConnectionManager获取Connection超时时间
</th>
</tr>
<tr>
    <th>http.connect.timeout</th>
    <th>3000(毫秒)</th>
	<th>连接服务器超时时间</th>
</tr>
<tr>
    <th>http.socket.timeout</th>
    <th>4000(毫秒)</th>
	<th>请求发送成功后等待服务器返回的时间(读取数据时间)</th>
</tr>
</table>

## 认证中心服务端SDK验签服务端

第一步，引入maven依赖：

	<groupId>com.yonyou.cloud</groupId>
    <artifactId>auth-sdk-server</artifactId>
   	<version>1.0.20-SNAPSHOT</version>

第二步，在web.xml中配置验签Filter：

	<filter>
    	<filter-name>signVerifyFilter</filter-name>
    	<filter-class>com.yonyou.cloud.auth.sdk.service.filter.SignVerifyFilter</filter-class>
    </filter>
    <filter-mapping>
    	<filter-name>signVerifyFilter</filter-name>
        <!-- 验签路径 -->
    	<url-pattern>/api/*</url-pattern>
    </filter-mapping>

第三步，在classpath下配置application.properties文件中配置认证中心地址

	accesscenter_sign_url=http://xxx.com

线上环境地址：https://developer.yonyoucloud.com/accesscenter/api/v1/access/signkey

测试环境地址：http://172.20.27.27/accesscenter/api/v1/access/signkey

第四步，配置键accesscenter\_sign\_url

###读取顺序：

系统属性(tomcat的-D参数) > 环境变量> application.properties中配置的值 > 配置Filter时的init-param配置值.

### 在Filter中可获取的上下文属性方法为：

	com.yonyou.cloud.auth.sdk.service.context.SignVerifyThreadContextHolder.getXxx()

具体方法及说明如下：

<table>
<tr>
    <th>方法</th>
    <th>说明</th>
	<th>开始版本</th>
</tr>
<tr>
    <th>getProviderId()</th>
    <th>获取调用者的租户ID</th>
	<th>1.0.17-SNAPSHOT</th>
</tr>
<tr>
    <th>getAccessKey()</th>
    <th>获取调用者的AccessKey</th>
	<th>1.0.17-SNAPSHOT</th>
</tr>
<tr>
    <th>getTs()</th>
    <th>获取调用者的调用时间</th>
	<th>1.0.17-SNAPSHOT</th>
</tr>
<tr>
    <th>getExtendedAttributes()</th>
    <th>获取调用者的扩展属性</th>
	<th>1.0.18-SNAPSHOT</th>
</tr>
<tr>
    <th>getUserCode()</th>
    <th>获取调用者的UserCode</th>
	<th>1.0.17-SNAPSHOT</th>
</tr>
<tr>
    <th>getSysid()</th>
    <th>获取系统ID</th>
	<th>1.0.19-SNAPSHOT</th>
</tr>
</table>

## 常见问题

#### secretkey哪里获取呢？

在开发者中心右上角有个签名的生成, 可以获取ak和as

#### 验签是每次都要请求认证中心进行验证吗？

不是, 有JVM缓存, 一般10分钟验一次

#### 有没有接口，我们需要给别的应用发放authfile，需要分配secretkey？

不需要发放authfile, 每个在开发者中心的用户都可以申请ak/as, as不能泄露, 每个应用用它自己的ak/as加签/验签. 加签和验签的ak/as不要求使用同一个.

#### ak/as是什么?

ak是accessKey, as是accessSecret的简称.

#### 在ak/as正确的情况下, 为什么验签不过:

1.	加签使用的URL与服务提供者接收到的URL不一致, 如加签使用的是: https://xxx.com:80, 接收到的是:http://172.29.11.10:8080,常见于nginx转发造成的原URL丢失, 或schema不一致(https>http)

2.	ak/as错误

3.	请求url拼接错误, 如: http://10.11.11.1/abc/eee 多了一个斜杠如:http://10.11.11.1/abc//eee

4.	验签http和https的兼容重试的error信息可忽略,此信息已经在1.0.21-SNAPSHOT版本里删掉了, 此版本以下的版本都会打出兼容报错信息, 除非打出类似于以下日志才说明真的验签失败了:

	token verify faild using X-Forwarded-Proto 'null' and scheme 
	'http', url:http://172.20.28.31/confcenter/api/app/listAuthApp 
	error:token is invalid, details: 	
	com.auth0.jwt.exceptions.SignatureVerificationException: The 
	Token's Signature resulted invalid when verified using the 
	Algorithm: HmacSHA256 at com.auth0.jwt.algorithms.HMACAlgorithm.
	verify(HMACAlgorithm.java:49)