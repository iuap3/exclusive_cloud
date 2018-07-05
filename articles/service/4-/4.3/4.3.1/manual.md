
# 同步RPC接口开发

## RPC接口声明
- 新建maven工程, 在pom.xml内引用maven依赖:

		<dependency>
			<groupId>com.yonyou.cloud.middleware</groupId>
			<artifactId>mwclient</artifactId>
			<version>5.0.0-RELEASE</version>
			<type>pom</type>
		</dependency>

- 在工程内新建接口类

- 在接口类上标注@com.yonyou.cloud.middleware.rpc.RemoteCall注解;

- 在接口类中增加API方法, 并增加相关文档注释方便使用者理解, 示例接口如下:

		package com.yonyou.cloud.ms.service;
		
		import com.wordnik.swagger.annotations.ApiOperation;
		import com.yonyou.cloud.middleware.rpc.RemoteCall;
		import com.yonyou.cloud.ms.constants.AppInfoConstant;
		import com.yonyou.cloud.mwclient.servmeta.annotation.ApiParam;
		import com.yonyou.cloud.mwclient.servmeta.annotation.ApiReturnValue;
		
		/**
		 * 普通的同步RPC调用的示例
		 */
		@RemoteCall(AppInfoConstant.APP_INF_PROVIDER)
		public interface IChartDataService {
			
			/**
			 * 远程获取echart的各种图表的json数据
			 * 
			 * @param type line/pie/bar
			 * @return echarts的json数据
			 */
			@ApiOperation("根据类型获取各种echarts图表的数据")
			public @ApiReturnValue(name="json 数据", description="echarts 需要的option数据，详情请参考echarts官网文档") String getChartDataByType(@ApiParam(name="图表类型", required=true, description="图表类型数据文件前缀，如bar、pie、line等", exampleValue="line") String type) throws Exception;
		}


- 对此API工程执行maven clean deploy,部署到maven仓库;

- 此API工程的服务实现/提供方引用此接口的maven依赖, 并做相应实现;

- 此API工程的服务使用/消费方引用此接口的maven依赖, 可调用提供的相关API.


