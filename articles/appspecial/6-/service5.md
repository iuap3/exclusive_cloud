
## 开发指南:定时任务

### 简介
iuap-dispatch-service组件功能包括添加、删除、暂停、重启任务。不仅提供了外部调用的Rest服务，并且本身也有完整的任务配置界面，
包括任务调度，日志查询等功能。

### 数据库信息预置
1.执行数据库脚本，预置数据库表信息

依次执行examples项目下sql目录中的dll.sql、index.sql、dml.sql建立数据库并初始化数据。
预置数据库表dispatch_taskway的信息，这张表是用户要执行任务的清单，需要用户预置进去，其中url是指你要执行的定时任务，通过HTTP的方式访问。

### 任务调度提供两种方式如下

1.新建任务Rest接口

A、新增一个简单任务

(1)Rest服务URL 

“http://localhost:8080/iuap-dispatch-service/dispatchserver/add.do” 

(2)参数,格式如下

<pre>{"replace":true,"recallConfig":{"data":{},"option":{"url":"http://localhost:8080/iuap-dispatch-service/dispatchserver/pause.do"},"recallType":"HTTP"},"taskConfig":{"triggerType":"SimpleTrigger","jobCode":"22b511e8-1b80-4f4d-b65e-48f52d8aa682","groupCode":"simpleTaskGroup","startDate":1463813876403,"endDate":null,"priority":0,"timeConfig":{"interval":2,"intervalType":"SECOND","isForever":false,"repeatCount":1}}};</pre>

B、新增一个Cron表达式任务

(1)Rest服务URL 

http://localhost:8080/iuap-dispatch-service/dispatchserver/add.do”

(2)参数,格式如下
<pre>
{\"replace\":true, \"recallConfig\":{\"data\":{\"serverName\":\"Windows 2003\"},\"option\":{\"url\":\"http://localhost:8080/iuap-dispatch-service/dispatchserver/pause.do\"},\"recallType\":\"HTTP\"}, \"taskConfig\":{\"cronExpress\":\"* */1 * * * ?\",\"groupCode\":\"cronTaskGroup\",\"jobCode\":\"cronTask\",\"priority\":0,\"triggerType\":\"CronTrigger\"}}";</pre>

### 通过界面进行任务调度

将组件war包部署到服务器上访问首页http://IP:PORT/iuap-dispatch-service 进行任务的添加.

#### 任务列表界面

界面如下图所示：


![](/articles/application/6-/images/05/6-4-1.png)

操作说明

可以新增分组、新增任务、查看日志、对任务进行批量的启用停用。

界面说明如下

左侧为任务分组
右侧为分组下的任务列表界面，任务会根据状态显示为不同的颜色。最查看任务、查看任务日志、删除任务等操作。鼠标悬停时，未超期任务显示停用、启用操作按钮，

### 新增任务分组

界面如下图所示：



![](/articles/application/6-/images/05/6-4-2.png)

操作说明：在主界面上点击界面左上角新建分组按钮。弹出界面，填写后保存。

界面说明如下：

分组名称：分组的标识

### 新增任务

界面如下图所示


![](/articles/application/6-/images/05/6-4-3.png)

操作说明：在主界面上点击界面左上角新建任务按钮。弹出任务编辑界面，填写后保存。

界面说明如下：

1.任务编码：任务的标识

2.任务名称：具有识别意思的名字

3.任务分组：指定任务所属的组

4.消息接收：配置任务成功后发送消息的接收人和接收方式（需要扩展实现消息发送逻辑)

5.定时规则：任务具体执行时间，每多少天执行一次，并指定开始时间

6.调用规则：指定要执行的任务，具体规则详见下面预制数据说明。

7.参数：任务执行时，会根据指定的参数值调用业务服务。

定时规则界面如下图所示：

![](/articles/application/6-/images/05/6-4-4.png)

界面说明如下：

1.支持指定任务开始结束时间

2.支持指定每天、每周（可选择周几）、每月第几天执行任务

