#  业务日志
## 业务背景
1、业务日志组件可以解决此问题：业务逻辑和服务器日志逻辑“分离”。 
- 业务日志组件的目标：
- 日志的记录对业务方法尽量无侵入
- 尽最大可能不影响业务方法的性能
- 系统及日志模板配置简单
- 日志持久化(也称为导出日志)方式灵活
- 修改日志模板而不需要重启应用
事实上，要达到真正的无侵入是不可能的，业务日志组件对业务方法的侵入只不过是要在业务方法上加上一个注解。

2、业务与日志分离的原理是采用Spring AOP的拦截机制进行。
- 组件对使用@BusilogConfig(“业务方法别名”)注解的业务方法，通过Spring AOP进行拦截。当Spring察觉到被注解的业务方法被调用的时候，即可对此方法进行日志记录。 
- 本组件通过Groovy脚本配置日志模版，定义拦截的业务方法的参数。日志注解@BusilogConfig(“业务方法别名”)用来给需要日志记录的方法链接日志模版，不同的业务方法可以有不同的日志模板。同时对于日志内容的输出也是可配置的，用户可以自定义来实现对业务日志的持久化操作。
3、案例需求：预订单保存、删除触发业务日志消息，记录当前操作员、当前时间、预订单号等
## 环境配置
1、war包部署
应用平台安装盘中获取业务日志war包iuap-saas-busilog-service.war，部署到tomcat对应webapps下。
	 
1）.	auth.properties文件配置缓存地址、系统id。系统id默认配置为wbalone
 
2）.	jdbc.properties数据源配置
 
配置数据源地址，上图中为mysql配置示例，如果非动态数据源则配置为dataSource
 
3）.	sdk.properties配置sdk相关身份认证通道。对当前示例，我们需要配置autifile地址。
 
4）.	workbench-sdk.properties工作台配置
配置工作台地址、工作台的认证文件地址
 
经过以上4个步骤，业务日志配置成功
2、开发环境配置
1）.	pom.xml上增加如下配置，添加依赖的业务日志war包
		<!-- 业务日志 -->
		<dependency>
			<groupId>com.yonyou.iuap</groupId>
			<artifactId>iuap-busilog</artifactId>
			<version>3.1.0-RC001</version>
		</dependency>   

2）.	在路径DevTool\examples\example-iuap-busilog\src\main\resources中找到两个配置文件
 
并拷贝到当前项目的resource资源文件夹下
 
3）.	修改配置文件
因为业务日志使用了Mybatis的持久化方式，在这里与applicationContext_persistence里的配置需要合并，否则会有冲突。
如下图中busilog-applicationContext.xml中如下图部分注释掉。
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"       
       xsi:schemaLocation="
	   		http://www.springframework.org/schema/context
	   		http://www.springframework.org/schema/context/spring-context-3.2.xsd
	   		http://www.springframework.org/schema/beans
	   		http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
	   		http://www.springframework.org/schema/tx
            http://www.springframework.org/schema/tx/spring-tx-3.1.xsd
	   		http://www.springframework.org/schema/aop
	   		http://www.springframework.org/schema/aop/spring-aop-3.2.xsd
	   		http://www.springframework.org/schema/mvc 
      		http://www.springframework.org/schema/mvc/spring-mvc.xsd"
	   		default-autowire="byName" default-lazy-init="true">   		
	<context:annotation-config />
	<!-- 自动扫描	除了@Controller注解以外的注解 
	<context:component-scan base-package="com.yonyou.uap.busiog">
		<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller" />
	</context:component-scan>
	-->
	<!-- 加载组件配置文件 -->
<!-- 	<context:property-placeholder system-properties-mode="OVERRIDE"  ignore-unresolvable="true" location="classpath:busilog-systemConfig.properties" />
 -->
 
	<!-- 注入日志拦截器 -->
	<bean id="logInterceptor" class="com.yonyou.uap.ieop.busilog.BusiLogInterceptor">
		<property name="threadPoolTaskExecutor" ref="busilogThreadPoolTaskExecutor" />
		<property name="busiLogWriter">
			<bean class="${businessLogExporter}" />
		</property>
	</bean>

	<!-- 加了 proxy-target-class="true"，基于类的代理将被创建 -->
	<aop:config proxy-target-class="true">
		<aop:pointcut id="businessBehavior" expression="${pointcut}" />
		<aop:aspect id="logAspect" ref="logInterceptor">
			<aop:after-returning returning="result" method="logAfter" pointcut-ref="businessBehavior" />
			<aop:after-throwing method="afterThrowing" pointcut-ref="businessBehavior" throwing="error" />
		</aop:aspect>
	</aop:config>

	<!-- 异步线程池   -->
	<bean id="busilogThreadPoolTaskExecutor"
		class="org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor">
		<!-- 核心线程数 -->
		<property name="corePoolSize" value="${log.threadPool.corePoolSize}" />
		<!-- 最大线程数 -->
		<property name="maxPoolSize" value="${log.threadPool.maxPoolSize}" />
		<!-- 队列最大长度 >=mainExecutor.maxSize -->
		<property name="queueCapacity" value="${log.threadPool.queueCapacity}" />
		<property name="keepAliveSeconds" value="${log.threadPool.keepAliveSeconds}" />
		<!-- 线程池维护线程所允许的空闲时间 
		线程池对拒绝任务(无线程可用)的处理策略 -->
		<property name="rejectedExecutionHandler">
			<bean class="${log.threadPool.rejectedExecutionHandler}" />
		</property>
		
	</bean>
	
	<!-- mybatis的数据连接配置 -->
	
