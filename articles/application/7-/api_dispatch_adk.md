
# SDK加签调用:定时任务
## 简介
iuap-dispatch-service组件功能包括添加、删除、暂停、重启任务。不仅提供了外部调用的Rest服务，并且本身也有完整的任务配置界面，
包括任务调度，日志查询等功能。

## API接口 ##

### 指定执行类新增定时任务 ###

**描述**

添加或覆盖简单类型的定时调度任务

**请求方法**

com.yonyou.iuap.dispatch.client.DispatchRemoteManager.add(SimpleTaskConfig simpleTaskConfig, Class<? extends ITask> taskName, boolean replace)

**请求方式**

工具类调用  

**请求参数说明**

<table>
  <tr>
    <th>参数字段</th>
    <th>必选</th>
		<th>类型</th>
    <th>长度限制</th>
    <th>说明</th>
  </tr>
  <tr>
    <td>simpleTaskConfig</td>
    <td>True</td>
		<td>SimpleTaskConfig</td>
    <td>无</td>
    <td>任务执行类</td>
  </tr>
  <tr>
    <td>taskName</td>
    <td>True</td>
		<td>Class</td>
    <td>无</td>
    <td>Cron表达式及相关参数</td>
  </tr>
  <tr>
    <td>replace</td>
    <td>True</td>
		<td>boolean</td>
    <td>无</td>
    <td>是否覆盖，为ture时，自动覆盖，为false时，如果存在会抛出异常</td>
  </tr>
</table>

**返回参数说明**

boolean 成功返回true，否则会抛出异常

### 指定执行类新增带参数的定时任务 ###

**描述**

添加或覆盖简单类型的定时调度任务

**请求方法**

com.yonyou.iuap.dispatch.client.DispatchRemoteManager.add(SimpleTaskConfig simpleTaskConfig, Class<? extends ITask> taskName, Map<String, String> data, boolean replace)

**请求方式**

工具类调用  

**请求参数说明**

<table>
  <tr>
    <th>参数字段</th>
    <th>必选</th>
		<th>类型</th>
    <th>长度限制</th>
    <th>说明</th>
  </tr>
  <tr>
    <td>simpleTaskConfig</td>
    <td>True</td>
		<td>SimpleTaskConfig</td>
    <td>无</td>
    <td>任务执行类</td>
  </tr>
  <tr>
    <td>taskName</td>
    <td>True</td>
		<td>Class</td>
    <td>无</td>
    <td>Cron表达式及相关参数</td>
  </tr>
  <tr>
    <td>data</td>
    <td>FALSE</td>
		<td>Map<String, String></td>
    <td>无</td>
    <td>任务附加数据，以key－value形式增加，仅支持String，在任务执行时传递给任务执行类</td>
  </tr>
  <tr>
    <td>replace</td>
    <td>True</td>
		<td>boolean</td>
    <td>无</td>
    <td>是否覆盖，为ture时，自动覆盖，为false时，如果存在会抛出异常</td>
  </tr>
</table>

**返回参数说明**

boolean 成功返回true，否则会抛出异常

### 指定任务id新增定时任务 ###

**描述**

添加或覆盖简单类型的定时调度任务

**请求方法**

com.yonyou.iuap.dispatch.client.DispatchRemoteManager.add(SimpleTaskConfig simpleTaskConfig,String taskBeanId,Map<String,String> data, boolean replace)

**请求方式**

工具类调用  

**请求参数说明**

