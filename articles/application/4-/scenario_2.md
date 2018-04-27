 

# 主子表节点案例开发

业务场景</br>

iuap平台是面向企业互联网应用的企业互联网运营平台。本指南以一个后台管理中的-列表类型页面为例，演示如何开发标准节点的操作步骤。</br>
1、需要安装iUAP-STUDIO开发工具（可以参考iuap后台环境搭建、Iuap前端环境搭建视频）</br>
2、建立实体模型，进行实体的设计</br>
3、其次先完成后台功能的开发，包括查询、新增保存、修改、删除操作。</br>
4、前端开发。</br>

## 1.	注册节点

### 1.1.	环境准备

新建项目以及数据库配置过程，请参考《环境搭建》文档。

### 1.2、示例代码注册

主子表示例代码
提供主子表busidemo示例代码，包括前后台代码，映射文件等。</br>
后台代码位置
![](/articles/application/4-/images/04/image2.png)

映射文件位置
 ![](/articles/application/4-/images/04/image3.png)

前台代码位置busidemo文件夹
 ![](/articles/application/4-/images/04/image4.png)

功能注册!</br>
使用系统管理员账号登陆</br>
 ![](/articles/application/4-/images/04/image5.png)

进入管理中心</br>
 ![](/articles/application/4-/images/04/image6.png)

选择功能管理下的“功能管理”</br>
 ![](/articles/application/4-/images/04/image7.png)
新增小应用，小应用链接填写项目名称+pages下的busidemo.js的路径</br>
![](/articles/application/4-/images/04/image8.png) 
对应的URL配置为后台服务地址</br>
 ![](/articles/application/4-/images/04/image9.png)
	在权限管理下选择“功能授权”进行授权
 ![](/articles/application/4-/images/04/image10.png)
点击授权按钮，勾选新增小应用</br>
![](/articles/application/4-/images/04/image11.png)
 
功能注册完毕</br>
**界面效果展现**</br>
访问首页http://127.0.0.1:8080/wbalone，使用sale1/123qwe登录</br>
 ![](/articles/application/4-/images/04/image12.png)
打开主子表节点</br>
 ![](/articles/application/4-/images/04/image13.png)

切换到编辑态</br>
 ![](/articles/application/4-/images/04/image14.png)
 
## 2	代码分析（结合实际业务）

实际业务需求： </br>
常规编辑功能：</br>
- 字段非空校验；</br>
- 新增默认值，设置字段为不可编辑</br>
- 节点有冻结和解冻功能</br>
- 常规参照的开发</br>
- 
### 2.1	示例代码结构

1.	后台代码

 ![](/articles/application/4-/images/04/image15.png) 

com.yonyou.iuap.appdemo.web.BusidemoController主表提供的控制层类
com.yonyou.iuap.appdemo.web.Busidemo_subController子表单独提供的控制层类

com.yonyou.iuap.appdemo.service.BusidemoService主表的Service服务类；
com.yonyou.iuap.appdemo.service.Busidemo_subService子表的Service服务类

com.yonyou.iuap.appdemo.mapper.BusidemoMapper主表持久层dao类
com.yonyou.iuap.appdemo.mapper.Busidemo_subMapper子表持久层dao类

com.yonyou.iuap.appdemo.entity.Busidemo主表主实体
com.yonyou.iuap.appdemo.entity.Busidemo_sub子实体

2.	SQL映射文件位置</br>
 
 ![](/articles/application/4-/images/04/image16.png)

3.	前台代码</br>
Css样式文件、html页面展示，js页面控制，meta.js模型文件</br>
 
![](/articles/application/4-/images/04/image17.png)

### 2.2 Mybatis介绍

MyBatis是支持普通SQL查询，存储过程和高级映射的优秀持久层框架。</br>
MyBatis消除了几乎所有的JDBC代码和参数的手工设置以及结果集的检索。MyBatis使用简单的XML或注解用于配置和原始映射，将接口和Java的POJOs（Plan Old Java Objects，普通的Java对象）映射成数据库中的记录.</br>

Mybatis运行过程</br>
![](/articles/application/4-/images/04/image18.png) 

### 2.3 后台代码解析（常见注解、保存校验、动作逻辑）

后端代码分为几层：实体、DAO、Service、Controller，下面的中描述了手工编写后台代码时的开发步骤，此处因为向导开发，已经生成完毕。

![](/articles/application/4-/images/04/image19.png) 
 

#### 2.3.1 配置解析

