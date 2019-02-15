# 异常白名单插件

## 适用场景
在RPC调用过程中服务提供者抛出的异常既不在接口声明列表也不在异常白名单列表时, 服务消费者将收到包装后的RuntimeException异常堆栈;
如果服务消费者想要准确捕获某个服务提供者抛出的异常时, 有两种方式:

- 服务提供者将异常加到接口API声明列表中.

- 自定义异常白名单插件注册异常到异常白名单; 

如果服务提供者抛出的异常在接口声明列表 或 异常白名单时, 服务消费者将收到真实的异常而非包装后的RuntimeException堆栈. 


## 使用用法

以下说明了如何将异常加入异常白名单的步骤:

- 在服务端自定义类实现com.yonyou.cloud.mwclient.itf.IMwStarting接口, 在run(req,resp,chain)方法中设置异常白名单:
	
		package com.yonyou.cloud.ms.exception;

		import com.yonyou.cloud.exception.ExceptionWhiteListUtils;
		import com.yonyou.cloud.mwclient.itf.IMwStarting;
		import com.yonyou.cloud.plugin.InvokeChain;
		import com.yonyou.cloud.plugin.InvokeRequest;
		import com.yonyou.cloud.plugin.InvokeResponse;
		
		public class ExceptionWhiteListPlugin implements IMwStarting {
		
			@Override
			public void run(InvokeRequest request, InvokeResponse response, InvokeChain chain) {
				// 此处加入指定异常到异常白名单:
				ExceptionWhiteListUtils.putExWhiteList(MyException.class.getName());
				chain.run(request, response, chain);
			}
		
			@Override
			public int order() {
				return 10;
			}
		
			@Override
			public String getPluginName() {
				return getClass().getName();
			}
		}

- 在classpath下新建文件夹META-INF, 在META-INF下新建文件夹services, 在services文件夹下新建文件com.yonyou.cloud.mwclient.itf.IMwStarting, 并将自定义的异常插件的全类名写入此文件:

	- META-INF
		- services
			- com.yonyou.cloud.mwclient.itf.IMwStarting


其中com.yonyou.cloud.mwclient.itf.IMwStarting文件内容上述自定义异常插件的全类名:
	
	com.yonyou.cloud.ms.exception.ExceptionWhiteListPlugin

