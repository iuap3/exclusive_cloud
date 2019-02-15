
# 配置说明
<br>

## Maven工程pom.xml引用配置:

	<dependency>
		<groupId>com.yonyou.cloud.middleware</groupId>
		<artifactId>mwclient</artifactId>
		<version>5.1.1-RELEASE</version>
		<type>pom</type>
	</dependency>


## application.properties配置

	# 从开发者中心申请的认证Key
	access.key=xxxxxxxxxxxxxxxx
	# 与上述认证key对应的认证秘钥
	access.secret=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
	
	# 应用名称, 应与开发者中心的应用CODE一致
	spring.application.name=xxxx
	# 应用部署的环境(开发:dev,测试:test,灰度:stage,线上:online)
	spring.profiles.active=dev
	
	#开发调试时指定IP调用的配置(默认为false), 
	client.usemock=false
	# 启用mock调用时针对某个调用者的具体IP指定
	appCode@providerId@profile=172.20.1.1 
	
	#指定微服务注册地址
	registry=172.20.17.4


## applicationContext.xml配置

### · 典型最小化配置

	<bean id="propertyConfigurer" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
	    <property name="locations">
	        <list>
	            <value>classpath*:/application.properties</value>
	        </list>
	    </property>
	    <property name="systemPropertiesMode" value="2"></property>
	</bean>
	
	<!-- 使用annotation 自动注册bean, 并保证@Required、@Autowired的属性被注入 -->
	<context:component-scan base-package="com.yonyou.cloud">
		<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
		<context:exclude-filter type="annotation" expression="org.springframework.web.bind.annotation.ControllerAdvice"/>
	</context:component-scan>



### · XML方式注册远程调用接口
在没有引用标注@RemoteCall注解的远程调用接口时注册远程调用接口, 适用于服务方没有发布API包的情况.
	
	<bean id="chartTypeService" class="com.yonyou.cloud.middleware.rpc.RPCBeanFactory">
	    <constructor-arg value="${remote-service.appCode}"></constructor-arg>
	    <constructor-arg value="${remote-service.providerId}"></constructor-arg>
	    <constructor-arg>
	    	<list>
	        	<value>${remote-service.fullQualifiedClassName}</value>
	        </list>
	    </constructor-arg>
	</bean>


### war包合并

## 新建工程合并war包
```xml
        <dependency>
            <groupId>com.yonyou.cloud.ms</groupId>
            <artifactId>rpc-client</artifactId>
            <version>5.1.1-RELEASE</version>
            <type>war</type>
        </dependency>
        <dependency>
            <groupId>com.yonyou.cloud.ms</groupId>
            <artifactId>rpc-provider</artifactId>
            <version>5.1.1-RELEASE</version>
            <type>war</type>
        </dependency>
```

## 添加配置文件覆盖war包中的配置文件

```properties
access.key=xxxxx
access.secret=xxxxxxxxx

spring.application.name=client-provider
spring.profiles.active=online

registry=http://172.20.52.128

jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://xxx.xx.xx.xxx:3306/rpc-provider?useUnicode=true&characterEncoding=utf-8&allowMultiQueries=true
jdbc.password=xxxx
jdbc.username=xxxx
```

## 新增parent配置文件

resources下新建META-INF文件夹用于存放parent文件
创建${appCode}-${providerId}.parent文件，比如
rpc-client-511-providerID.parent。
providerId可以省略为***rpc-client-511.parent***内容为

```
	parent=client-provider
```

***注意:***  
- 每个需要合并项目都需要建一个parent文件，放在与之有调用关系的服务下
- 如果有bean冲突需要手动解决

