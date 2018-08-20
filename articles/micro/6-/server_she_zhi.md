# 微服务服务器设置

打开Window->Preferences-> 微服务服务器设置，配置微服务编排运行环境。 

![](/assets/6-/image8.png) 

## 开发者信息

点击开发者信息，进行配置

![](/assets/6-/image_develop.png)

前两个参数是开发人员在开发者中心的key和secret。

spring.application.name是应用名称。

domain是开发者中心的域名。

spring.profiles.active是采用的环境。

创建微服务编排前需要配置如上参数。


## 数据库配置

点击【数据库配置】，勾选是否启用数据库，如下图所示： 

![](/assets/6-/image9.png)

### 数据源

点击【数据源】，进行数据源设置。如下图所示： 

![](/assets/6-/image10.png)

用户填写配置信息（*为必输项），包括数据源名称、数据源类型、URL、用户名、密码、OIDMark、最大链接数、最小链接数、初始链接数、最大Idle时间、超时时间等信息。
单击〖测试连接〗，测试当前修改的数据源是否正确，不会影响管理中心当前使用的数据源。
单击〖Apply〗应用设置，单击〖Restore Defaults〗恢复初始值。

数据源类型：MYSQL5。

Url：链接至本地开发库。

### 第三方数据源

点击【第三方数据源】，进行数据源设置，如下图所示：

![](/assets/6-/image11.png)

单击〖新建〗，弹出窗口，输入名称，单击〖OK〗，新建数据库驱动。 

![](/assets/6-/image12.png)

用户填写配置信息（*为必输项），包括数据源名称、数据源类型、URL、用户名、密码、OIDMark、最大链接数、最小链接数、初始链接数、最大Idle时间、超时时间等信息。
单击〖测试连接〗，测试当前修改的数据源是否正确，不会影响管理中心当前使用的数据源。
单击〖Apply〗应用设置，单击〖Restore Defaults〗恢复初始值。

数据源类型：MYSQL5。

Url：链接至本地开发库。 

## 启动设置

点击【启动设置】，如下图所示： 

![](/assets/6-/image13.png)

可配置OSGi Bundle组件信息，进行启动、不启动、设置启动级别及删除组件等操作。

单击〖Apply〗应用设置，单击〖Restore Defaults〗恢复初始值。

组件启动设置，详见server相关文档。 

## JVM及启动参数

点击【JVM及启动参数】，如下图所示： 

![](/assets/6-/image14.png)

设置JVM参数，
 Classpath，
Bootclasspath
单击〖Apply〗应用设置，单击〖Restore Defaults〗恢复初始值。 

## 流程调试

点击【流程调试】，如下图所示： 

![](/assets/6-/image15.png)

勾选启动流程调试，配置调试服务端口；
勾选启动OSGI远程管理，配置OSGI远程管理端口；
勾选开启JMX端口，配置JMX端口。

单击〖Apply〗应用设置，单击〖Restore Defaults〗恢复初始值。

## HTTP安全配置

点击【HTTP安全配置】，如下图所示： 

![](/assets/6-/image16.png)

单击〖新建〗，创建一个Https启动设置。
勾选是否使用安全模式，配置HTTPS端口号，设置证书库、信任证书库信息。

单击〖Apply〗应用设置，单击〖Restore Defaults〗恢复初始值。 

## 日志及输出

点击【日志及输出】，如下图所示： 

![](/assets/6-/image17.png)

显示选择的服务器上的日志配置信息，用户可以直接修改服务器上组件的Java包的日志，日志级别配置，配置日志输出规则。

单击〖Apply〗应用设置，单击〖Restore Defaults〗恢复初始值。 

## 异步任务

点击【异步任务】，如下图所示： 

![](/assets/6-/image18.png)

单击〖Apply〗应用设置，单击〖Restore Defaults〗恢复初始值。 

## 消息中间件

点击【消息中间件】，如下图所示： 

![](/assets/6-/image20.png)

其中，MQ Type可以选择MQ或者AMQP。 

![](/assets/6-/image21.png)



选择连接工厂类名，ibm.mq、ActiveMQ配置信息，用户可修改XML配置信息。

单击〖Apply〗应用设置，单击〖Restore Defaults〗恢复初始值。


## 控制台样式配置

点击【控制台样式配置】，如下图所示： 

![](/assets/6-/image23.png)

〖default〗样式组，默认10种样式。
单击〖添加样式〗，用户可自定义样式组。单击〖Add〗，在右侧窗口新增样式，设置样式具体信息，如下图所示：

![](/assets/6-/image24.png) 

单击〖删除样式〗，删除左侧窗口选中的样式组。
单击〖default〗，在样式组中添加默认的样式组。

单击〖Apply〗应用设置，单击〖Restore Defaults〗恢复初始值。 

## 部署配置

点击【部署配置】，如下图所示：

![](/assets/6-/image25.png)

用户可导出源代码包：导出通用的源代码包、导出的插件中含源代码。

单击〖Apply〗应用设置，单击〖Restore Defaults〗恢复初始值。 


















