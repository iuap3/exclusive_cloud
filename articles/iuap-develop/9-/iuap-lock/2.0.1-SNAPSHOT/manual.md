# 分布式锁组件 #

## 组件简介 ##
iuap-lock组件是利用Zookeeper的强一致特性，提供的分布式锁功能，解决了多JVM中对共享资源加锁解锁的需求。本组件支持对共享资源的互斥锁的加锁解锁、线程内锁重入、批量加解锁等功能。支持对Zookeeper的单机和集群方式连接。

锁的逻辑实现的流程图如下：
<img src="/images/iuap_lock_flow.jpg"/>

# 使用说明 #

## 配置和使用方式 ##
**1:在属性文件中，配置分布式锁组件使用的Zookeeper的连接url和方式，支持单机方式、集群方式**
	
	#配置锁组件连接zookeeper的url
	zklock.url=127.0.0.1:2181
	#锁存活最大时间，单位秒，如果不配置，不强制删除，如果配置，加锁失败且已有的锁存活时间大于此值，强制删除
	zklock.maxlocktime=3600

**2:Spring的配置文件中，声明iuap-lock中连接池建立的配置信息的bean,根据项目需要调整参数值**

在web.xml中引入iuap-lock对应的spring配置文件，也可以将内部的配置合并到其他的spring配置文件中。

	<context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>
	  	    classpath*:/applicationContext-lock.xml
	    </param-value>
    </context-param>

主要是配置了连接池的信息，具体配置如下：

	<bean id="zkPoolConfig" class="org.apache.commons.pool2.impl.GenericObjectPoolConfig">
		<property name="maxTotal" value="100"/>
		<property name="maxIdle" value="20"/>
		<property name="maxWaitMillis" value="60000"/>
		
		<property name="timeBetweenEvictionRunsMillis" value="60000"/>
		<property name="numTestsPerEvictionRun" value="-1"/>
		<property name="minEvictableIdleTimeMillis" value="30000"/>
	</bean>

**3:工程中引入对iuap-lock组件的依赖**

	<dependency>
		<groupId>com.yonyou.iuap</groupId>
		<artifactId>iuap-lock</artifactId>
		<version>${iuap.modules.version}</version>
	</dependency>
${iuap.modules.version} 为平台在maven私服上发布的组件的version。


**4:初始化连接池**

在应用启动时，需要初始化连接池，可以下listener中定义，示例如下：

	public void contextInitialized(ServletContextEvent event) {
		WebApplicationContext wac = WebApplicationContextUtils.getWebApplicationContext(event.getServletContext());
		ContextHolder.setContext(wac);
		GenericObjectPoolConfig config = (GenericObjectPoolConfig)ContextHolder.getContext().getBean("zkPoolConfig");
		ZkPool.initPool(config);
	}


**5:代码中调用组件提供的API，操作分布式锁的加锁和解锁，完成业务**

	@Test
	public void testDisLockCodeExample() throws InterruptedException {
		String lockpath = "iuapdislockpath_test";
		boolean islocked = false;
		try {
			islocked = DistributedLock.lock(lockpath);
			if(islocked){
				System.out.println("加锁2成功，业务处理!");
			}
		} catch (Exception e) {
			System.out.println("加锁2操作异常!");
		} finally {
			if(islocked){
				DistributedLock.unlock(lockpath);
				System.out.println("解锁2成功!");
			} 
		}
	}

**6：Web项目中，可以配置过滤器方式，防止忘记解锁（不强制）**

	<filter>
	    <filter-name>lockfilter</filter-name>
	    <filter-class>com.yonyou.iuap.lock.filter.DistributedLockFilter</filter-class>
	</filter>
	<filter-mapping>
	    <filter-name>lockfilter</filter-name>
	    <url-pattern>/*</url-pattern>
	</filter-mapping>

	注意：url-pattern要按照自己加锁业务的请求路径进行映射，不要使用/*，以免拦截范围过大。

**7:更多API操作和配置方式，请参考分布式锁对应的示例工程(DevTool/examples/example_iuap_lock)**