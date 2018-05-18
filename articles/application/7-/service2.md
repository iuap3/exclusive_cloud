# 业务日志
## 1.	功能简介
一个现实场景：</br>
我们对于记录（也叫业务日志）的操作，很多时候是这样编码的：</br>
   	// 创建一家公司</br>
	public Organization createCompany(CompanyDto dto) {</br>
		// 执行业务方法</br>
		companyDAO.save(dto);</br>
		// 记录日志</br>
		LogDAO.save(new BusinessLog(userDAO.getSubjectName() </br>
				+ "，创建子公司：" + dto.getCompanyName()));</br>
	}</br>
最后结果就是：</br>
    1). 新公司的创建</br>
    2). 业务日志：张三，创建子公司：广州子公司</br>
咋一看这样写没有什么问题，但是其中有一个最大的问题：业务逻辑和日志逻辑的混在一起。</br>
如果业务逻辑和日志逻辑足够复杂的时候，你可以想像你的代码就如同意大利面一样。以后维护的时候，就会变成人间地狱！

业务日志组件可以解决此问题：业务逻辑和日志逻辑“分离”！</br>
业务日志组件的目标</br>
1). 日志的记录对业务方法尽量无侵入</br>
2). 尽最大可能不影响业务方法的性能</br>
3). 系统及日志模板配置简单</br>
4). 日志持久化(也称为导出日志)方式灵活</br>
5). 修改日志模板而不需要重启应用</br>
事实上，要达到真正的无侵入是不可能的，业务日志组件对业务方法的侵入只不过是要在业务方法上加上一个注解。</br>
## 2.	工作流程
组件对使用@BusilogConfig("业务方法别名")注解的业务方法，通过Spring AOP进行拦截。当Spring察觉到被注解的业务方法被调用的时候，即可对此方法进行日志记录。</br>
其中，本组件通过Groovy脚本配置日志模版，定义拦截的业务方法的参数。日志注解@BusilogConfig("业务方法别名")用来给需要日志记录的方法链接日志模版，不同的业务方法可以有不同的日志模板。同时对于日志内容的输出也是可配置的，用户可以自定义来实现对业务日志的持久化操作。

### 2.1.	环境配置
#### 2.1.1.	war包部署
应用平台安装盘中获取业务日志war包iuap-saas-busilog-service.war，部署到tomcat对应webapps下。
 
1).	auth.properties文件配置缓存地址、系统id。系统id默认配置为wbalone
 
2).	jdbc.properties数据源配置
 
配置数据源地址，上图中为mysql配置示例，如果非动态数据源则配置为dataSource
 
3).	sdk.properties配置sdk相关身份认证通道。对当前示例，我们需要配置autifile地址。
 
4).	workbench-sdk.properties工作台配置
配置工作台地址、工作台的认证文件地址
 
经过以上4个步骤，业务日志配置成功</br>
#### 2.1.2.	数据库脚本
执行数据库脚本</br>
CREATE TABLE busilog(</br>
id VARCHAR(40) NOT NULL,</br>
clientip VARCHAR(18),/*IP*/</br>
operuser VARCHAR(40),/*用户*/</br>
logcategory  VARCHAR(40),/*日志分类*/</br>
logcontent  VARCHAR(500),/*日志内容*/</br>
sysid VARCHAR(40),/* 应用id*/</br>
tenantid VARCHAR(40),/* 租户id*/</br>
logdate TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,</br>
     PRIMARY KEY (id)</br>
) ENGINE=InnoDB DEFAULT CHARSET=utf8;</br>

#### 2.1.3.	开发环境配置

1、	pom.xml上增加如下配置，添加依赖的业务日志war包</br>
<module.version>3.5.1-RELEASE</module.version>

<!-- 业务日志 -->


    <dependency>
       <groupId>com.yonyou.iuap.pap.busilog</groupId>
       <artifactId>iuap-busilog-core</artifactId>
       <version>${module.version}</version>
       <exclusions>
         <exclusion>
           <groupId>com.yonyou.iuap</groupId>
               <artifactId>iuap-user-adapter</artifactId>
            </exclusion>
       </exclusions>
    </dependency>  
    
2、在路径DevTool\examples\example-iuap-busilog\src\main\resources中找到两个配置文件
 
并拷贝到当前项目的resource资源文件夹下
 
其中：BusinessLogConfig.groovy：日志输出模板，配置及意义，见 扩展机制

3、修改配置文件
如下图中busilog-applicationContext.xml中如下图部分注释掉。
<!-- 注入日志拦截器 -->
    <bean id="logInterceptor" class="com.yonyou.uap.ieop.busilog.BusiLogInterceptor">
       <property name="threadPoolTaskExecutor" ref="threadPoolTaskExecutor" />
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

4、busilog-systemConfig.properties配置

需要配置aop的插入点，在此处修改为service的包

  #日志开关

    yonyou.businesslog.enable=true
      #指定拦截的业务方法，使用Spring的切入点n写法
      #pointcut=execution(public * com.yonyou.*..*ser*.*(..))
       pointcut=execution(public * *..service.*.*(..))
       #pointcut=execution(public * *..service.*(..))
      #指定日志导出器IBusiLogWriter接口的实现。默认有：BusiLogConsoleWriter
      #businessLogExporter=com.yonyou.uap.busilog.BusiLogDBWriter
    businessLogExporter=com.yonyou.uap.ieop.busilog.writer.BusiLogRestWriter

      #线程池配置
      #核心线程数
    log.threadPool.corePoolSize=2
      #最大线程数
    log.threadPool.maxPoolSize=5
      #队列最大长度
    log.threadPool.queueCapacity=8
      #线程池维护线程所允许的空闲时间
    log.threadPool.keepAliveSeconds=100
      #线程池对拒绝任务(无线程可用)的处理策略log.threadPool.rejectedExecutionHandler=java.util.concurrent.ThreadPoolExecutor$CallerRunsPolicy
    
