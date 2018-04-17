#分布式缓存组件概述#

## 业务需求 ##

为了提高业务应用的性能，避免对数据库，磁盘等资源的频繁访问，应用中常常引入缓存技术，将需要访问的资源或初次调用后的结果放置到内存中，以保证应用的高并发支持。

内存缓存的实现方式可以是将数据放置在JVM的堆内存中，也可以借助三方中间件，将数据放置在如MemCached、Redis等堆外内存中。

JVM内存缓存的两个弊端：

- 1：大小受到JVM堆内存的限制，会引起频繁GC
- 2：集群部署时候，web应用间存在多份缓存，无法共享，更新需要互相通知

Redis针对数据结构、持久化、过期策略、分布式集群、数据备份和灾难恢复等方面相对MemCached有优势，所以iUAP平台采用Redis作为缓存中间件。Redis官方给出的优势列表如下：

- 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
- 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
- 原子 – Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行。
- 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。

操作Redis时，需要提供统一的建立连接，维护连接池，调用通用的API操作缓存数据，需要平台提供标准组件处理缓存。


## 解决方案 ##

iUAP平台采用Redis作为缓存中间件，实现对适当的业务数据、session cache等信息的缓存，提高系统的运行效率。

iuap-cache组件提供对分布式缓存中间件Redis的连接、池化、基本Java API封装，供其它产品或者项目简便的操作缓存数据。

iUAP缓存组件实现了对连接池的管理，支持主从方式和分片方式部署。分布式缓存架构保证了缓存服务的高可用性，支持写节点宕机后的主节点自动切换，持续提供可用服务。同时，平台提供模板类对缓存使用的API进行封装，方便业务开发快速的集成和使用。

iUAP缓存组件也提供对阿里云Redis数据库的适配。

# 整体设计 #

## 依赖环境 ##

组件采用Maven进行编译和打包发布，依赖Jedis的2.6.0版本和iUAP平台的一些基础组件如iuap-log，其对外提供的依赖方式如下：

	<dependency>
	  <groupId>com.yonyou.iuap</groupId>
	  <artifactId>iuap-cache</artifactId>
	  <version>${iuap.modules.version}</version>
	</dependency>

${iuap.modules.version} 为平台在maven私服上发布的组件的version。

## 部署结构 ##

Redis本身支持多种语言的客户端来连接，iuap-cache组件利用java语言通过Jedis客户端进行连接，建立连接池并支持哨兵方式的url。

Redis中间件通常是配合哨兵进行集群部署，一主多从的部署结构和连接的示意图如下： 

<img src="/images/redis_sentinel.png"/>

# 使用说明 #

## 组件包说明 ##

iuap-cache组件利用jedis客户端，在springside提供的对jedis的封装的基础上，提供使用简易url的方式创建连接池、兼容直连和哨兵方式连接，模板类支持Redis的分片。

## 组件配置 ##

**1:在属性文件中，配置redis的连接url，根据业务不同的需要，可以配置成单机方式、集群方式、分片方式等**

	单机方式示例：
	redis.url=direct://localhost:6379?poolName=mypool&poolSize=100
	
	集群示例：
	redis.url=sentinel://20.12.6.57:26379,20.12.6.58:26379,20.12.6.59:26379?poolName=mypool&masterName=mymaster&poolSize=100
	
	分片示例：
	redis.shardedurl=direct://20.12.6.57:6379,20.12.6.58:6379,20.12.6.59:6379?poolName=mypool&masterName=mymaster&poolSize=100

**2:Spring的配置文件中，声明iuap-cache中的bean**

	<bean id="redisPool" class="com.yonyou.iuap.cache.redis.RedisPoolFactory" scope="prototype" factory-method="createJedisPool">
		<constructor-arg value="${redis.url}" />
	</bean>
	
	<bean id="jedisTemplate" class="org.springside.modules.nosql.redis.JedisTemplate">
		<constructor-arg ref="redisPool"></constructor-arg>
	</bean> 
	
    <bean id="cacheManager" class="com.yonyou.iuap.cache.CacheManager">
        <property name="jedisTemplate" ref="jedisTemplate"/>
    </bean>	

**3:工程中引入对iuap-cache组件的依赖**

	<dependency>
		<groupId>com.yonyou.iuap</groupId>
		<artifactId>iuap-cache</artifactId>
		<version>${iuap.modules.version}</version>
	</dependency>	

**4:代码中调用组件提供的API，操作分布式缓存**

	/**
	 * 普通存取、删除测试,get,set,remove,exists
	 * 
	 * @throws Exception
	 */
	@Test
	public void testSimpleCache() throws Exception {
		String key = "testKey";
		cacheManager.set(key, "test");
		
		Assert.isTrue(cacheManager.exists(key));
		
		String cacheValue = (String)cacheManager.get(key);
		System.out.println(cacheValue);
		
		cacheManager.removeCache(key);
		
		Assert.isNull(cacheManager.get(key));
	}

