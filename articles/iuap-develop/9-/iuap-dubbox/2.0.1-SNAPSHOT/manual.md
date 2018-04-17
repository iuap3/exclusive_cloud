#Dubbox扩展组件#

##业务需求##
在构建大型业务场景时候，往往需要按照服务进行拆分，以微服务的方式提供，以便于针对不同的服务进行不同的集群部署和扩展，服务的提供方式可以是多样的，如REST服务、WebService、dubbo服务等。

分布式服务的提供解决了服务之间的紧耦合问题和单独部署等问题，但在服务调用过程中的上下文传递时候需要特殊的考虑。例如，在服务式服务调用时候，希望将当前的用户登录信息、租户信息、线程号等传递到服务提供方，方便后续业务的日志输出、错误跟踪、信息持久化等，这就需要统一规范服务的上下文传递过程，来保证一致性。


##解决方案##
Dubbo是一个被国内很多互联网公司广泛使用的开源分布式服务框架，即使从国际视野来看应该也是一个非常全面的SOA基础框架。Dubbox是在Dubbo的基础上，封装了Restful调用以及做了一些jar的版本升级。

Dubbo解决了分布式服务的URL调用配置复杂，相互之间依赖关系难以整理，服务容量难以估算的问题。并结合自身的集群机制以及版本控制机制，提供了动态扩容/缩容和动态升级/降级的功能。

iuap-dubbox组件适配dubbo 2.8.4，基于spring 4.0.5 重新编译；同时支持服务调用过程中的上下文信息传递。上下文信息可以定制。

在对dubbox调用过程中，服务消费者向服务提供者传递上下文信息。传递的信息包括当前上下文中的用户信、线程信息等，还包括定制的日志输出信息。开发者可以在dubbo服务声明时指定过滤器，从而配置传递的上下文信息。


# 整体设计 #

## 依赖环境 ##
iuap-dubbox组件依赖dubbo 2.8.4版本，是基于spring 4.0.5 重新编译的，会随着IUAP的开发工具包发布，内部依赖方式如下：

	<dependency>
	    <groupId>com.alibaba</groupId>
	    <artifactId>dubbo</artifactId>
	    <version>2.8.4.RELEASE</version>
	    <exclusions>
	    	<exclusion>
	    		<artifactId>log4j</artifactId>
	    		<groupId>log4j</groupId>
	    	</exclusion>
	    </exclusions>
	</dependency>
	<dependency>
		<groupId>com.yonyou.iuap</groupId>
		<artifactId>iuap-generic</artifactId>
		<version>${iuap.modules.version}</version>
	</dependency>

iuap-dubbox组件依赖上述jar包，外部项目直接依赖iuap-dubbox即可：

	<dependency>
	  <groupId>com.yonyou.iuap</groupId>
	  <artifactId>iuap-dubbox</artifactId>
	  <version>${iuap.modules.version}</version>
	</dependency>


## 功能结构 ##

**基本概念**

- 服务提供者

    暴露服务的服务提供方

- 服务消费者

    调用远程服务的服务消费方

- 注册中心  

    服务注册与发现的注册中心，我们使用zookeeper作为示例注册中心。
<img src="/images/dubbox.png"/>

iuap-dubbox在原有dubbo服务的基础上，以Filter的方式进行扩展，增加上下文业务信息和日志信息的传递。

# 使用说明 #

##配置方式##
**1:在属性文件中，配置zookeeper连接地址、协议名称、负载策略等**

	dubbo.application.name=dubbox_provider
	dubbo.registry.address=zookeeper://localhost:2181
	dubbo.monitor.protocol=registry
	dubbo.protocol.name=dubbo
	dubbo.protocol.port=20880
	dubbo.service.loadbalance=consistenthash
	dubbo.log4j.file=logs/dubbox_provider.log
	dubbo.log4j.level=DEBUG

**2:增加dubbox对应的provider、customer的Spring配置文件**
	

- 1）provider示例：applicationContext-dubbo-provider.xml

		<!-- dubbo 配置 -->
		<dubbo:application name="${dubbo.application.name}"/>
		<dubbo:registry address="${dubbo.registry.address}"/>
		<dubbo:protocol name="${dubbo.protocol.name}" port="${dubbo.protocol.port}"/>
	
		<dubbo:monitor protocol="registry"></dubbo:monitor>
		<!-- 生产者声明 -->
		<bean id="dubboxProvider" class="uap.web.dubbox.DubboxProviderImplTest"/>
		<dubbo:service interface="uap.web.dubbox.DubboxProviderItfTest" ref="dubboxProvider" version="0.1" retries="0" timeout="35000" loadbalance="${dubbo.service.loadbalance}" filter="logcontext"/>


- 2）consumer示例：applicationContext-dubbo-consumer.xml

	    <!-- dubbo 配置 -->
	    <dubbo:application name="${dubbo.application.name}"/>
	    <dubbo:registry address="${dubbo.registry.address}" timeout="15000"/>
	    <dubbo:consumer check="false"/>
	
	    <!-- 消费者声明 -->
	    <dubbo:reference id="dubboxConsumer" interface="uap.web.dubbox.DubboxProviderItfTest" version="0.1" filter="logcontext"/>

**3:在服务消费者的工程上注入bean，调用服务**

**4:其他详细配置和参数，请参考示例工程(DevTool/examples/example\_dubbox\_consumer和example\_dubbox\_provider)和官网文档**

##工程样例##

- 启动Zookeeper，在DevTool的bin目录下，启动Zookeeper作为注册中心

- 启动服务提供者，导入iuap-dubbox-example-provider，以单元测试方式启动即可

- 模拟消费者调用，导入iuap-dubbox-example-consumer，单元测试方式运行TestDubboxConsumer中的方法

	
