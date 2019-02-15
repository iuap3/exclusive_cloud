# 异步调用

## 快速入门

下面以搭建示例工程为主线, 介绍EOS的使用. 
* 分为三个工程: <br>
  * API接口工程
  * 服务提供者(提供API接口和实现的服务方)
  * 服务消费者(调用API接口方).

### API接口工程

#### 引入maven依赖:

	<dependency>
		<groupId>com.yonyou.cloud.middleware</groupId>
		<artifactId>mwclient</artifactId>
		<version>5.1.1-RELEASE</version>
	</dependency>


#### 编写API接口类
* 编写API接口类, 此处以**com.mybiz.api.EOSDemoAPI**接口类为示例.<br>
* 在EOSDemoAPI接口上标注@com.yonyou.cloud.middleware.rpc.RemoteCall(**appCode@providerId**)注解, 表示此类暴露为微服务接口, 其中RemoteCall中的appCode@providerId表示此接口所归属的租户ID和应用Code.
* 在EOSDemoAPI接口类中编写API方法, 在API方法上标注: @com.yonyou.cloud.middleware.rpc.Async注解, 方法返回值须为void, 表示此API注册为EOS组件异步一致性API.

### 客户端服务端编写规范:

#### 引入maven依赖:

	<dependency>
		<groupId>com.yonyou.cloud.middleware</groupId>
		<artifactId>eos-spring-support</artifactId>
		<version>5.1.1-RELEASE</version>
	</dependency>
	<dependency>
		<groupId>com.yonyou.cloud.middleware</groupId>
		<artifactId>mwclient</artifactId>
		<version>5.1.1-RELEASE</version>
	</dependency>
	<dependency>
		<groupId>com.yonyou.cloud</groupId>
		<artifactId>auth-sdk-client</artifactId>
		<version>1.0.15-SNAPSHOT</version>
	</dependency>
	

#### 其他Maven依赖项:
依赖的Jdbc数据源组件用户可根据实际情况替换为DBCP或其他,依赖的SpringJdbc和Context版本用户可自定义, 推荐4.3.x及以上:

	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-jdbc</artifactId>
		<version>4.3.8.RELEASE</version>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
		<version>4.3.8.RELEASE</version>
	</dependency>
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>${mysql.version}</version>
	</dependency>
	<dependency>
		<groupId>org.apache.tomcat</groupId>
		<artifactId>tomcat-jdbc</artifactId>
		<version>${tomcat-jdbc.version}</version>
	</dependency>



#### Spring文件最简化配置:


	<bean id="eosConfig" class="com.yonyou.cloud.config.eos.EOSConfig">
	    	<property name="jdbcTemplate" ref="jdbcTemplate"/>
	    	<property name="transactionManager" ref="transactionManager"/>
	    	<property name="authSDKClient" ref="authSDKClient"/>
	</bean>


#### Spring文件典型配置:

	<!-- EOS 所需 jdbc组件 -->
	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
	        <property name="dataSource" ref="dataSource"></property>
	</bean>
	    
	<!--使用annotation定义事务 -->
	<tx:annotation-driven transaction-manager="transactionManager" proxy-target-class="true"/>
	    
	<!-- 事务配置 -->
	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	        <property name="dataSource" ref="dataSource"/>
	 </bean>
	
	<!--加签配置 -->
	<bean id="authSDKClient" class="com.yonyou.cloud.auth.sdk.client.AuthSDKClient">
		<constructor-arg name="accessKey" value="${prop.accessKey}" />  
		<constructor-arg name="accessSecret" value="${prop.accessSecret}" />  
	</bean>
	    
	<!--eos组件配置综合 -->
	<bean id="eosConfig" class="com.yonyou.cloud.config.eos.EOSConfig">
	    	<property name="jdbcTemplate" ref="jdbcTemplate"/>
	    	<property name="transactionManager" ref="transactionManager"/>
	    	<property name="authSDKClient" ref="authSDKClient"/>
	</bean>


