# 指定调用


## 简介
在微服务的开发和测试中，我们经常会需要调用特定地址或者版本的微服务，有两种方法可以实现这个目的，一是指定需要调用的微服务ip地址，二是为微服务设置版本，只调用响应版本的微服务。

## 指定ip
第一种指定调用微服务的方法是采用@RpcMock，调用微服务需要相应的微服务接口类，在接口类上增加@RpcMock注解并配置调用ip地址，就能够实现调用指定ip地址部署的微服务

例子

	@RpcMock(serviceIp="192.168.1.1：8080")
	@RemoteCall("rpc-provider-test@c87e2267-1001-4c70-bb2a-ab41f3b81aa3")
	public interface ITestCompateOut {
		
		public String sendHello(String name);
	
	}

如果不能调通请加上应用context再试

	@RpcMock(serviceIp="192.168.1.1：8080/rpc-provider")
	@RemoteCall("rpc-provider-test@c87e2267-1001-4c70-bb2a-ab41f3b81aa3")
	public interface ITestCompateOut {
		
		public String sendHello(String name);
	
	}

配置好微服务接口以后正常调用即可


## 非注解方式制定调用ip
有些情况下调用的接口是由他人提供的，或者用户使用的是动态调用，这些情况下是无法对接口源码进行修改的。如果在这种情况中要使用mock制定调用ip就需要用配置文件的方式进行配置。

首先我们要修改application.properties文件，修改配置项client.usemock=true，如下：
	
	access.key=fKSAt9PCjHkbEKdq
	access.secret=ezpY9WjnBooLpnuST1pFp3ZLKEL5wS		
	spring.application.name=test-app
	spring.profiles.active=dev
	client.usemock=true

然后增加配置文件mock.properties到\src\main\resources，这个文件如下配置

	rpc-provider@35568e76-1ef1-4d77-b5cf-8fb66d2c8002@dev=192.168.1.1:8082

这里的配置分为以下几个部分

{appcode}@{providerId}@{env}={app.ip}


**appcode**为应用编码

**providerId**为所属用户的编号

**env**为应用的部署环境（分为开发、测试、灰度、生产等）

**app.ip**为需要制定的ip地址，某些情况下还要加上context

做完了上面的配置以后发送至rpc-provider@35568e76-1ef1-4d77-b5cf-8fb66d2c8002的dev环境的请求就会被分配到192.168.1.1:8082了

## 指定版本
指定微服务调用版本有两种方法，在配置文件中配置和使用线程上下文配置


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

通过线程上下文能够达到同样的效果

compateService 为微服务接口

	@RequestMapping(value = "/compate")
	public String compate() throws Exception {
		
		//将调用版本设置为3.0
		RPCInvocationInfoProxy.setInstanceZone("3.0");
		
		String result = compateService.sendHello("2222");
		return result;
	}