3.支持在上面选择的日期一天内，指定发生次数、间隔时间和起止时间

### 查看任务详情

界面如下图所示：


![](/articles/application/6-/images/05/6-4-5.png)

操作说明：点击主界面列表的数据行右侧查看任务详情图标。弹出任务详情界面。

界面说明如下：

1.左侧显示任务的名称和执行情况

2.右侧显示任务的详细信息

3.编辑：进入任务编辑界面

4.日志：进入日志查看界面，显示当前任务的日志

### 修改任务

界面如下图所示：

![](/articles/application/6-/images/05/6-4-6.png)

操作说明：在主界面列表的数据行右侧点击编辑任务图标。弹出任务编辑界面，填写后保存。

界面说明如下：

1.任务编码：任务的标识

2.任务名称：具有识别意思的名字

3.任务分组：指定任务所属的组

4.消息接收：配置任务成功后发送消息的接收人和接收方式（需要扩展实现消息发送逻辑）

5.定时规则：任务具体执行时间，每多少天执行一次，并指定开始时间

6.调用规则：指定要执行的任务，具体规则详见下面预制数据说明

7.参数：任务执行时，会根据指定的参数值调用业务服务。

### 删除任务

操作说明：在主界面列表的数据行右侧点击删除任务图标。

### 启用任务

界面如下图所示：


![](/articles/application/6-/images/05/6-4-7.png)

操作说明：鼠标选择已停用的任务时，列表行显示启用按钮。点击按钮启用该任务。

### 停用任务

界面如下图所示：


![](/articles/application/6-/images/05/6-4-8.png)

操作说明：鼠标选择正在运行中的任务时，列表行显示停用按钮。点击按钮停用该任务。

### 批量操作任务

界面如下图所示：


![](/articles/application/6-/images/05/6-4-9.png)

操作说明：点击多选按钮时，会滑出启用、停用、删除按钮。列表界面切换为多选状态，选择多个任务后，点击按钮进行操作。点击多选回到原有状态。

### 查看任务执行日志

界面如下图所示：

![](/articles/application/6-/images/05/6-4-10.png)

操作说明：

1.点击主界面左上角的“日志”按钮，显示全部任务日志

2.点击任务详情界面的“日志”按钮，显示当前任务的日志

3.在日志界面的左上角可以选择要查询的任务，选择后，根据指定的任务查询并显示日志信息详见下图。

过滤任务界面： 

![](/articles/application/6-/images/05/6-4-11.png)

操作说明：

1.选择任务分组显示分组下的任务

2.选择具体任务后，界面会按照选择的任务过滤显示。

### 查看日志详情

界面如下图所示：

![](/articles/application/6-/images/05/6-4-12.png)

操作说明：点击任务日志列表界面的数据行右侧的“查看原因”按钮，显示日志详情。

描述信息：对任务的简单描述

### 开发步骤

1.从maven库上下载war包。

2.配置dispatch_dbinfo.properties 配置文件，连接mysql数据库的数据源信息。

3.执行dispatch.sql 和tables_mysql.sql 初始化数据库的脚本。

4.预置数据库表dispatch_taskway的信息，这张表是用户要执行任务的清单，需要用户预置进去，其中url是指你要执行的定时任务，通过HTTP的方式访问。如果不需要带界面的任务调度服务可以忽略此项。

5.Rest服务调用接口提供任务的增删等功能，具体调用方式参考工程样例章节。

6.带界面的任务调度系统访问方式 

http://IP:PORT/iuap-dispatch-service

### 预制数据说明

需要用户在数据库表dispatch_taskwayclass、dispatch_taskway和dispatch_taskparam中预制规则数据，

1.dispatch_taskwayclas：规则分类

2.dispatch_taskway：规则，主要是url字段为执行任务时，调用的业务服务的地址，需要按任务调用的要求提供。

3.dispatch_taskparam：规则可设置参数列表，在调用业务服务时，会根据任务指定参数调用
