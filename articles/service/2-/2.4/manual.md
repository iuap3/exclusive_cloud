# 微服务工程创建及配置

微服务治理平台提供java版的RPC调用SDK，采用Maven组件的方式发布，开发者可以使用Maven工程的方式进行工程配置和构建，需要修改的配置主要有application.properties、pom.xml以及spring的配置文件。

## 一：微服务java工程说明

**1：可以新建Maven工程，maven工程可以为普通的基于spring框架的web工程，也可以为springboot工程；**

**2：建议参考示例工程中的rpc-client、rpc-provider等，下载地址请参考示例说明章节；**


## 二：工程配置修改

### （1）maven依赖(pom文件)配置 ###

**1：增加mwclient的依赖.**

	<dependency>
	    <groupId>com.yonyou.cloud.middleware</groupId>
	    <artifactId>mwclient</artifactId>
	    <version>5.1.1-RELEASE</version>
		<type>pom</type>
	</dependency>


 2.注意：artifactId(影响访问路径)，finalName要与应用名称尽量保持一致.


### （2）属性文件配置 ###

**application.properties**

1. access.key和access.secret与申请的Access Key中内容保持一致

2. spring.profiles.active为应用的环境(开发：dev，测试：test，灰度：stage，生产：online)

3. spring.application.name为应用的编码，其内容应与RemoteCall注解中的应用编码部分一致（RemoteCall注解由应用编码@租户id组成）

4. registry 此选项为专属云版的微服务治理平台需要的特定配置，对应后端各个服务的地址

### （3）spring配置文件 ###

示例工程中提供不同组件需要的spring配置文件示例，其功能列表如下：

1. applicationContext.xml为工程的spring主配置文件，定义了扫描包和属性文件的引入;

2. applicationContext-rpc.xml为RPC Bean配置文件;
 
3. applicationContext-eos.xml为异步调用配置文件;

## 三：配置常见问题

### （1）验证失败.请确认accessKey 和application.name的正确性! ###

**出现此问题可能的原因如下:**

- 输入的access.key和access.secret不一致或者错误
- 此AccessKey已被停用或者删除
- application.name即应用编码输入错误
- 此用户下没有应用编码为application.name的应用

### （2）调试态启动多个服务端口冲突 ###

示例工程默认提供jetty插件配置，可以使用jetty:run的方式直接调试运行，请检查jetty插件配置的各个应用的端口是否被占用。