### 2.2.	代码示例
1、配置业务模板

BusinessLogConfig.groovy日志输出模板

import static 

com.yonyou.uap.ieop.busilog.context.ContextKeyConstant.BUSINESS_SYS_ID;

class BusinessLogConfig_iuap_qy {

    def context;    
    def Ygdemo_yw_info_save() {
        [category:"业务日志",log:"督办任务：执行保存方法:IP地址为${context._ip},USER用户为${context._user},TIME操作时间为${context._time},编码为${context._methodReturn.code},名称为${context._param0.name}"]
    }
    
    def Ygdemo_yw_info_selectAllByPage() {
        [category:"业务日志",log:"督办任务：执行查询方法:IP地址为${context._ip},USER用户为${context._user},TIME操作时间为${context._time}"]
    }
    
    def Ygdemo_yw_info_batchDeleteEntity() {
        [category:"业务日志",log:"督办任务：执行删除方法:IP地址为${context._ip},USER用户为${context._user},TIME操作时间为${context._time}"]
    }
}

具体的说明请参考业务日志的API（扩展机制）
http://iuap.yonyou.com/page/service/book/platform3/index.html#/platform3/articles/iuap-develop/10-/iuap-busilog/3.0.0-release/manual.html

2、为业务方法加上annotation
这个别名必须符合Java方法名的命名规则，给业务方法加别名的目的是为了方便业务方法与日志模板之间的映射。
package com.yonyou.iuap.example.service;

import com.yonyou.uap.ieop.busilog.config.annotation.BusiLogConfig;

public class Ygdemo_yw_infoService {
	
	// 需要被日志记录的方法
	@BusiLogConfig("Ygdemo_yw_info_save")
	public Ygdemo_yw_info save(Ygdemo_yw_info entity){
		System.out.println("BusinessService.excute()");
	}

}

### 2.3	应用效果
 

## 3.	扩展机制
### 3.1	日志模板
是一个groovy文件，在这个groovy文件中，你可以写Java代码，也可以写groovy代码。这样，就可以达到最大的灵活；同时，配置起来又不复杂。目前我们支持两种配置方式：单文件配置方式和多文件配置方式。

单文件配置：

 	1）在类路径下加入'BusinessLogConfig.groovy'
	2）文件模板为：

            class BusinesslogConfig {
                //必须
                def context

                //InvoiceApplicationImpl_addInvoice为业务方法别名
                def InvoiceApplicationImpl_addInvoice() {
                    "日志内容"
                }

                def ProjectApplicationImpl_findSomeProjects() {
                    [category:"项目操作", logs:"查找项目"]
                }

            }
    3）配置模板实际上是一个Groovy类，你可以在类中定义任何方法。如果方法为某个业务方法的别名（使用'@BusilogConfig'注解）那么，我们就认为它是一个业务日志方法。它的返回值（return或者放在方法最后一行的变量）将会被Set到'com.yonyou.uap.ieop.busilog.BusinessLog'的实例中。

日志方法返回值有两种情况：

	1. 只返回一个String类型的日志文本；
	2. 返回一个Map，这个Map包括Key为'category'的日志分类及日志文本。

在类中，还可以使用Groovy定义变量的方法：'def context' 定义一个变量。这个变量实际上是一个Map。 Map中存储的是业务方法的 '返回值'、'参数'。如果需要，你可以存储任何你需要的数据。你可以从这个context中取出你需要的内容，填充到你的日志中。至于如何取context中的内容，请看：
在日志模板中取'context'的内容：

     key                      value
    _methodReturn           业务方法返回值
    _param                   业务方法的参数, _param0代表第一个参数 _param1代表第二个参数，依此类推
    _executeError          业务方法执行失败的异常信息
    _businessMethod        业务方法
    _user                    业务方法操作人
    _time                    业务方法操作时间
    _ip                      ip地址


多文件配置：

当业务系统非常复杂的时候，一个日志配置文件是不足够的。我们提供多文件的配置方式<br/>
1）在类路径中加入'businessLogConfig'文件夹。<br/>
2）在该文件夹中加入日志配置文件，文件名任意，只要符合Groovy类文件的命名规范即可。<br/>
注：多文件配置方式与单文件配置方式不兼容。在此业务日志组件中，单文件配置方式优先。<br/>businessLogConfig'文件夹中的所有以'.groovy'结尾的文件都将被作为日志配置文件。<br/>


### 3.2	数据库存储
业务日志组件提供了日志的数据库存储方式，支持替换为自定义的日志存储方式<br/>
步骤如下：<br/>
1.	开发人员通过实现接口<br/>
com.yonyou.uap.ieop.busilog.writer.itf.IBusiLogWriter<br/>
2.	在spring的properties文件中，将如下属性替换为自定义的实现类<br/>
businessLogExporter= com.yonyou.uap.ieop.busilog.writer.BusiLogRestWriter<br/>
 #指定日志导出器IBusiLogWriter接口的实现。默认有：BusiLogConsoleWriter<br/>
 #businessLogExporter=com.yonyou.uap.busilog.BusiLogDBWriter<br/>
businessLogExporter=com.yonyou.uap.ieop.busilog.writer.BusiLogRestWriter