**SpringMVC框架**</br>
iuap 平台集成了Spring框架进行组件的配置和管理，以及Spring MVC作为后端MVC框架，更方便业务开发者进行开发使用。</br>
![](/articles/application/4-/images/04/image20.png)  
从pom.xml中可以查看到Spring的版本，集成时此版本务必保持一致。

![](/articles/application/4-/images/04/image21.png)  

**持久化配置**</br>
iuap平台使用iuap-mybatis作为MyBatis持久化的支持并提供了统一的Spring扫描注解和分页插件来支持分页的实现。</br>
iuap-persistence同时依赖了基础的Spring Data JPA和Mybatis，如果项目上明确只使用Mybatis，则不需要引入iuap-persistence组件，直接依赖iuap-mybatis组件即可，注意要和iuap其他组件的版本保持一致。</br>

**功能特性**</br>
iuap-mybatis提供了统一的Spring扫描注解和分页插件。通过@MyBatisRepository确定mybatis提供服务的接口，再使用mybatis的xml文件管理实现mybatis服务的sql语句。</br>
分页插件支持MyBatis3.2.0~3.3.0(包含)</br>
**pom配置**</br>
在maven中配置
 ![](/articles/application/4-/images/04/image22.png) 
**mybatis持久化配置**</br>
找到持久化配置文件</br>
 ![](/articles/application/4-/images/04/image23.png) 
因为使用了Mybatis的持久化方式，所以需要修改对应的配置信息，增加Mybatis扫描的entity包。同时因为使用了mysql数据库，所以下面配置文件为mysql文件夹下的xml文件

![](/articles/application/4-/images/04/image24.png) 
 
mapper接口类位置，增加我们目前生成的mapper包位置

 ![](/articles/application/4-/images/04/image25.png) 

#### 2.3.2 实体

主子表类型的实体有两张表：主表和子表</br>
1)	主表对应实体：Busidemo类</br>
 ![](/articles/application/4-/images/04/image26.png) 

  ![](/articles/application/4-/images/04/image27.png) 

id_busidemo_sub为主实体中对应的子实体List
 
通用的get/set方法</br>
  ![](/articles/application/4-/images/04/image28.png) 

@JsonIgnoreProperties：此注解是类注解，作用是json序列化时将java bean中的一些属性忽略掉，序列化和反序列化都受影响。</br>
主表中会加入参照的名称字段，用于在列表界面参照主键与名称的转换【参考参照开发章节】</br>
特殊字段：</br>
业务代码如果需要操作对象的ts属性，则每次在持久化数据库中都需要修改操作ts属性为务器的当前时间。</br>
2)	子表对应实体类：Busidemo_sub.java</br>
 ![](/articles/application/4-/images/04/image29.png)

子表特殊的地方在于增加了针对主表的外键字段fk_busidemo_sub</br>

#### 2.3.3 后台代码通用功能

**Controller** </br>
主表BusidemoController.java类</br>
 ![](/articles/application/4-/images/04/image30.png) 

子表Busidemo_subController</br>
  ![](/articles/application/4-/images/04/image31.png)

Controller继承了iuap提供的Controller基类：</br>
com.yonyou.iuap.example.web.BaseController【提供了各种错误、成功提示信息包装】</br>
Controller类上必须使用@RestController或者@Controller注解、@RequestMapping注解用户地址映射</br>
@RestController：继承自@Controller注解，Spring MVC的组件都使用@Controller来标识当前类是一个控制器servlet。使用这个特性，我们可以开发REST服务的时候不需要使用@Controller而专门的@RestController。</br>
@RequestMapping：是一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径1、 value， method；value： 指定请求的实际地址，指定的地址可以是URI Template 模式；method：  指定请求的method类型， GET、POST、PUT、DELETE等；</br>
@Autowired：用在JavaBean中的注解，通过byType形式，用来给指定的字段或方法注入所需的外部资源。</br>
@ResponseBody:用于将Controller的方法返回的对象，通过适当的
HttpMessageConverter转换为指定格式后，写入到Response对象的body数据区。【此处可以将我们的实体转为前端的Jason对象，用于前端数据显示，或者从前端的jason对象转为我们的实体】；</br>
Controller需要调用Service中的方法</br>
 ![](/articles/application/4-/images/04/image32.png) 

**Service**</br>
主表Service层代码BusidemoService.java</br>
  ![](/articles/application/4-/images/04/image33.png)
 
子表Busidemo_subService</br>
  ![](/articles/application/4-/images/04/image34.png) 


Service类上必须使用@Service注解；</br>
@Service：用于标注业务层组件</br>
@Transactional：如果在service类前加上@Transactional，则声明这个service所有方法需要事务管理。每一个业务方法开始时都会打开一个事务。但是查询方法本身不需要事务管理，所以我们不在类上加，而在对应的方法上加注解。

