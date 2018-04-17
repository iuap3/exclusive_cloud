# 日志组件 #

## 组件简介 ##
iUAP后台日志中，统一使用slf4j+logback的方式进行日志的记录和输出，通过slf4j对各个日志组件的桥接器，适配不同日志件对日志的输出。

同时，日志组件采用slf4j和MDC的结合，可以在日志中记录如用户编号，线程调用唯一标识等指定信息，方便问题的追踪，为运维管理中如ELK方案追踪日志信息提供线索。

## 配置和使用方式 ##
**1:在工程的classpath中加入logback的日志配置文件logback.xml,详细配置请参考示例工程**

日志的配置中，可以配置日志的输出级别，输出格式，自定义输出变量等，还可以指定具体包下的日志输出级别。

**2:工程中引入对iuap-log组件的依赖**

	<dependency>
		<groupId>com.yonyou.iuap</groupId>
		<artifactId>iuap-log</artifactId>
		<version>2.0.1-SNAPSHOT</version>
	</dependency>	

**3:类中声名Logger变量**

	private static final Logger logger = LoggerFactory.getLogger(LogTest.class);

**4:在业务代码中，调用logger的API输出各种级别的日志信息**

	/**
	 * 多参数示例
	 */
	@Test
	public void testMultiParamsLog() {
		logger.info("this is a {} log demo! level is：{}", "multi params", "info");
	}


**5:更多API操作和配置方式，请参考对应的示例工程(DevTool/examples/example_iuap_log)**