<table>
  <tr>
    <th>参数字段</th>
    <th>必选</th>
		<th>类型</th>
    <th>长度限制</th>
    <th>说明</th>
  </tr>
  <tr>
    <td>simpleTaskConfig</td>
    <td>True</td>
		<td>SimpleTaskConfig</td>
    <td>无</td>
    <td>任务执行类</td>
  </tr>
  <tr>
    <td>taskBeanId</td>
    <td>True</td>
		<td>String</td>
    <td>无</td>
    <td>任务执行类BeanId(Spring中配置的BeanID，实现类需要继承com.yonyou.iuap.dispatch.client.ITask接口</td>
  </tr>
  <tr>
    <td>data</td>
    <td>FALSE</td>
		<td>Map<String, String></td>
    <td>无</td>
    <td>任务附加数据，以key－value形式增加，仅支持String，在任务执行时传递给任务执行类</td>
  </tr>
  <tr>
    <td>replace</td>
    <td>True</td>
		<td>boolean</td>
    <td>无</td>
    <td>是否覆盖，为ture时，自动覆盖，为false时，如果存在会抛出异常</td>
  </tr>
</table>

**返回参数说明**

boolean 成功返回true，否则会抛出异常


### 指定执行类新增基于Cron表达式的定时任务 ###

**描述**

添加或覆盖基于Cron表达式的定时调度任务

**请求方法**

com.yonyou.iuap.dispatch.client.DispatchRemoteManager.add(CronTaskConfig cronTaskConfig, Class<? extends ITask> taskName, boolean replace)

**请求方式**

工具类调用  

**请求参数说明**

<table>
  <tr>
    <th>参数字段</th>
    <th>必选</th>
		<th>类型</th>
    <th>长度限制</th>
    <th>说明</th>
  </tr>
  <tr>
    <td>cronTaskConfig</td>
    <td>True</td>
		<td>CronTaskConfig</td>
    <td>无</td>
    <td>Cron表达式及相关参数</td>
  </tr>
  <tr>
    <td>taskName</td>
    <td>True</td>
		<td>Class</td>
    <td>无</td>
    <td>任务执行类</td>
  </tr>
  <tr>
    <td>replace</td>
    <td>True</td>
		<td>boolean</td>
    <td>无</td>
    <td>是否覆盖，为ture时，自动覆盖，为false时，如果存在会抛出异常</td>
  </tr>
</table>

**返回参数说明**

boolean 成功返回true，否则会抛出异常

### 指定执行类新增带参数基于Cron表达式的定时任务 ###

**描述**

添加或覆盖基于Cron表达式的定时调度任务

**请求方法**

com.yonyou.iuap.dispatch.client.DispatchRemoteManager.add(CronTaskConfig cronTaskConfig, Class<? extends ITask> taskName, Map<String, String> data, boolean replace)

**请求方式**

工具类调用  

**请求参数说明**

<table>
  <tr>
    <th>参数字段</th>
    <th>必选</th>
		<th>类型</th>
    <th>长度限制</th>
    <th>说明</th>
  </tr>
  <tr>
    <td>cronTaskConfig</td>
    <td>True</td>
		<td>CronTaskConfig</td>
    <td>无</td>
    <td>Cron表达式及相关参数</td>
  </tr>
  <tr>
    <td>taskName</td>
    <td>True</td>
		<td>Class</td>
    <td>无</td>
    <td>任务执行类</td>
  </tr>
  <tr>
    <td>data</td>
    <td>FALSE</td>
		<td>Map<String, String></td>
    <td>无</td>
    <td>任务附加数据，以key－value形式增加，仅支持String，在任务执行时传递给任务执行类</td>
  </tr>
  <tr>
    <td>replace</td>
    <td>True</td>
		<td>boolean</td>
    <td>无</td>
    <td>是否覆盖，为ture时，自动覆盖，为false时，如果存在会抛出异常</td>
  </tr>
</table>

**返回参数说明**

boolean 成功返回true，否则会抛出异常

### 指定任务Beanid新增基于Cron表达式的定时任务 ###

**描述**

添加或覆盖基于Cron表达式的定时调度任务

**请求方法**

com.yonyou.iuap.dispatch.client.DispatchRemoteManager.add(CronTaskConfig cronTaskConfig,String taskBeanId,Map<String,String> data, boolean replace)

**请求方式**

工具类调用  

**请求参数说明**

<table>
  <tr>
    <th>参数字段</th>
    <th>必选</th>
		<th>类型</th>
    <th>长度限制</th>
    <th>说明</th>
  </tr>
  <tr>
    <td>cronTaskConfig</td>
    <td>True</td>
		<td>CronTaskConfig</td>
    <td>无</td>
    <td>任务执行类</td>
  </tr>
  <tr>
    <td>taskBeanId</td>
    <td>True</td>
		<td>String</td>
    <td>无</td>
    <td>任务执行类BeanId(Spring中配置的BeanID，实现类需要继承com.yonyou.iuap.dispatch.client.ITask接口</td>
  </tr>
  <tr>
    <td>data</td>
    <td>FALSE</td>
		<td>Map<String, String></td>
    <td>无</td>
    <td>任务附加数据，以key－value形式增加，仅支持String，在任务执行时传递给任务执行类</td>
  </tr>
  <tr>
    <td>replace</td>
    <td>True</td>
		<td>boolean</td>
    <td>无</td>
    <td>是否覆盖，为ture时，自动覆盖，为false时，如果存在会抛出异常</td>
  </tr>
</table>

**返回参数说明**

boolean 成功返回true，否则会抛出异常

### 暂停任务 ###

**描述**

 根据任务名称和组名称暂停任务

**请求方法**

com.yonyou.iuap.dispatch.client.DispatchRemoteManager.pauseJob(String jobName, String groupName)

**请求方式**

工具类调用  

**请求参数说明**

<table>
  <tr>
    <th>参数字段</th>
    <th>必选</th>
		<th>类型</th>
    <th>长度限制</th>
    <th>说明</th>
  </tr>
  <tr>
    <td>jobName</td>
    <td>True</td>
		<td>String</td>
    <td>无</td>
    <td>任务名称</td>
  </tr>
  <tr>
    <td>groupName</td>
    <td>True</td>
		<td>String</td>
    <td>无</td>
    <td>组名称</td>
  </tr>
</table>

**返回参数说明**

暂停成功返回NULL，否则返回具体异常原因

### 恢复任务 ###

**描述**

 根据任务名称和组名称恢复任务

**请求方法**

com.yonyou.iuap.dispatch.client.DispatchRemoteManager.resumeJob(String jobName, String groupName)

**请求方式**

工具类调用  

**请求参数说明**

<table>
  <tr>
    <th>参数字段</th>
    <th>必选</th>
		<th>类型</th>
    <th>长度限制</th>
    <th>说明</th>
  </tr>
  <tr>
    <td>jobName</td>
    <td>True</td>
		<td>String</td>
    <td>无</td>
    <td>任务名称</td>
  </tr>
  <tr>
    <td>groupName</td>
    <td>True</td>
		<td>String</td>
    <td>无</td>
    <td>组名称</td>
  </tr>
</table>

**返回参数说明**

暂停成功返回NULL，否则返回具体异常原因

### 删除任务 ###

**描述**

 根据任务名称和组名称删除任务

**请求方法**

com.yonyou.iuap.dispatch.client.DispatchRemoteManager.deleteJob(String jobName, String groupName)

**请求方式**

工具类调用  

**请求参数说明**

<table>
  <tr>
    <th>参数字段</th>
    <th>必选</th>
		<th>类型</th>
    <th>长度限制</th>
    <th>说明</th>
  </tr>
  <tr>
    <td>jobName</td>
    <td>True</td>
		<td>String</td>
    <td>无</td>
    <td>任务名称</td>
  </tr>
  <tr>
    <td>groupName</td>
    <td>True</td>
		<td>String</td>
    <td>无</td>
    <td>组名称</td>
  </tr>
</table>

**返回参数说明**

暂停成功返回NULL，否则返回具体异常原因

### 立即执行任务 ###

**描述**

 根据任务名称和组名称立即执行任务

**请求方法**

com.yonyou.iuap.dispatch.client.DispatchRemoteManager.triggerJob(String jobName, String groupName)

**请求方式**

工具类调用  

**请求参数说明**

<table>
  <tr>
    <th>参数字段</th>
    <th>必选</th>
		<th>类型</th>
    <th>长度限制</th>
    <th>说明</th>
  </tr>
  <tr>
    <td>jobName</td>
    <td>True</td>
		<td>String</td>
    <td>无</td>
    <td>任务名称</td>
  </tr>
  <tr>
    <td>groupName</td>
    <td>True</td>
		<td>String</td>
    <td>无</td>
    <td>组名称</td>
  </tr>
</table>

**返回参数说明**

暂停成功返回NULL，否则返回具体异常原因