**Dao/mapper层**</br>
主表代码BusidemoMapper.java</br>
![](/articles/application/4-/images/04/image35.png)  
子表代码</br>
![](/articles/application/4-/images/04/image36.png) 

**映射文件xml**

![](/articles/application/4-/images/04/image37.png)  

BusidemoMapper.xml为主表中的持久化sql语句

![](/articles/application/4-/images/04/image38.png) 
 
Busidemo_subMapper.xml为子表中的持久化sql语句

![](/articles/application/4-/images/04/image39.png) 
 
在Mapper上需要通过@MyBatisRepository的注解向Spring容器标示出当前类为DAO

#### 2.3.4 查询分页功能

打开节点时，加载默认数据，调用分页方法查询数据</br>
Controller层</br>
分页方法参数：pageRequest分页参数/searchParams查询条件</br>
 
![](/articles/application/4-/images/04/image40.png) 

Service层
![](/articles/application/4-/images/04/image41.png)  

DAO层：</br>
对查询sql做条件拼接处理；</br>
调用dao做分页查询，返回查询成功后的分页信息</br>
![](/articles/application/4-/images/04/image42.png) 
 
SQL映射文件BusidemoMapper.xml</br>
 ![](/articles/application/4-/images/04/image43.png) 
其中分页查询方法使用了配置文件做参数和返回值翻译处理</br>
![](/articles/application/4-/images/04/image44.png)  

#### 2.3.5 加载子表方法【为选中行、切换分页信息提供】

子表Controller代码</br>
com.yonyou.iuap.appdemo.web.Busidemo_subController
地址映射为”/busidemo/list”</br>
![](/articles/application/4-/images/04/image45.png)  

Service层，调用子表dao的查询方法</br>
![](/articles/application/4-/images/04/image46.png)  

Dao持久层</br>
 ![](/articles/application/4-/images/04/image47.png) 

SQL映射文件Busidemo_subMapper.xml</br>
![](/articles/application/4-/images/04/image48.png)  

#### 2.3.6 新增保存【修改保存】

Web层：主表Controller</br>
新增和修改方法，提供给前台的调用地址不同</br>

新增为”/add” 。修改为”/update”</br>
 ![](/articles/application/4-/images/04/image49.png) 

Service层代码</br>
调用了dao层的save方法；注意，此处同时调用了子表的dao做保存。</br>
![](/articles/application/4-/images/04/image50.png) 
主表DAO，BusidemoMapper定义了新增和修改的接口</br>
 ![](/articles/application/4-/images/04/image51.png) 
子表DAO，Busidemo_subMapper定义了新增和修改的接口</br>
 ![](/articles/application/4-/images/04/image52.png) 

主表SQL映射BusidemoMapper.xml</br>
 ![](/articles/application/4-/images/04/image53.png) 
 ![](/articles/application/4-/images/04/image54.png) 

Busidemo_subMapper.xml</br>
![](/articles/application/4-/images/04/image55.png) 
 
#### 2.3.7 批量删除功能

com.yonyou.iuap.appdemo.web.BusidemoController</br>
前台传入的参数为主表数据；调用Service的删除实体方法，注意主子表都需要删除</br>
 ![](/articles/application/4-/images/04/image56.png) 

Service层，删除时需要批量删除主表和子表信息；</br>
调用dao的batchDelete方法，批量删除主表</br>
调用dao提供的删除方法batchDelChild，批量删除子表</br>
 ![](/articles/application/4-/images/04/image57.png) 

主表映射SQL</br>
![](/articles/application/4-/images/04/image58.png)  
子表映射SQL</br>
![](/articles/application/4-/images/04/image59.png) 

#### 2.3.8 子表删除【删除行功能】

Busidemo_subController.java</br>
参数，传入需要删除的子表VO，调用子表的service的删除方法</br>
 ![](/articles/application/4-/images/04/image60.png) 

Busidemo_subService.java</br>
 ![](/articles/application/4-/images/04/image61.png) 

Busidemo_subMapper.java</br>
![](/articles/application/4-/images/04/image62.png) 
 
Busidemo_subMapper.xml</br>
 ![](/articles/application/4-/images/04/image63.png) 

#### 2.3.9 前后端参数、返回值传递

前端框架与服务端没有耦合，中间通过JSON数据格式进行数据传递。可采用正常的ajax请求。如果要发送数据模型中的相关数据，需要把数据模型中的数据转成JSON发请到服务端。</br>
前后端地址传递</br>
 ![](/articles/application/4-/images/04/image64.png)  