<!-- 	<bean id="busilogDataSource" class="org.apache.commons.dbcp.BasicDataSource"
		destroy-method="close">
		<property name="driverClassName" value="${driver}" />
		<property name="url" value="${url}" />
		<property name="username" value="${busilog_username}" />
		<property name="password" value="${busilog_password}" />
		初始化连接大小
		<property name="initialSize" value="${initialSize}"></property>
		连接池最大数量
		<property name="maxActive" value="${maxActive}"></property>
		连接池最大空闲
		<property name="maxIdle" value="${maxIdle}"></property>
		连接池最小空闲
		<property name="minIdle" value="${minIdle}"></property>
		获取连接最大等待时间
		<property name="maxWait" value="${maxWait}"></property>
	</bean>

	spring和MyBatis完美整合，不需要mybatis的配置映射文件
	<bean id="busilogSqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="busilogDataSource" />
		自动扫描mapping.xml文件
		<property name="mapperLocations" value="classpath*:/BusilogMapper.xml"></property>
	</bean>

	DAO接口所在包名，Spring会自动查找其下的类
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		  <property name="basePackage" value="com.yonyou.uap.busilog.dao" /> 
		<property name="sqlSessionFactoryBeanName" value="busilogSqlSessionFactory"></property>
	</bean>

	(事务管理)transaction manager, use JtaTransactionManager for global tx
	<bean id="busilogTransactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="busilogDataSource" />
	</bean>
	<tx:annotation-driven transaction-manager="busilogTransactionManager" proxy-target-class="true"/>  
	<bean id="exampleService" class="com.yonyou.uap.busiog.service.ExampleService"></bean>     --> 
</beans>
mybatis的扫描mapper，扫描包等都放入项目通用持久化配置applicationContext-persistence.xml
        <!-- 扫描basePackage下所有以@MyBatisRepository标识的 接口-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.yonyou.iuap.example.repository,com.yonyou.metadata.mybatis.dao,com.yonyou.uap.busilog.dao,com.yonyou.uap.billcode.repository,com.yonyou.iuap.billcode.repository"/>
        <!-- <property name="annotationClass" value="com.yonyou.iuap.persistence.mybatis.anotation.MyBatisRepository"/> -->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
    </bean>
增加了busilogMapper.xml配置文件扫描
 
4）.	busilog-systemConfig.properties配置
需要配置aop的插入点，在此处修改为service的包
 
## 数据库脚本
1、执行数据库脚本
CREATE TABLE busilog(
     id VARCHAR(40) NOT NULL,
     clientip VARCHAR(18),/*IP*/
     operuser VARCHAR(40),/*用户*/
	 logcategory  VARCHAR(40),/*日志分类*/
	 logcontent  VARCHAR(500),/*日志内容*/
	 sysid VARCHAR(40),/* 应用id*/
	 tenantid VARCHAR(40),/* 租户id*/
     logdate TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
     PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

2、代码示例
1）.	配置业务模板
BusinessLogConfig.groovy日志输出模板
具体的说明请参考业务日志的API
http://iuap.yonyou.com/page/service/book/platform3/index.html#/platform3/articles/iuap-develop/10-/iuap-busilog/3.0.0-release/manual.html

## 应用效果

说明：
业务日志组件提供了日志的数据库存储方式，支持替换为自定义的日志存储方式 
步骤如下：
开发人员通过实现接口com.yonyou.uap.ieop.busilog.writer.itf. 
IBusiLogWriter
在spring的properties文件中，将如下属性替换为自定义的实现类 
businessLogExporter=com.yonyou.uap.ieop.busilog.writer.BusiLogConsoleWriter


