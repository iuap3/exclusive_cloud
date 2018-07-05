# 指定调用

## 简介
在微服务的开发和测试中，我们经常会需要调用特定地址或者版本的微服务，有两种方法可以实现这个目的，一是指定需要调用的微服务ip地址，二是为微服务设置版本，只调用响应版本的微服务。

## 指定ip
第一种指定调用微服务的方法是采用@RpcMock，调用微服务需要相应的微服务接口类，在接口类上增加@RpcMock注解并配置调用ip地址，就能够实现调用指定ip地址部署的微服务.

例子

	@RpcMock(serviceIp="192.168.1.1：8080")
	@RemoteCall("rpc-provider-test@c87e2267-1001-4c70-bb2a-ab41f3b81aa3")
	public interface ITestCompateOut {
		
		public String sendHello(String name);
	
	}

如果不能调通请加上应用context再试.

	@RpcMock(serviceIp="192.168.1.1：8080/rpc-provider")
	@RemoteCall("rpc-provider-test@c87e2267-1001-4c70-bb2a-ab41f3b81aa3")
	public interface ITestCompateOut {
		
		public String sendHello(String name);
	
	}

配置好微服务接口以后正常调用即可.

 
## 指定版本
指定微服务调用版本有两种方法，在配置文件中配置和使用线程上下文配置.


**配置文件**

在配置文件application.properties中加入配置
	
app.version=你的版本号

例子
	
	access.key=*********************
	access.secret=*********************
	spring.application.name=rpc-provider-test
	spring.profiles.active=dev
	app.version=3.0


**线程上下文**

通过线程上下文能够达到同样的效果,compateService 为微服务接口.

	@RequestMapping(value = "/compate")
	public String compate() throws Exception {
		
		//将调用版本设置为3.0
		RPCInvocationInfoProxy.setInstanceZone("3.0");
		
		String result = compateService.sendHello("test");
		return result;
	}