前端一般通过ajax调用到后台controller，前后端参数传递的几种方式：</br>
1.	传递单条数据，返回单条数据【新增保存修改保存】；</br>
2.	传递多条，返回为void【删除】;</br>

#### 2.3.10 SpringMVC异常处理

对SpringMVC 异常处理的封装，处理了iweb异常WebRuntimeException,校验异常ValidException和异常错误码支持SysPromptException。</br>
对于ajax请求或者http header中包含Accept:application/json 的请求，会返回json数据； </br>
对于其他请求，会跳转到springmvc视图下的error/500页面。</br>
spring-mvc.xml配置如下：</br>
![](/articles/application/4-/images/04/image65.png) 
 
500页面示例如下：</br>
 ![](/articles/application/4-/images/04/image66.png)
 
使用方式：对于WebRuntimeException和ValidException，直接throw异常即可。</br>
对于SysPromptException，需要系统针对模块名称，错误码，版本，系统国际化信息在数据库【sys_prompt】预制信息，根据参数取值【![](/articles/application/4-/images/04/image67.png)  】。并在spring配置文件中声明获取的实现。</br>
spring配置文件中声明如下</br>
 ![](/articles/application/4-/images/04/image68.png) 

#### 2.3.11 主键处理

比如：在我们的示例中使用了UUI的主键策略，所以可以使用下面的API生成</br>
 ![](/articles/application/4-/images/04/image69.png)
 
或者使用下面的API生成</br>
IDGenerator【项目demo中应用了这种方式】</br>
 ![](/articles/application/4-/images/04/image70.png) 

### 2.4 前台代码解析

#### 2.4.1 meta数据模型

meta.js数据模型定义</br>
metaDt为主表数据模型</br>

![](/articles/application/4-/images/04/image71.png) 
metabusidemo_sub为子表数据模型</br>
 ![](/articles/application/4-/images/04/image72.png) 

在页面控制中，主子表busidemo定义了四个数据模型，分别是列表界面的主表数据模型和子表数据模型、卡片页面的主表数据模型和子表数据模型</br>
 ![](/articles/application/4-/images/04/image73.png) 

#### 2.4.2 html解析 

列表显示panel区域按钮，用于返回应用中心
 
![](/articles/application/4-/images/04/image74.png) 

列表与卡片按钮组，通过页面的切换，来动态显示或隐藏相应按钮组</br>
列表按钮</br>
![](/articles/application/4-/images/04/image75.png) 

卡片按钮</br>
 ![](/articles/application/4-/images/04/image76.png)

列表页面查询区</br>
 ![](/articles/application/4-/images/04/image77.png)

列表页面的主表及绑定的数据模型</br>
![](/articles/application/4-/images/04/image78.png) 

列表页面子表及绑定的数据模型</br>
![](/articles/application/4-/images/04/image79.png) 

卡片页面主表表单，绑定的数据模型和字段</br>
 ![](/articles/application/4-/images/04/image80.png)

卡片界面子表grid表格，绑定的数据模型和字段，对子表操作的增加和删除按钮</br>
 ![](/articles/application/4-/images/04/image81.png)

#### 2.4.3 js解析

通过require的方式引入依赖的资源，其中uiReferComp,refer为参照实现资源</br>
  ![](/articles/application/4-/images/04/image82.png)
分页查询</br>
打开节点时，会调用分页查询方法加载数据</br>
   ![](/articles/application/4-/images/04/image83.png)

分页方法</br>
   ![](/articles/application/4-/images/04/image84.png)

   ![](/articles/application/4-/images/04/image85.png)

加载子表数据</br>
点击列表主表数据，进入rowClick，获取id，进入getUserJobList</br>
   ![](/articles/application/4-/images/04/image86.png)

getUserJobList中向后台发请求，根据主表pk查询子表数据，查询成功后，将后台查询的数据set到对应数据模型中</br>
   ![](/articles/application/4-/images/04/image87.png)
  ![](/articles/application/4-/images/04/image88.png)
 

保存</br>
saveClick中首先是对数据的校验，将主子表数据存入jsondata中，发送给后台</br>
  ![](/articles/application/4-/images/04/image89.png) 
判断当前是否编辑态，分别调用后台新增/修改服务</br>
   ![](/articles/application/4-/images/04/image90.png)

主表删除</br>
delRow选择行校验</br>
   ![](/articles/application/4-/images/04/image91.png)

与后台交互，获取选中数据，并调用后台删除服务</br>
   ![](/articles/application/4-/images/04/image92.png)

删行（子表删除）</br>
对选择行的校验，将数据传递到后台，再将数据从相应数据模型中移除</br>
   ![](/articles/application/4-/images/04/image93.png)