**5:更多API操作和配置方式，请参考缓存对应的示例工程(DevTool/examples/example\_iuap\_cache)**

## 工程样例 ##

开发工具包DevTool中携带了对分布式缓存组件的示例工程，位置位于DevTool/examples/example_iuap_cache下，在IUAP_STUDIO中导入已有的Maven工程，可以将示例工程导入到工作区。示例工程中有较为完整的对iuap-cache组件的使用示例代码。
<img src="/images/cache_example.jpg"/>

## 开发步骤 ##

- 参考上述配置项，配置属性文件中的redis连接串，引入缓存对应的spring配置文件applicationContext-cache.xml，如果是web应用，修改web.xml如下：

		<context-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>
				classpath*:/applicationContext.xml,
				classpath*:/applicationContext-cache.xml,
				classpath*:/applicationContext-persistence.xml,
				classpath*:/applicationContext-shiro.xml
			</param-value>
		</context-param>
		<context-param>
			<param-name>spring-mvc-servlet-name</param-name>
			<param-value>springServlet</param-value>
		</context-param>
		<listener>
			<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
		</listener>

- 属性文件中配置的redis应该是服务器的redis地址，如果本机调试，可以启动DevTool中默认携带的redis，启动脚本位于DevTool->bin->startRedis.bat,双击运行，效果如下：
<img style="margin-left:25px;" src="/images/cache_redis.jpg"/>

- 注入在applicationContext-cache.xml中声明的bean cacheManager，如果应用中只有一个此类型的bean，则可以使用@Autowired注入，如果存在多个，则按照名称注入。业务开发时候，可以为不同的业务模块声明不同的cache区域，注册多份CacheManager和redisPool即可。

- iuap-cache组件默认使用的是java基础的序列化和反序列化方式，如果项目上需要替换更高性能的序列化方式，在声明CacheManager时候注入Serializer即可。

	    public void setSerializer(Serializer serializer) {
	        this.serializer = serializer;
	    }


## 常用接口 ##

- CacheManager

<table style="border-collapse:collapse">
	<thead>
		<tr>
			<th>方法名</th>
			<th>参数</th>
			<th>返回值</th>
			<th>说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>set(final String key, final T value)</td>
			<td>String key（缓存key）, T value（缓存对象，允许序列化）</td>
			<td>void</td>
			<td>放置键值对对应的缓存</td>
		</tr>
		<tr>
			<td>setex(final String key, final T value，final int timeout)</td>
			<td>String key（缓存key）, T value（缓存对象，允许序列化），int timeout（缓存的有效期）</td>
			<td>void</td>
			<td>放置键值对对应的缓存并设置缓存有效期，单位为秒</td>
		</tr>
		<tr>
			<td>expire(final String key, final int timeout)</td>
			<td>final String key（缓存key）, final int timeout（缓存的有效期）</td>
			<td>void</td>
			<td>修改缓存信息的有效期</td>
		</tr>
		<tr>
			<td>exists(final String key)</td>
			<td>final String key（缓存key）</td>
			<td>Boolean（是否存在的标志）</td>
			<td>判断键值为key的缓存是否存在</td>
		</tr>
		<tr>
			<td>get(final String key)</td>
			<td>final String key（缓存key）</td>
			<td>T extends Serializable（声明的返回类型对象）</td>
			<td>获取对应键值的缓存对象</td>
		</tr>
		<tr>
			<td>hget(final String key, final String fieldName)</td>
			<td>final String key（缓存Map的key），final String fieldName（Map下某个属性的key）</td>
			<td>T extends Serializable（声明的返回类型对象）</td>
			<td>获取HashMap中对应的子key的缓存值</td>
		</tr>
		<tr>
			<td>hmget(final String key, final String... fieldName)</td>
			<td>final String key（缓存Map的key）, final String... fieldName（Map下若干属性的key）</td>
			<td>List</td>
			<td>获取HashMap中对应的多个子key的缓存值集合</td>
		</tr>
		<tr>
			<td>hexists(final String key, final String field)</td>
			<td>final String key（缓存Map的key），final String field（Map下某个属性的key）</td>
			<td>Boolean（是否存在的标志）</td>
			<td>判断HashMap中对应的key是否存在</td>
		</tr>
		<tr>
			<td>hset(final String key, final String fieldName, final T value)</td>
			<td>final String key（缓存Map的key），final String fieldName（Map下某个属性的key），final T value（要放置的缓存对象）</td>
			<td>void</td>
			<td>放置HashMap中的属性和值</td>
		</tr>
		<tr>
			<td>removeCache(String key)</td>
			<td>String key（缓存的key）</td>
			<td>void</td>
			<td>删除key对应的缓存信息</td>
		</tr>
		<tr>
			<td>hdel(String key, String field)</td>
			<td>String key（缓存Map的key），String field（Map下某个属性的key）</td>
			<td>void</td>
			<td>删除Map下键值为filed的属性</td>
		</tr>
	</tbody>
</table>
