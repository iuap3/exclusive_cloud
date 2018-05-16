# 编码规则开发案例

**业务场景**</br>
编码规则使用了应用组件iuap-billcode</br>
组件功能：根据编码规则和一些必要信息，生成业务对象编码。组件需要的编码规则可以存储在文件中，甚至是手工构造，或者存储在数据库中。本组件提供了存储在数据库中一种实现，提供了基本的增加，删除，修改编码规则的服务方法，用户可以在此调用此服务，开发出自己的编码规则设计工具，也可以直接设计符合自己要求的编码规则数据库结构和服务，编码规则VO符合组件的模型即可（实现组件的规则接口）。</br>
另外，组件提供了数据库行锁和zookeeper分布式锁的支持，以应对大并发的情况下可能出现的编码重复问题。</br>
文档中案例业务需求：客户节点新增保存时获取定制的单据号。

**开发方案**</br>
编码规则的获取相对来讲是比较独立的业务，所以从业务上可以把它单独分包。同时使用dubbox提供分布式服务。这样如果在获取量非常大的情况下，可以给编码规则单独部署增加服务达到性能优化的途径，同时使用zookeeper分布式锁, 以应对大并发的情况下可能出现的编码重复问题。</br>
总体方案为：编码规则组件+zookeeper【分布式锁】+Dubbox【分布式服务】</br>
新版本中使用RestAPI方式调用并获取编码规则。

## 1、环境准备

### 1.1、war包配置

下面说明获取到iuap-saas-billcode-service.war后，需要修改的配置文件。培训现场环境已提供，不需要修改。直接跳转到1.2章节
修改配置文件路径WEB-INF\classes
1.	application.properties 配置zookeeper地址，如果为集群配置集群服务</br>
 ![](/articles/application/4-/images/16/image2.png)

2.	jdbc.properties</br>
 ![](/articles/application/4-/images/16/image3.png)

3.	auth.properties 配置Redis服务地址，注意sysid配置为wbalone，和业务系统一致</br>
 ![](/articles/application/4-/images/16/image4.png)

### 1.2、开发环境配置

配置application.properties文件，配置编码规则提供的服务地址</br>
如案例中使用了本机的编码规则服务，则如下图配置</br>
billcodeservice.base.url=http://127.0.0.1:8080/iuap-saas-billcode-service

### 1.3、数据库脚本配置

#### 1.3.1 注册编码对象

编码实体手工注册到数据库表pub_bcr_obj中</br>
注册根节点、示例节点【主子表示例】</br>
注意iscatalog要做赋值，否则页面加载不出数据来

 ![](/articles/application/4-/images/16/image5.png)

#### 1.3.2 注册编码规则定义

在应用平台里已经提供了编码规则定义的节点：部署war包成功后，在管理中心-编码规则的节点就可以正常打开

 ![](/articles/application/4-/images/16/image6.png)

编码规则定义节点可以注册编码规则。在数据库里注册编码对象后，在编码规则定义节点左侧就会出现对象的树型结构

 ![](/articles/application/4-/images/16/image7.png)


选择刚才注册的主子表示例节点，点击新增，录入数据后保存
注意这里的规则编码在后面代码部分会用到。
我们按照DEMO+日期YYMMDD+数字流水号完成

 ![](/articles/application/4-/images/16/image8.png)

保存成功后如下图所示

 ![](/articles/application/4-/images/16/image9.png)

需要将编码规则设置为默认，操作如下图</br>
 
 ![](/articles/application/4-/images/16/image10.png)

完成后如下图所示</br>
 
 ![](/articles/application/4-/images/16/image11.png)

此时编码规则的注册就完成了</br>

## 2、代码开发示例

### 2.1 编码规则RestAPI

官网API：组件提供的接口


- 获取编码规则</br>
服务API“/billcoderest/billcoderest”</br>
参数字段	&ensp; &ensp; 必选	&ensp; 类型	&ensp; 长度	&ensp; &ensp; 说明</br>
billObjCode	&ensp; True	&ensp; String	 &ensp; &ensp; &ensp; &ensp; 	编码规则对象的编码</br>
pkAssign&ensp; &ensp; &ensp; 	False&ensp; 	String	 	&ensp; &ensp; &ensp; &ensp; 分配对象</br>
billVo	&ensp; &ensp; &ensp; &ensp; &ensp; False	 Object	 &ensp; &ensp; &ensp; &ensp; 	实体对象</br>
 

- 退号服务</br>
服务URL：” /billcoderest /returnBillCode”</br>
参数字段	 &ensp;&ensp; &ensp;  必选  &ensp;&ensp;	类型	 &ensp;&ensp; 长度&ensp;&ensp;	说明</br>
billObjCode&ensp;&ensp;	True&ensp;&ensp;	String	 &ensp;&ensp;	&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;编码规则对象的编码</br>
pkAssign&ensp;&ensp;&ensp;&ensp;	False	&ensp;&ensp;String	 &ensp;&ensp;&ensp;	&ensp;&ensp;&ensp;&ensp;分配对象</br>
billVo	 &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;  False	   &ensp; Object	 &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;实体对象</br>
billCode&ensp;&ensp;&ensp;&ensp;&ensp;	True	&ensp;&ensp;&ensp;String		&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;生成的编码</br>

### 2.2 业务单据实现

**获取单据号</br>**
调用编码规则的RestAPI，调用服务获取到单据号后，保存即可。
 
 ![](/articles/application/4-/images/16/image12.png)

 ![](/articles/application/4-/images/16/image13.png)
 
**单据号退号**</br>
在删除方法里调用退号的RestAPI</br>
参数还需要传入当前业务单据的编号</br>
![](/articles/application/4-/images/16/image14.png) 

退号方法</br>
 ![](/articles/application/4-/images/16/image15.png)

3、应用效果
登录应用平台环境，打开“主子表示例点，新增保存后，生成单据号
 
 ![](/articles/application/4-/images/16/image16.png)