#### EOS数据库建库脚本及数据结构，引用框架会自动建表，用户不必自己创建
	
	/*!40101 SET NAMES utf8 */;
	
	/*!40101 SET SQL_MODE=''*/;
	
	/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
	/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
	/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;
	CREATE DATABASE /*!32312 IF NOT EXISTS*/`eos` /*!40100 DEFAULT CHARACTER SET utf8 COLLATE utf8_bin */;
	
	USE `eos`;
	
	/*Table structure for table `eos_actionlog` */
	
	DROP TABLE IF EXISTS `eos_actionlog`;
	
	CREATE TABLE `eos_actionlog` (
	  `pk` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'DB主键,为性能提升',
	  `id` varchar(36) NOT NULL COMMENT '业务主键',
	  `src_provider_id` varchar(36) DEFAULT NULL COMMENT '源租户ID',
	  `dest_provider_id` varchar(36) DEFAULT NULL COMMENT '目标租户ID',
	  `own_provider_id` varchar(36) DEFAULT NULL COMMENT '事务所属租户ID',
	  `src_app_code` varchar(36) DEFAULT NULL COMMENT '调用来源应用编码',
	  `dest_app_code` varchar(36) DEFAULT NULL COMMENT '调用目标应用编码',
	  `own_app_code` varchar(36) DEFAULT NULL COMMENT '事务所属应用编码',
	  `interface_name` varchar(255) DEFAULT NULL COMMENT 'RPC服务接口名称',
	  `method_name` varchar(100) DEFAULT NULL COMMENT 'RPC服务接口的方法名',
	  `env` varchar(20) DEFAULT NULL COMMENT 'SDK中指定的调用环境',
	  `gtx_id` varchar(36) DEFAULT NULL COMMENT '全局事务ID',
	  `ptx_id` varchar(36) DEFAULT NULL COMMENT '父事务ID',
	  `tx_id` varchar(36) DEFAULT NULL COMMENT '本次调用事务ID',
	  `msg_id` varchar(36) DEFAULT NULL COMMENT '消息ID',
	  `err_log` varchar(10000) DEFAULT NULL COMMENT '错误事务简单日志',
	  `status` varchar(20) DEFAULT NULL COMMENT '事务状态编码（对应枚举类）',
	  `sync_direction` varchar(20) DEFAULT NULL COMMENT '同步方向，sdk和云端同步时使用',
	  `create_time` bigint(20) DEFAULT NULL COMMENT '错误事务初始入库时间，不能修改',
	  `update_time` bigint(20) DEFAULT NULL COMMENT '更新时间，每次修改状态或者字段时候更新',
	  `version` int(11) DEFAULT NULL COMMENT '乐观锁version',
	  PRIMARY KEY (`pk`),
	  UNIQUE KEY `UQ_ID` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='EOS 异步调用错误日志实体类，一条错误日志代表一次错误的异步调用事务';
	
	/*Table structure for table `eos_mqerror` */
	
	DROP TABLE IF EXISTS `eos_mqerror`;
	
	CREATE TABLE `eos_mqerror` (
	  `pk` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'DB主键,为性能提升',
	  `id` varchar(36) NOT NULL COMMENT '业务错误日志主键',
	  `logtype` varchar(20) DEFAULT NULL COMMENT '日志类型(0:发送日志,1:接收日志)',
	  `mqid` varchar(100) DEFAULT NULL COMMENT '消息id',
	  `errlog` varchar(10000) DEFAULT NULL COMMENT '错误日志消息',
	  `ts` bigint(20) DEFAULT NULL COMMENT '当前时间',
	  `status` varchar(20) DEFAULT NULL COMMENT '未归档/已归档',
	  PRIMARY KEY (`pk`),
	  UNIQUE KEY `UQ_ID` (`id`),
	  KEY `IDX_MQID` (`mqid`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;
	
	/*Table structure for table `eos_mqrecv_error` */
	
	DROP TABLE IF EXISTS `eos_mqrecv_error`;
	
	CREATE TABLE `eos_mqrecv_error` (
	  `pk` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'DB主键,为性能提升',
	  `id` varchar(36) NOT NULL COMMENT 'MQID-UUID-主键',
	  `txid` varchar(36) DEFAULT NULL COMMENT '事务ID-UUID',
	  `ptxid` varchar(36) DEFAULT NULL COMMENT '父事务ID-UUID',
	  `gtxid` varchar(36) DEFAULT NULL COMMENT '全局事务ID-UUID',
	  `srcqueue` varchar(100) DEFAULT NULL COMMENT '发送队列名称=应用编码-租户ID-环境',
	  `destqueue` varchar(100) DEFAULT NULL COMMENT '接收队列名称=应用编码-租户ID-环境',
	  `content` varchar(20000) DEFAULT NULL COMMENT '消息内容',
	  `createtime` bigint(20) DEFAULT NULL COMMENT '创建时间',
	  `updatetime` bigint(20) DEFAULT NULL COMMENT '更新时间',
	  `status` varchar(20) DEFAULT NULL COMMENT '状态(0:失败,1成功)',
	  `sync` varchar(20) DEFAULT NULL COMMENT '同步状态(0未同步,1已同步)',
	  PRIMARY KEY (`pk`),
	  UNIQUE KEY `UQ_ID` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;
	
	/*Table structure for table `eos_mqrecv_success` */
	
	DROP TABLE IF EXISTS `eos_mqrecv_success`;
	
	CREATE TABLE `eos_mqrecv_success` (
	  `pk` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'DB主键,为性能提升',
	  `id` varchar(36) NOT NULL COMMENT '业务MQID-UUID-主键',
	  `txid` varchar(36) DEFAULT NULL COMMENT '事务ID-UUID',
	  `ptxid` varchar(36) DEFAULT NULL COMMENT '父事务ID-UUID',
	  `gtxid` varchar(36) DEFAULT NULL COMMENT '全局事务ID-UUID',
	  `srcqueue` varchar(100) DEFAULT NULL COMMENT '发送队列名称=应用编码-租户ID-环境',
	  `destqueue` varchar(100) DEFAULT NULL COMMENT '接收队列名称=应用编码-租户ID-环境',
	  `content` varchar(20000) DEFAULT NULL COMMENT '消息内容',
	  `createtime` bigint(20) DEFAULT NULL COMMENT '创建时间',
	  `updatetime` bigint(20) DEFAULT NULL COMMENT '更新时间',
	  PRIMARY KEY (`pk`),
	  UNIQUE KEY `UQ_ID` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;
	
	/*Table structure for table `eos_mqsend_error` */
	
	DROP TABLE IF EXISTS `eos_mqsend_error`;
	
	CREATE TABLE `eos_mqsend_error` (
	  `pk` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'DB主键,为性能提升',
	  `id` varchar(36) NOT NULL COMMENT '主键-UUID',
	  `txid` varchar(36) DEFAULT NULL COMMENT '事务ID-UUID',
	  `ptxid` varchar(36) DEFAULT NULL COMMENT '父事务ID-UUID',
	  `gtxid` varchar(36) DEFAULT NULL COMMENT '全局事务ID-UUID',
	  `srcqueue` varchar(100) DEFAULT NULL COMMENT '发送队列名称=应用编码-租户ID-环境',
	  `destqueue` varchar(100) DEFAULT NULL COMMENT '接收队列名称=应用编码-租户ID-环境',
	  `content` varchar(20000) DEFAULT NULL COMMENT '消息内容',
	  `createtime` bigint(20) DEFAULT NULL COMMENT '创建时间',
	  `updatetime` bigint(20) DEFAULT NULL COMMENT '更新时间',
	  `status` varchar(20) DEFAULT NULL COMMENT '状态(0失败,1成功)',
	  `sync` varchar(20) DEFAULT NULL COMMENT '同步状态(0未同步,1已同步)',
	  PRIMARY KEY (`pk`),
	  UNIQUE KEY `UQ_ID` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;
	
	/*Table structure for table `eos_mqsend_success` */
	
	DROP TABLE IF EXISTS `eos_mqsend_success`;
	
	<!--此表采用了分表策略，默认创建好十天内的表，表名后缀为日期，数据中gitd字段前八位为年月日-->
	CREATE TABLE `eos_mqsend_success_20181209` (
	  `pk` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'DB主键(为性能提升)',
	  `id` varchar(36) NOT NULL COMMENT '业务主键-UUID',
	  `txid` varchar(36) DEFAULT NULL COMMENT '事务ID-UUID',
	  `ptxid` varchar(36) DEFAULT NULL COMMENT '父事务ID-UUID',
	  `gtxid` varchar(36) DEFAULT NULL COMMENT '全局事务ID-UUID',
	  `srcqueue` varchar(100) DEFAULT NULL COMMENT '发送队列名称=应用编码-租户ID-环境',
	  `destqueue` varchar(100) DEFAULT NULL COMMENT '接收队列名称=应用编码-租户ID-环境',
	  `content` varchar(20000) DEFAULT NULL COMMENT '消息内容',
	  `createtime` bigint(20) DEFAULT NULL COMMENT '创建时间',
	  `updatetime` bigint(20) DEFAULT NULL COMMENT '更新时间',
	  `status` varchar(20) NOT NULL COMMENT '发送状态(0待发送,1发送中,2发送成功)',
	  PRIMARY KEY (`pk`),
	  UNIQUE KEY `UQ_ID` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;
	
	/*Table structure for table `eos_mqsend_success_bak` */
	
	DROP TABLE IF EXISTS `eos_mqsend_success_bak`;
	
	CREATE TABLE `eos_mqsend_success_bak` (
	  `pk` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '伪主键(为性能提升)',
	  `id` varchar(36) NOT NULL COMMENT '主键-UUID',
	  `txid` varchar(36) DEFAULT NULL COMMENT '事务ID-UUID',
	  `ptxid` varchar(36) DEFAULT NULL COMMENT '父事务ID-UUID',
	  `gtxid` varchar(36) DEFAULT NULL COMMENT '全局事务ID-UUID',
	  `srcqueue` varchar(100) DEFAULT NULL COMMENT '发送队列名称=应用编码-租户ID-环境',
	  `destqueue` varchar(100) DEFAULT NULL COMMENT '接收队列名称=应用编码-租户ID-环境',
	  `content` varchar(20000) DEFAULT NULL COMMENT '消息内容',
	  `createtime` bigint(20) DEFAULT NULL COMMENT '创建时间',
	  `updatetime` bigint(20) DEFAULT NULL COMMENT '更新时间',
	  `status` varchar(20) NOT NULL COMMENT '发送状态(0待发送,1发送中,2发送成功)',
	  PRIMARY KEY (`pk`),
	  UNIQUE KEY `UQ_ID` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;
	
	/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
	/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
	/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;



#### 客户端服务端编写规范

- 搭建客户端和服务端maven工程, 此处命名客户端为**my-eos-client**, 命名服务端为**my-eos-service**.
- 客户端和服务端所需maven依赖及spring配置参见以上部分, 且引入my-eos-api对应的maven依赖.
- 客户端使用spring方式注入使用my-eos-api的EOSDemoAPI接口, 并在调用接口的方法上标注@Transactional的注解或使用声明式事务方式.
- 服务端实现my-eos-api的EOSDemoAPI接口.
- 访问开发者中心, 新建名称的客户端和服务端微服务(新建流程参见开发者中心使用文档,在此不再赘述)
- 新建完成后配置服务端的RabbitMQ地址(客户端不用配置), 进入开发者中心的左侧菜单 >> 微服务 >> 服务管理 >> 事务管理(标签页) >> 消息队列配置 >> EOS映射地址(输入框), 填写部署的RabbitMQ实例地址, 点击保存, 完成应用对应的MQ配置; RabbitMQ地址格式为以逗号分割的IP:Port, 如线上环境提供的公共RabbitMQ地址为: 			10.3.15.53:5672,10.3.15.54:5672.
- 在application.properties配置文件中配置好accessKey,accessSecret,spring.application.name,spring.profiles.active等属性(配置参考微服务搭建文档/示例工程).
- 根据以上数据库脚本建立数据库.
- 启动应用, 调试成功后部署到开发者中心.



## 自建RabbitMQ:

EOS依赖RabbitMQ组件, 需启用RabbitMQ的Http认证插件, 由用友云提供Http认证, 认证地址指向: **developer.yonyoucloud.com**

### 以镜像方式搭建:

* docker环境安装:
* 
		yum install -y docker.x86_64
		vim /etc/docker/daemon.json  输入内容并保存:
		{ "insecure-registries":["dockerhub.yonyou.com"] }
		systemctl start docker

* docker镜像名称为: 
		
		公司内网镜像名称: 10.3.15.191:5000/eos-auth-rabbitmq:v1
		公有仓库镜像名称: dockerhub.yonyou.com/cloud/eos-rabbitmq:v1

* 启动实例命令为(其中环境变量RABBITMQ\_AUTH\_SERVER为RabbitMQ的认证服务器地址):
		
		docker run -d --name rabbitmq_eos -p 15672:15672 -p 5672:5672 -e RABBITMQ_NODENAME=rabbitmq_eos -e RABBITMQ_AUTH_SERVER=developer.yonyoucloud.com dockerhub.yonyou.com/cloud/eos-rabbitmq:v1



### 直接安装方式搭建:
- yum install -y rabbitmq
- 修改/etc/rabbitmq/enabled\_plugins文件,启用http认证:

		[rabbitmq_management,rabbitmq_auth_backend_http].

- 配置认证地址, 以指向线上环境的/etc/rabbitmq/rabbitmq.conf配置为例:

		loopback_users.guest = false
		listeners.tcp.default = 5672
		hipe_compile = true
		management.listener.port = 15672
		management.listener.ssl = false
		
		auth_backends.1 = http
		
		auth_http.user_path     = http://developer.yonyoucloud.com/eos-mq-auth/auth/user
		
		auth_http.vhost_path    = http://developer.yonyoucloud.com/eos-mq-auth/auth/vhost
		
		auth_http.resource_path = http://developer.yonyoucloud.com/eos-mq-auth/auth/resource
		
		auth_http.topic_path    = http://developer.yonyoucloud.com/eos-mq-auth/auth/topic

- 如需更改数据和日志文件目录, 更改/etc/rabbitmq/rabbitmq-env.conf内容以指向具体的目录(选配项):

		MNESIA_BASE=/data/rabbitmq/datas
		LOG_BASE=/data/rabbitmq/logs


## EOS控制台

**客户端在发送调用MQ失败或者服务端在接收处理MQ调用时失败, 会通过在EOSConfig的eosCenterUrl上报到对应的开发者中心, EOS控制台可以方便的查看和检索处理失败的MQ, 并根据实际情况作出重试或忽略操作.**


- EOS控制台地址: 进入开发者中心的左侧菜单 >> 微服务 >> 服务管理 >> 事务管理(标签页)
- 检索: 根据页面上的对应字段进行检索查询.
- 重试: 点击某一条消息进行重试, 重试命令会发送到客户端进行此业务消息的重试, 待客户端重试完成后会将重试结果上报到控制台.
- 忽略: 点击某一条消息进行忽略, 忽略命令会发送到客户端, 客户端将将此业务消息标记为忽略状态.
- 批量忽略(此操作慎用): 步骤为: 填写对应的检索条件, 点击"批量忽略"按钮, 将会批量忽略匹配检索条件的所有业务消息.

## EOS SDK提供的API
**EOS对外提供了三个API, 方便业务系统对EOS错误消息处理的扩展.**

- API类名称: **com.yonyou.cloud.eos.core.service.EosApiService**
- API-1: 根据查询条件获取发送/接收失败的业务消息:

		com.yonyou.cloud.eos.core.service.EOSApiService.findActionLogApi(String, String, EnumActionLogStatus, int, int, boolean):
		查询条件分别为: 接口名称, 方法名称, 日志状态(异常,重试中,重试锁定,重试失败,重试成功), 分页参数(offset, rows), 是否启用模糊查询.


- API-2: 根据消息ID获取发送失败的消息详情:  

		com.yonyou.cloud.eos.core.service.EOSApiService.getMQSendByIdApi(String)
		参数为消息ID


- API-3: 根据消息ID获取接收失败的消息详情:  

		com.yonyou.cloud.eos.core.service.EOSApiService.getMQRecvByIdApi(String)
		参数为消息ID





## 异步组件对事件中心的支持:
**对事件中心的支持实质是仅保留了发送MQ消息的功能.**

- 修改EOSConfig的配置:

		<bean id="eosConfig" class="com.yonyou.cloud.config.eos.EOSConfig">
			<property name="jdbcTemplate" ref="jdbcTemplate"/>
			<property name="transactionManager" ref="transactionManager"/>
			<property name="authSDKClient" ref="authSDKClient"/>
			<property name="forceMQSend" value="true"/>
			<property name="enableMQRecv" value="false"/>
			<property name="syncWithCloud" value="false"/>
			<!-- ConsoleEOSSender需要实现接口:com.yonyou.cloud.config.eos.IEOSender -->
			<property name="senderClassName" value="com.yonyou.cloud.eos.component.ConsoleEOSSender"/>
		</bean>

- 在异步调用消息保存到DB中供EOS组件扫描发送, 即 保存异步调用信息到eos_mqsend_success表.

- 保存异步调用消息到表中列值的规范: 

		列txid,gtxid,为UUID生成, ptxid为null.
		srcqueue为发送业务端的AppCode@ProviderId@Env
		destqueue为接收处理业务端的AppCode@ProviderId@Env
		content为消息内容, 接收端收到后会调用IEOSender实现类中sendMQ方法, 并传入此消息对应的实体类:EOSMessage.
		createTime为创建时间, updatetime为更新时间.
		status为固定字符串内容为:waitsend


## EOS详细配置项详解(EOSConfig):

EOS配置项有许多, 可根据实际业务情况稍加调整.

**计划任务表达式**支持数字和Quartz的Cron表达式两种配置方式,其中数字方式表示定时任务的执行间隔时间(以毫秒为单位).

<table style="text-align:left;">
    <tr>
        <th>配置项名称</th>
        <th>是否必配项(空为默认值)</th>
        <th>配置项说明</th>
        <th>默认值(空为null)</th>
    </tr>
    <tr>
        <td>jdbcTemplate</td>
        <td>√</td>
        <td>数据库访问组件</td>
        <td></td>
    </tr>
    <tr>
        <td>transactionManager</td>
        <td>√</td>
        <td>Spring事务组件</td>
        <td></td>
    </tr>
    <tr>
        <td>authSDKClient</td>
        <td>√</td>
        <td>加签SDK客户端</td>
        <td></td>
    </tr>
    <tr>
        <td>eosCenterUrl</td>
        <td></td>
        <td>EOS异常上报控制台地址, 按照以下优先级获取非空值: 用户配置的值 > 属性文件/系统属性/系统环境变量中的名称为"registry"的值:${registry}/eos-console/ > 线上地址:https://developer.yonyoucloud.com/eos-console/</td>
        <td></td>
    </tr>
    <tr>
        <td>discardedComponentTtl</td>
        <td></td>
        <td>更换MQ后原MQ监听/发送<br>组件在销毁前的保留时长</td>
        <td>1000L * 60 * 60 * 24(ms)</td>
    </tr>
    <tr>
        <td>listenerThreadCount</td>
        <td></td>
        <td>MQ监听器线程池数量</td>
        <td>值为部署机器的CPU个数</td>
    </tr>
    <tr>
        <td>mqSendGroupConcurrentCount</td>
        <td>除非自定义发送器且对<br>多线程支持良好(无影响<br>性能锁), 否则不建议配置此项</td>
        <td>发送MQ时的并发线程数, <br>对性能影响大</td>
        <td>1</td>
    </tr>
    <tr>
        <td>innodbLockWaitTimeOut</td>
        <td></td>
        <td>innodb行锁超时时间, 在发送MQ时DB锁读取超时时长.</td>
        <td>5(s)</td>
    </tr>
    <tr>
        <td>maxSendTryErrCount</td>
        <td></td>
        <td>发送MQ时可容忍异常次数,超过配置次数<br>会置为异常状态并上报控制台处理.</td>
        <td>5</td>
    </tr>
    <tr>
        <td>maxRecvTryErrCount</td>
        <td></td>
        <td>接收处理MQ时可容忍异常次数,超过<br>配置次数会置为异常状态并上报控制台处理.</td>
        <td>5</td>
    </tr>
    <tr>
        <td>batchSendMQCount</td>
        <td></td>
        <td>每次批量从DB中获取MQ记录的条数,取出后放入JVM待发送队列中的其中一批数据.</td>
        <td>30000</td>
    </tr>
    <tr>
        <td>batchSyncMQCount</td>
        <td></td>
        <td>批量同步/移动MQ条数, 用于同步发送表(MQSend)中的异常消息到异常处理表(ActionLog)、同步异常处理表消息到云端、 同步本地已重试/忽略的异常消息表到云端</td>
        <td>2000</td>
    </tr>
    <tr>
        <td>batchResetCount</td>
        <td></td>
        <td>每次批量重置MQ状态条数, 用于<br>发送超时状态重置(发送中>待发送)、<br>重置同步发送处理失败的消息到异常处理表(ActionLog)时的同步状态(同步中>未同步)、<br>重置同步接收处理失败的消息到异常处理表(ActionLog)时的同步状态(同步中>未同步)、<br>重置异常消息重试状态(重试锁定>重试中)、<br>异常消息云端上传状态(上传中>待上传)</td>
        <td>30000</td>
    </tr>
    <tr>
        <td>mqSuccessQueueSize</td>
        <td></td>
        <td>发送MQ的队列长度,队列中每个元素是一个集合, 集合大小为batchSendMQCount属性定义的.</td>
        <td>10</td>
    </tr>
    <tr>
        <td>mqActionLogQueueSize</td>
        <td></td>
        <td>异常消息处理(ActionLog)的队列长度,队列中每个元素是一个集合, 集合大小为batchSendMQCount属性定义的.</td>
        <td>10</td>
    </tr>
    <tr>
        <td>recvPrefetchCount</td>
        <td></td>
        <td>监听器每次从RabbitMQ服务器批量获取的消息数量(经压测,单根连接最大阻塞消息数量为8700, 最大阻塞消息大小为9.2MB)</td>
        <td>7500(条)</td>
    </tr>
    <tr>
        <td>cronForSendMQ</td>
        <td></td>
        <td>发送MQ的计划任务表达式</td>
        <td>5000(ms)</td>
    </tr>
    <tr>
        <td>cronForRecvMQ</td>
        <td></td>
        <td>重试接收处理控制台下发重试命令的异常业务消息计划任务表达式</td>
        <td>5000(ms)</td>
    </tr>
    <tr>
        <td>cronForResetMQStatus</td>
        <td></td>
        <td>重置MQ状态的计划任务表达式</td>
        <td>0 0 2 * * ?</td>
    </tr>
    <tr>
        <td>cronForMoveData</td>
        <td></td>
        <td>定时迁移数据计划任务表达式(默认禁用,值为-1代表此项禁用)</td>
        <td>-1</td>
    </tr>
    <tr>
        <td>cronForRebuildMQComponents</td>
        <td></td>
        <td>定时按照用户配置重构组件(新建连接失败的MQ、销毁更换配置前的MQ发送器/监听器)计划任务表达式</td>
        <td>1000L * 60 * 10(ms)</td>
    </tr>
    <tr>
        <td>sendingMQDuration</td>
        <td></td>
        <td>'发送中'状态的MQ最长时长, 超过此时长后将会被重置为待发送状态</td>
        <td>1000L * 60 * 30(ms)</td>
    </tr>
    <tr>
        <td>recvingMQDuration</td>
        <td></td>
        <td>'接收中'状态的MQ最长时长, 超过此时长后将会被重置为待发送状态</td>
        <td>1000L * 60 * 30(ms)</td>
    </tr>
    <tr>
        <td>cronForSyncActionLog</td>
        <td></td>
        <td>同步数据到ActionLog的定时表达式</td>
        <td>1000L * 20(ms)</td>
    </tr>
    <tr>
        <td>cronForCloudSync</td>
        <td></td>
        <td>与云端定时同步表达式</td>
        <td>1000L * 20</td>
    </tr>
    <tr>
        <td>fixedInititalDelay</td>
        <td></td>
        <td>所有定时器固定启动延迟时间</td>
        <td>1000L * 10</td>
    </tr>
    <tr>
        <td>syncWithCloud</td>
        <td></td>
        <td>是否将异常消息和EOS控制台云端进行同步</td>
        <td>true</td>
    </tr>
    <tr>
        <td>syncDBToMQ</td>
        <td></td>
        <td>即使框架检测到需要发送MQ消息, 如果此值设置为false, 那么也不会发送MQ消息.</td>
        <td>true</td>
    </tr>
    <tr>
        <td>enableMQRecv</td>
        <td></td>
        <td>即使框架检测到需要发送MQ消息, 如果此值设置为false, 那么也不会接收MQ消息.</td>
        <td>true</td>
    </tr>
    <tr>
        <td>forceMQSend</td>
        <td></td>
        <td>即使框架检测到"不"需要发送MQ消息, 如果此值设置为true, 那么也会扫描DB发送MQ消息.</td>
        <td>false</td>
    </tr>
    <tr>
        <td>senderClassName</td>
        <td></td>
        <td>自定义发送器替代SDK的MQ发送器全类名</td>
        <td></td>
    </tr>
    <tr>
        <td>mqConnectTimeout</td>
        <td></td>
        <td>连接RabbitMQ的超时时间</td>
        <td>1000 * 30(ms)</td>
    </tr>
    <tr>
        <td>fullScanIntevalSend</td>
        <td></td>
        <td>在增量查询下没有要发送MQ(MQSend)数据的,可以全表扫描(很耗费性能)的间隔时长, 为NULL时每次无发送数据都扫描</td>
        <td>1000L * 60 * 60 * 10</td>
    </tr>
    <tr>
        <td>fullScanIntevalActionLog</td>
        <td></td>
        <td>在增量查询下没有要发送异常MQ(ActionLog)数据时,可以全表扫描(很耗费性能)的间隔时长, 为NULL时每次无发送数据都扫描</td>
        <td>1000L * 60 * 60 * 10</td>
    </tr>
 	<tr>
        <td>errorLineCount</td>
        <td></td>
        <td>发送和接收处理发生异常时记录到错误表的错误日志的行数</td>
        <td>10</td>
    </tr>
</table>

