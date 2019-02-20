
# API文档:定时任务

## 简介
iuap-dispatch-service组件功能包括添加、删除、暂停、重启任务。不仅提供了外部调用的Rest服务，并且本身也有完整的任务配置界面，
包括任务调度，日志查询等功能。


##API接口

###  新增定时任务

添加或覆盖基于Cron表达式的定时调度任务。

**请求体：**

| URl | /dispatchserver/add.do |
| --- | --- |
| Method | POST |

**请求参数说明：**

| 参数字段 | 必选 | 类型 | 长度限制 | 说明 |
| --- | --- | --- | --- | --- |
| recallConfig | True | String | 无 | 回调信息配置，Json格式，具体参考下面说明 |
| taskConfig | True | String | 无 | 任务执行配置，Json格式，具体参考下面说明 |
| replace | True | boolean | 无 | 是否覆盖，为ture时，自动覆盖，为false时，如果存在会返回错误信息 |

recallConfig内容格式：

| 参数字段 | 必选 | 类型 | 长度限制 | 说明 |   |
| --- | --- | --- | --- | --- | --- |
| recallType | True | String | 无 | 回调方式，SOCKET/HTTP |   |
| option | True | String | 无 | 回调的方式地址，&quot;recallType&quot; = &quot;HTTP&quot;时，格式为{&quot;url&quot;:&quot;[http://ip:port/XXX&quot;}](http://ip:port/XXX%22%7D)；&quot;recallType&quot; = &quot;SOCKET&quot;时，格式为{&quot;host&quot;:&quot;ip&quot;,&quot;port&quot;} |   |
| data | True | String | 无 | 任务附加数据，执行时传递给调用接口，建议为json格式 |   |

taskConfig内容格式：

| 参数字段 | triggerType | 必选 | 类型 | 长度限制 | 说明 |
| --- | --- | --- | --- | --- | --- |
| triggerType |   | True | String | 无 | 触发方式，SimpleTrigger/CronTrigger，下面参数根据类型不同变化，dk中的CronTaskConfig/SimpleTaskConfig两种配置类参数 |
| jobCode | SimpleTrigger/CronTrigger | True | String | 无 | 任务名称可参考s |
| groupCode | SimpleTrigger/CronTrigger | True | String | 无 | 组名称 |
| priority | SimpleTrigger/CronTrigger | True | int | 无 | 优先级，数字大的优先执行 |
| endDate | SimpleTrigger/CronTrigger | True | Date | 无 | 任务结束始时间 |
| cronExpress | CronTrigger | True | String | 无 | 表达式， |
| startDate | SimpleTrigger | True | Date | 无 | 任务开始时间 |
| timeConfig | SimpleTrigger | True | Json | 无 | 时间配置，json格式，参考TimeConfig类格式如下 |

timeConfig内容格式：

| 参数字段 | 必选 | 类型 | 长度限制 | 说明 |
| --- | --- | --- | --- | --- |
| interval | True | int | 无 | 执行间隔时间，具体单位见intervalType |
| intervalType | True | String | 无 | 执行间隔时间单位：NULL/MILLISECOND/SECOND/MINUTE/HOUR |
| isForever | True | boolean | 无 | 是否重复执行，为true时会一直定时执行，直到暂停或删除任务 |
| repeatCount | True | int | 无 | 重复次数，isForever为false时，任务仅执行指定的次数 |

**返回参数说明：**

| 参数字段 | 类型 | 说明 |
| --- | --- | --- |
| success | boolean | true/false |
| error | String | 错误信息 |
| resultValue | String | 返回值，一般为null |

### 暂停任务

根据任务名称和组名称暂停任务

**请求体：**

| URl | /dispatchserver/pause.do |
| --- | --- |
| Method | POST |

**请求参数说明：**

| 参数字段 | 必选 | 类型 | 长度限制 | 说明 |
| --- | --- | --- | --- | --- |
| jobName | True | String | 无 | 任务名称 |
| groupName | True | String | 无 | 组名称 |

**返回参数说明：**

| 参数字段 | 类型 | 说明 |
| --- | --- | --- |
| success | boolean | true/false |
| error | String | 错误信息 |
| resultValue | String | 返回值，一般为null |

### 恢复任务

根据任务名称和组名称恢复任务

**请求体：**

| URl | /dispatchserver/resumeTask.do |
| --- | --- |
| Method | POST |

**请求参数说明：**

| 参数字段 | 必选 | 类型 | 长度限制 | 说明 |
| --- | --- | --- | --- | --- |
| jobName | True | String | 无 | 任务名称 |
| groupName | True | String | 无 | 组名称 |

**返回参数说明：**

| 参数字段 | 类型 | 说明 |
| --- | --- | --- |
| success | boolean | true/false |
| error | String | 错误信息 |
| resultValue | String | 返回值，一般为null |

### 删除任务

根据任务名称和组名称删除任务

**请求体：**

| URl | /dispatchserver/delete.do |
| --- | --- |
| Method | POST |

**请求参数说明：**

| 参数字段 | 必选 | 类型 | 长度限制 | 说明 |
| --- | --- | --- | --- | --- |
| jobName | True | String | 无 | 任务名称 |
| groupName | True | String | 无 | 组名称 |

**返回参数说明：**

| 参数字段 | 类型 | 说明 |
| --- | --- | --- |
| success | boolean | true/false |
| error | String | 错误信息 |
| resultValue | String | 返回值，一般为null |

### 立即执行任务

根据任务名称和组名称立即执行任务

**请求体：**

| URl | /dispatchserver/ trigger.do |
| --- | --- |
| Method | POST |

**请求参数说明：**

| 参数字段 | 必选 | 类型 | 长度限制 | 说明 |
| --- | --- | --- | --- | --- |
| jobName | True | String | 无 | 任务名称 |
| groupName | True | String | 无 | 组名称 |

**返回参数说明：**

| 参数字段 | 类型 | 说明 |
| --- | --- | --- |
| success | boolean | true/false |
| error | String | 错误信息 |
| resultValue | String | 返回值，一般为null |