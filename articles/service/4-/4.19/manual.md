# 自定义IRIS拦截插件

微服务支持用户自定义的IRIS拦截插件，包含客户端调用方和服务端接收方的插件编写，整个过程包含两个步骤分别是编写插件和注册插件。

下文将分别演示客户端调用方拦截插件和服务端接收方拦截插件的开发示例。

## 客户端调用方拦截插件

1. 编写插件，其中包名根据项目随意变动:
		
		package com.yonyou.xbiz.plugin;

		public class DubboBeforeInvokePlugin implements IBeforeInvoke {

			@Override
			public String getPluginName() {
				return this.getClass().getName();
			}

			@Override
			public int order() {
				return PluginConstants.PLUGIN_START_INDEX + 6;
			}

			@Override
			public void run(InvokeRequest req, InvokeResponse resp, InvokeChain chain) {

				RPCRequest rpcRequest = req.getRequest(RPCRequest.class);
				RemoteInvocation remoteInvocation = rpcRequest.getRemoteInvocation();

				//这里填写需要添加上下文名称testcode1，testcode2及对应值
				remoteInvocation.addAttribute("testcode1", true);
				remoteInvocation.addAttribute("testcode2", "true");


				chain.run(req, resp, chain);
    		}
		}

2. 注册插件：在src/main/resources下新建META-INF/services文件夹下新建com.yonyou.cloud.middleware.iris.IBeforeInvoke 文件，文件内容:

>com.xxx.BeforeInvokePlugin

## 服务端接收方拦截插件
1.	编写插件，其中包名根据项目随意变动

		package com.yonyou.xbiz.plugin;

		public class DubboFilterExecuteBefore implements IBeforeExecute {

    		@Override
    		public void run(InvokeRequest invokeRequest, InvokeResponse invokeResponse, InvokeChain invokeChain) {
        		RemoteInvocation remoteInvocation = invokeRequest.getAttribute(IrisConstans.METHOD_INVOCATION, RemoteInvocation.class);

				//这里编写对于调用方传入的属性的处理过程，依然以testcode1及testcode2为例
				boolean dubboFuse = null != remoteInvocation.getAttribute("testcode1") && (boolean) remoteInvocation.getAttribute("testcode1");
       		 	if (testcode1) {
            		MetaBeanFactory.destroyThreadLocalAware();
            		String token= null == remoteInvocation.getAttribute("testcode2") ? null : (String) remoteInvocation.getAttribute("testcode2");
            		AppContext.getToken();
            		if(null!=token) AppContext.setToken(token);
        		}
				//处理过程编写完成，下方内容保持不变。

        		invokeChain.run(invokeRequest, invokeResponse, invokeChain);
    		}

    		@Override
    		public int order() {
        		return PluginConstants.PLUGIN_START_INDEX;
    		}

    		@Override
    		public String getPluginName() {
        		return DubboFilterExecuteBefore.class.getName();
    		}
		}



2.	注册插件：在src/main/resources下新建META-INF/services文件夹下新建 com.yonyou.cloud.middleware.iris.IBeforeExecute 文件, 文件内容:

>com.yonyoucloud.uretail.middleware.filter.xxxBefore