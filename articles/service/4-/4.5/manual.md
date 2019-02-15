# 动态调用配置

## 适用场景
没有对应的微服务接口类还需要调用微服务的情况下，可以通过动态调用方法调用微服务

## 微服务配置
和普通微服务一样需要配置application.properties文件

	access.key=**************
	access.secret==**************
	spring.application.name=ms-client
	spring.profiles.active=dev

## 动态调用工具类DynStub

动态调用工具类将方法的参数类型和参数转化为json字符串进行调用，在没有相应的类型时能自行拼接目标类的字符串。


	ExecuteResult resultexecute = DynStub.invokerExact(String serviceInterfaceName, String serviceUrl, String alias, String methodName, String[] parameterTypes, String[] arguments);

**参数列表**

<table>
    <thead>
        <tr>
            <th>参数</th>
            <th>描述</th>
            <th>示例</th>
        </tr>
    </thead>
    <tr>
      	 <td>String serviceInterfaceName </td>
		 <td>微服务调用接口</td>
		 <td>com.yonyou.cloud.ms.service.IStatisticsService</td>
    </tr>
    <tr>
      	<td>String serviceUrl</td>
		 <td>微服务应用编码（识别微服务的标识）</td>
		 <td>rpc-provider@c87e2267-1001-4c70-bb2a-ab41f3b81aa3由微服务应用名和应用providerid两部分组成</td>
    </tr>
    <tr>
      	<td>String alias</td>
		 <td>微服务别名</td>
		 <td>没有别名可以填null</td>
    </tr>
    <tr>
      	<td>String methodName</td>
		 <td>调用微服务接口方法的方法名</td>
		 <td>getInt</td>
    </tr>
    <tr>
      	<td>String[] parameterTypes</td>
		 <td>方法参数类型数组</td>
		 <td></td>
    </tr>
    <tr>
      	<td>String[] arguments</td>
		 <td>方法参数json串</td>
		 <td></td>
    </tr>
    <tr>
      	<td>返回值	ExecuteResult</td>
		 <td>有getValue()方法获得返回值对象</td>
		 <td></td>
    </tr>
</table>


## 调用示例

		//参数类型名数组
		String[] paramsTypes =new String[] {String.class.getName()}; 
		//参数json串	
		String[] params =new String[] {"222"};
		//微服务应用编码
		String service_appcode = "rpc-provider@c87e2267-1001-4c70-bb2a-ab41f3b81aa3";
		ExecuteResult result = DynStub.invokerExact("com.yonyou.cloud.ms.service.IStatisticsService",service_appcode, null, "getInt",paramsTypes,params);
		//动态调用得到的返回值都是ExecuteResult 需要将其中包裹的真正返回值取出来
		Object entity = result.getValue();
		ReturnClass final_result = (ReturnClass)entity;

**采用jason序列化调用**

		//调用之前加上
		RPCInvocationInfoProxy.setProto(CodecAdapterFactory.APPLICATION_JSON_UTF8_VALUE);

		ExecuteResult result = DynStub.invokerExact("com.yonyou.cloud.ms.service.IStatisticsService",service_appcode, null, "getInt",paramsTypes,params);
		
		//jason序列化需要对返回值进行转换，可以是使用工具类GsonUtilHolder，也可以自行选择jason序列化工具
		Object entity = result.getValue();
		String result_rawJsonStr = GsonUtilHolder.toJson(entity);
		Object real_result = GsonUtilHolder.fromJson(result_rawJsonStr, ReturnClass.class);
		ReturnClass final_result = (ReturnClass)real_result;		