## 3、功能开发

### 3.1、前台界面编辑控制、默认值

需求：

- 主表单据状态、创建人、创建时间、最后修改人、最后修改日期不能编辑
- 主表名称不能为空
- 子表金额不能编辑
- 子表编码、名称不能为空

**1.	表头设置非空项**</br>
以表头名称字段为例，配置编码、名称不能为空</br>
分为两个部分</br>
1）html增加必输项的标示</br>
 ![](/articles/application/4-/images/04/image94.png) 

2）	在数据模型中增加必输项属性，meta.js中</br>
  ![](/articles/application/4-/images/04/image95.png)

这样必输项设置就完成了

**2.	可编辑、不可编辑控制**</br>
以单据状态字段为例，单据状态字段不能编辑</br>
1)	字段设置为不可编辑</br>
找到meta.js的对应设置，找到状态字段billstate设置enable为false</br>
  ![](/articles/application/4-/images/04/image96.png)

2)	新增时设置默认值为“待确认”</br>
  ![](/articles/application/4-/images/04/image97.png)

**3.	子表设置非空项**</br>
编码名称不能为空</br>
1)	meta.js数据模型中设置提示信息</br>
  ![](/articles/application/4-/images/04/image98.png)

2)	设置edittype中属性requried为true，必输</br>
  ![](/articles/application/4-/images/04/image99.png)

**4.	子表不可编辑设置**</br>
如下图示例：子表“金额”字段不能编辑</br>
  ![](/articles/application/4-/images/04/image100.png)

**5.	字段精度设置，在meta.js中数据模型上配置属性**</br>
  ![](/articles/application/4-/images/04/image101.png) 

### 3.2、前台编辑监听

业务需求：

1.	子表中存在数量、价格、金额字段，计算公式：数量*无税单价=价税合计；
2.	主表开始日期<结束日期
3.	是否KPI为“是”，则KPI级别才可以编辑，设置默认为“一级”；
4.	保存时做前端校验

**开发步骤**</br>
1、注册监听</br>
在新增按钮事件中增加valuechange监听</br>
   ![](/articles/application/4-/images/04/image102.png) 
   ![](/articles/application/4-/images/04/image103.png) 

2、监听代码</br>
金额计算：获取信息并计算赋值</br>
   ![](/articles/application/4-/images/04/image104.png) 
 
日期控制，设置日期范围</br>
在html上增加对应field</br>
   ![](/articles/application/4-/images/04/image105.png) 
  ![](/articles/application/4-/images/04/image106.png) 
 
KPI字段编辑控制，控制是否可编辑</br>
在html上增加或查看是否存在对应field和input的ID</br>
  ![](/articles/application/4-/images/04/image107.png) 
   ![](/articles/application/4-/images/04/image108.png) 

应用效果</br>
开始日期与结束日期控制如下图所示</br>
如果录入了“开始日期”，则编辑“结束日期”时，如下图所示
   ![](/articles/application/4-/images/04/image109.png) 

如果选择了“结束日期”，则“开始日期”控制如下
   ![](/articles/application/4-/images/04/image110.png) 

### 3.4、按钮开发

单据增加冻结、解冻按钮及对应前后台对应功能</br>
**前台改造**</br>
html中按钮绑定事件</br>
   ![](/articles/application/4-/images/04/image111.png) 

js增加相应事件：</br>
服务地址定义</br>
   ![](/articles/application/4-/images/04/image112.png) 

枚举值定义增加了value=4冻结状态</br>
   ![](/articles/application/4-/images/04/image113.png) 

冻结活动</br>
   ![](/articles/application/4-/images/04/image114.png) 

确认冻结</br>
   ![](/articles/application/4-/images/04/image115.png) 

解冻功能</br>
   ![](/articles/application/4-/images/04/image116.png)
  
确认解冻</br>
   ![](/articles/application/4-/images/04/image117.png) 

**后台开发**</br>
Controller类增加冻结、解冻方法，服务地址与上述定义一致</br>
   ![](/articles/application/4-/images/04/image118.png) 
BusidemoService里增加对应方法，分别</br>
   ![](/articles/application/4-/images/04/image119.png)  
常量定义如下图所示</br>
   ![](/articles/application/4-/images/04/image120.png)  
Mapper增加更新方法</br>
   ![](/articles/application/4-/images/04/image121.png)  
 
SQL脚本映射</br>
批量更新数据</br>
   ![](/articles/application/4-/images/04/image122.png) 
应用效果</br>
   ![](/articles/application/4-/images/04/image123.png) 








