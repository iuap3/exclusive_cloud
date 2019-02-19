

**<font face="宋体">基础档案组件</font>**



# 第六章 **<font face="宋体">开放接口规范</font>**



<font face="宋体">以下接口通过</font>workbench-sdk<font face="宋体">开放， 可以调用以下接口</font>

![](images/20190218122701516-21.jpg)

<font face="宋体">图18</font> 

<font face="宋体">以组织为例，提供</font>rest<font face="宋体">服务有两个类</font><font face="Times New Roman">OrgRest</font><font face="宋体">，</font><font face="Times New Roman">OrgRestWithSign</font><font face="宋体">，</font><font face="Times New Roman">OrgRest</font><font face="宋体">是普通的</font><font face="Times New Roman">http</font><font face="宋体">服务调用，服务端会验证</font><font face="Times New Roman">token,sdk</font><font face="宋体">把用户的请求转发给了服务端。服务端会知道当前用户是谁。</font><font face="Times New Roman">OrgRestWithSign</font><font face="宋体">是一种无状态请求，服务端不校验用户，只会校验是否来自可信的客户端。服务端和客户端通过同一份</font><font face="Times New Roman">authfile.txt</font><font face="宋体">进行校验。具体的方法有以下几种。</font>



<font face="宋体">首先描述提供那些接口？比如有没有批量添加人员的接口？然后那些接口怎么调用。</font>

## 六.1 **<font face="宋体">基础档案</font>sdk<font face="宋体">接口</font>**



<font face="宋体">使用</font>sdk<font face="宋体">调用基础档案服务时需要</font>

1.<font face="宋体">添加以下依赖</font>

|         <dependency>           <groupId>com.yonyou.iuap.pap.workbench</groupId>      workbench-sdk       <version>${project.version}</version>        </dependency> |
| --- |



2.<font face="宋体">由于</font><font face="Times New Roman">sdk</font><font face="宋体">使用</font><font face="Times New Roman">workbench-sdk</font><font face="宋体">进行加签调用</font><font face="Times New Roman">pap_basedoc</font><font face="宋体">的服务，所以需要配置服务地址与加签相关信息</font>

#pap_basedoc url

papBasedoc.base.url=127.0.0.1:8080/pap_basedoc



### 六.1.1 **<font face="宋体">组织</font><font face="宋体">加签调用</font>**

<font face="宋体">类：</font>com.yonyou.uap.wb.sdk.OrgRestWithSign

<font face="宋体">所有的返回结果都是</font> json <font face="宋体">格式为</font><font face="Times New Roman">{“data”:””,”status”:</font>1,”msg”:”msg”}

status:<font face="宋体">状态</font><font face="Times New Roman">1</font><font face="宋体">：成功，</font><font face="Times New Roman">0</font><font face="宋体">：失败</font>

msg:<font face="宋体">消息</font>

data:<font face="宋体">里是具体数据。以下返回值都指的是</font><font face="Times New Roman">data</font><font face="宋体">里面的数据</font>

![](images/20190218122701516-22.jpg)

<font face="宋体">图19</font> 

#### 六.1.1.1 **<font face="宋体">创建组织</font>**

create(Map<String, String>, String)<font face="宋体">或</font><font face="Times New Roman">createObj(Map<String, String>, String)</font>

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

**<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>**

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| parent_id | String | 是 | 父节点<font face="Times New Roman">id</font> |
| name | String | 是 | 名称 |
| code | String | 是 | 编码 |
| short_name | String | 否 | 简称 |
| effective_date | String | 是 | 生效日期<font face="Times New Roman">yyyy-MM-dd HH:mm:ss</font> |
| principal | String | 否 | 负责人<font face="Times New Roman">id</font> |
| contact | String | 否 | 联系人 |
| contact_phone | String | 否 | 联系人电话 |
| contact_address | String | 否 | 联系地址 |
| description | String | 否 | 描述 |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 否 | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |

**data<font face="宋体">返回值：</font>**

**<font face="宋体">无</font>**

****

<font face="宋体">返回值：</font>

#### 六.1.1.2 **<font face="宋体">修改组织</font>**

updateByParam(Map<String, String>, String)



**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

**<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>**

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| id | String | 是 | 节点<font face="Times New Roman">id</font> |
| name | String | 是 | 名称 |
| code | String | 是 | 编码 |
| short_name | String | 否 | 简称 |
| effective_date | String | 是 | 生效日期<font face="Times New Roman">yyyy-MM-dd HH:mm:ss</font> |
| principal | String | 否 | 负责人<font face="Times New Roman">id</font> |
| contact | String | 否 | 联系人 |
| contact_phone | String | 否 | 联系人电话 |
| contact_address | String | 否 | 联系地址 |
| description | String | 否 | 描述 |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 否 | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |

**data<font face="宋体">返回值：</font>**

**<font face="宋体">无</font>**









#### 六.1.1.3 **<font face="宋体">删除组织</font>**

delete(Map<String, String>, String)



**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| id | String | 是 | 节点<font face="Times New Roman">id</font> |

**data<font face="宋体">返回值：</font>**

**<font face="宋体">无</font>**





#### 六.1.1.4 **<font face="宋体">查询所有组织</font>**

getAll(Map<String, String>, String)<font face="宋体">：获取所有的组织</font>



**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>


| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |

**data<font face="宋体">返回值：</font>**

**List:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **说明**|
| --- | --- | --- |
| parent_id | String | 父节点<font face="Times New Roman">id</font> |
| name | String | 名称 |
| code | String | 编码 |
| short_name | String | 简称 |
| effective_date | String | 生效时间 |
| principal | String | 负责人<font face="Times New Roman">id</font> |
| contact | String | 联系人 |
| contact_phone | String | 联系人电话 |
| contact_address | String | 联系地址 |
| description | String | 描述 |
| tenant_id | String | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |

#### 六.1.1.5 **<font face="宋体">根据</font>code <font face="宋体">查询组织</font>**

getByCode(Map<String, String> params, String prefix)<font face="宋体">：根据</font><font face="Times New Roman">code</font> <font face="宋体">查询组织</font>

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数** | **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| code | String | 是 | 组织编码|
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |

**data<font face="宋体">返回值：</font>**

**Organization:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **说明**|
| --- | --- | --- |
| parent_id | String | 父节点<font face="Times New Roman">id</font> |
| name | String | 名称 |
| code | String | 编码 |
| short_name | String | 简称 |
| effective_date | String | 生效时间 |
| principal | String | 负责人<font face="Times New Roman">id</font> |
| contact | String | 联系人 |
| contact_phone | String | 联系人电话 |
| contact_address | String | 联系地址 |
| description | String | 描述 |
| tenant_id | String | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |





#### 六.1.1.6 **<font face="宋体">根据</font>id<font face="宋体">查询</font>**

getById(Map<String, String> params, String prefix)<font face="宋体">：根据</font><font face="Times New Roman">id</font><font face="宋体">查询</font>

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型** | **是否必须**| **说明**|
| --- | --- | --- | --- |
| id | String | 是| 组织<font face="Times New Roman">id</font> |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |

**data<font face="宋体">返回值：</font>**

**Organization:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **说明** |
| --- | --- | --- |
| parent_id | String | 父节点<font face="Times New Roman">id</font> |
| name | String | 名称 |
| code | String | 编码 |
| short_name | String | 简称 |
| effective_date | String | 生效时间 |
| principal | String | 负责人<font face="Times New Roman">id</font> |
| contact | String | 联系人 |
| contact_phone | String | 联系人电话 |
| contact_address | String | 联系地址 |
| description | String | 描述 |
| tenant_id | String | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |



#### 六.1.1.7 **<font face="宋体">根据</font>ids<font face="宋体">批量获取组织</font>**

getByIds(Map<String, String> params, String prefix)<font face="宋体">：根据</font><font face="Times New Roman">ids</font><font face="宋体">批量获取组织</font>

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型**| **是否必须** | **说明**|
| --- | --- | --- | --- |
| Ids | String| 是| 组织<font face="Times New Roman">ids,</font><font face="宋体">数组</font><font face="Times New Roman">json</font>|
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |

**data<font face="宋体">返回值：</font>**

**list:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **说明**|
| --- | --- | --- |
| parent_id | String | 父节点<font face="Times New Roman">id</font> |
| name | String | 名称 |
| code | String | 编码 |
| short_name | String | 简称 |
| effective_date | String | 生效时间 |
| principal | String | 负责人<font face="Times New Roman">id</font> |
| contact | String | 联系人 |
| contact_phone | String | 联系人电话 |
| contact_address | String | 联系地址 |
| description | String | 描述 |
| tenant_id | String | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |









#### 六.1.1.8 **<font face="宋体">根据</font>name<font face="宋体">字段模糊查询</font>**

likeSearch(Map<String, String> params, String prefix):<font face="宋体">根据</font><font face="Times New Roman">name</font><font face="宋体">字段模糊查询</font>

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型** | **是否必须**| **说明**|
| --- | --- | --- | --- |
| name | String | 否 | 组织名称 |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |

**data<font face="宋体">返回值：</font>**

**list:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **说明**|
| --- | --- | --- |
| parent_id | String | 父节点<font face="Times New Roman">id</font> |
| name | String | 名称 |
| code | String | 编码 |
| short_name | String | 简称 |
| effective_date | String | 生效时间 |
| principal | String | 负责人<font face="Times New Roman">id</font> |
| contact | String | 联系人 |
| contact_phone | String | 联系人电话 |
| contact_address | String | 联系地址 |
| description | String | 描述 |
| tenant_id | String | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |



### 六.1.2 **<font face="宋体">组织</font>**<font face="宋体">加签</font>**<font face="宋体">调用</font>**

<font face="宋体">类：</font>com.yonyou.uap.wb.sdk.OrgRest

Sdk<font face="宋体">会把来自浏览器或客户端的</font><font face="Times New Roman">http</font><font face="宋体">的</font><font face="Times New Roman">head</font><font face="宋体">转发给基础档案，所以基础档案能知道会话的用户信息。会根据用户具有的数据不同显示不同的结果。</font>

![](images/20190218122701516-23.jpg)

<font face="宋体">图20</font>

<font face="宋体">和组织的加签调用对应的方法功能一样，参数不用第二个。</font>

<font face="宋体">返回对象为</font>com.alibaba.fastjson.JSONObject <font face="宋体">，数据格式为</font><font face="Times New Roman">{“data”:””,”status”:”success”,”msg”:”msg”}</font>

status:<font face="宋体">状态</font><font face="Times New Roman">1</font><font face="宋体">：成功，</font><font face="Times New Roman">0</font><font face="宋体">：失败</font>

msg:<font face="宋体">消息</font>

data:





### 六.1.3 **<font face="宋体">部门加签调用</font>**

<font face="宋体">类：</font>com.yonyou.uap.wb.sdk.DeptRestWithSign

<font face="宋体">所有的返回结果都是</font> json <font face="宋体">格式为</font><font face="Times New Roman">{“data”:””,”status”:</font>1,”msg”:”msg”}

status:<font face="宋体">状态</font><font face="Times New Roman">1</font><font face="宋体">：成功，</font><font face="Times New Roman">0</font><font face="宋体">：失败</font>

msg:<font face="宋体">消息</font>

data:<font face="宋体">里是具体数据。一下返回值都指的是</font><font face="Times New Roman">data</font><font face="宋体">里面的数据</font>

![](images/20190218122701516-24.jpg)

<font face="宋体">图21</font> 

#### 六.1.3.1 **<font face="宋体">创建部门</font>**

<font face="宋体">方法：</font>create(Map<String, String> params, String prefix)

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

**<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>**

****

| **参数** | **参数类型**| **是否必须**| **说明** |
| --- | --- | --- | --- |
| organization_id | String| 是| 组织<font face="Times New Roman">id</font>|
| parent_id | String | 否 | 父节点<font face="Times New Roman">id</font> |
| name | String | 是 | 名称 |
| code | String | 是 | 编码 |
| short_name | String | 否 | 简称 |
| effective_date | String | 是 | 生效日期<font face="Times New Roman">yyyy-MM-dd HH:mm:ss</font> |
| principal | String | 否 | 负责人<font face="Times New Roman">id</font> |
| description | String | 否 | 描述 |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 否 | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |

**data<font face="宋体">返回值：</font>**

**<font face="宋体">无</font>**

#### 六.1.3.2 **<font face="宋体">删除部门</font>**

<font face="宋体">方法：</font>delete(Map<String, String> params, String prefix)

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

**<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>**

****

| **参数**| **参数类型**| **是否必须**| **说明** |
| --- | --- | --- | --- |
| id| String| 是| 组织<font face="Times New Roman">id</font>|

**data<font face="宋体">返回值：</font>**

**<font face="宋体">无</font>**

#### 六.1.3.3 **<font face="宋体">批量删除</font>**

<font face="宋体">方法：</font>batchDelete(Map<String, String> params, String prefix)

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

**<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>**

****

| **参数**| **参数类型**| **是否必须**| **说明** |
| --- | --- | --- | --- |
| **Ids** | String | 是| 组织<font face="Times New Roman">id</font><font face="宋体">数组转的</font><font face="Times New Roman">json</font>|

**data<font face="宋体">返回值：</font>**

**<font face="宋体">无</font>**



#### 六.1.3.4 **<font face="宋体">更新部门</font>**

<font face="宋体">方法：</font>update(Map<String, String> params, String prefix)

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

**<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>**

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| id | String | 是 | 部门<font face="Times New Roman">id</font> |
| organization_id | String | 是 | 组织<font face="Times New Roman">id</font>**** |
| parent_id | String | 否 | 父节点<font face="Times New Roman">id</font> |
| name | String | 是 | 名称 |
| code | String | 是 | 编码 |
| short_name | String | 否 | 简称 |
| effective_date | String | 是 | 生效日期<font face="Times New Roman">yyyy-MM-dd HH:mm:ss</font> |
| principal | String | 否 | 负责人<font face="Times New Roman">id</font> |
| description | String | 否 | 描述 |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 否 | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |

**data<font face="宋体">返回值：</font>**

**<font face="宋体">无</font>**

#### 六.1.3.5 **<font face="宋体">获取所有</font>**

<font face="宋体">方法：</font>getAll(Map<String, String> params, String prefix)

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |

**data<font face="宋体">返回值：</font>**

**List:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| id | String | 是 | 部门<font face="Times New Roman">id</font> |
| organization_id | String | 是 | 组织<font face="Times New Roman">id</font>**** |
| parent_id | String | 否 | 父节点<font face="Times New Roman">id</font> |
| name | String | 是 | 名称 |
| code | String | 是 | 编码 |
| short_name | String | 否 | 简称 |
| effective_date | String | 是 | 生效日期<font face="Times New Roman">yyyy-MM-dd HH:mm:ss</font> |
| principal | String | 否 | 负责人<font face="Times New Roman">id</font> |
| description | String | 否 | 描述 |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 否 | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |



#### 六.1.3.6 **<font face="宋体">根据组织获取部门</font>**

<font face="宋体">方法：</font>getAllByOrg(Map<String, String>, String)



**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型** | **是否必须**| **说明**|
| --- | --- | --- | --- |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| orgId | String | 否 | 组织<font face="Times New Roman">id</font> |

**data<font face="宋体">返回值：</font>**

**List:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| id | String | 是 | 部门<font face="Times New Roman">id</font> |
| organization_id | String| 是 | 组织<font face="Times New Roman">id</font> |
| parent_id | String | 否 | 父节点<font face="Times New Roman">id</font> |
| name | String | 是 | 名称 |
| code | String | 是 | 编码 |
| short_name | String | 否 | 简称 |
| effective_date | String | 是 | 生效日期<font face="Times New Roman">yyyy-MM-dd HH:mm:ss</font> |
| principal | String | 否 | 负责人<font face="Times New Roman">id</font> |
| description | String | 否 | 描述 |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 否 | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |



#### 六.1.3.7 **<font face="宋体">根据</font>id<font face="宋体">批量查询</font>**

<font face="宋体">方法：</font>getByIds(Map<String, String> params, String prefix)



**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| Ids | String | 是 | 部门<font face="Times New Roman">id</font><font face="宋体">数组的</font><font face="Times New Roman">json</font> |

**data<font face="宋体">返回值：</font>**

**List:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| id | String | 是 | 部门<font face="Times New Roman">id</font> |
| organization_id| String| 是| 组织<font face="Times New Roman">id</font>**** |
| parent_id | String | 否 | 父节点<font face="Times New Roman">id</font> |
| name | String | 是 | 名称 |
| code | String | 是 | 编码 |
| short_name | String | 否 | 简称 |
| effective_date | String | 是 | 生效日期<font face="Times New Roman">yyyy-MM-dd HH:mm:ss</font> |
| principal | String | 否 | 负责人<font face="Times New Roman">id</font> |
| description | String | 否 | 描述 |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 否 | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |

#### 六.1.3.8 **<font face="宋体">根据名称模糊查询</font>**

<font face="宋体">方法：</font>likeSearch(Map<String, String> params, String prefix)

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数** | **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| name | String | 否 | 部门名称 |

**data<font face="宋体">返回值：</font>**

**List:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| id | String | 是 | 部门<font face="Times New Roman">id</font> |
| organization_id| String| 是| 组织<font face="Times New Roman">id</font>|
| parent_id | String | 否 | 父节点<font face="Times New Roman">id</font> |
| name | String | 是 | 名称 |
| code | String | 是 | 编码 |
| short_name | String | 否 | 简称 |
| effective_date | String | 是 | 生效日期<font face="Times New Roman">yyyy-MM-dd HH:mm:ss</font> |
| principal | String | 否 | 负责人<font face="Times New Roman">id</font> |
| description | String | 否 | 描述 |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 否 | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |





#### 六.1.3.9 **<font face="宋体">根据根据内部编码模糊查询</font>**

<font face="宋体">方法：</font>getByInnerCodeLike(Map<String, String> queryParams)



**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| innerCode | String | 否 | 部门内码 |

**data<font face="宋体">返回值：</font>**

**List:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| id | String | 是 | 部门<font face="Times New Roman">id</font> |
| organization_id| String| 是| 组织<font face="Times New Roman">id</font>|
| parent_id | String | 否 | 父节点<font face="Times New Roman">id</font> |
| name | String | 是 | 名称 |
| code | String | 是 | 编码 |
| short_name | String | 否 | 简称 |
| effective_date | String | 是 | 生效日期<font face="Times New Roman">yyyy-MM-dd HH:mm:ss</font> |
| principal | String | 否 | 负责人<font face="Times New Roman">id</font> |
| description | String | 否 | 描述 |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 否 | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |



### 六.1.4 **<font face="宋体">部门</font>rest<font face="宋体">调用</font>**

<font face="宋体">类：</font>com.yonyou.uap.wb.sdk.DeptRest



Sdk<font face="宋体">会把来自浏览器或客户端的</font><font face="Times New Roman">http</font><font face="宋体">的</font><font face="Times New Roman">head</font><font face="宋体">转发给基础档案，所以基础档案能知道会话的用户信息，会根据用户具有的数据不同显示不同的结果。</font>



![](images/20190218122701516-25.jpg)

<font face="宋体">图22</font>



<font face="宋体">和部门的加签调用对应的方法功能一样，参数不用第二个。</font>

<font face="宋体">返回对象为</font>com.alibaba.fastjson.JSONObject <font face="宋体">，数据格式为</font><font face="Times New Roman">{“data”:””,”status”:”success”,”msg”:”msg”}</font>

status:<font face="宋体">状态</font><font face="Times New Roman">1</font><font face="宋体">：成功，</font><font face="Times New Roman">0</font><font face="宋体">：失败</font>

msg:<font face="宋体">消息</font>

data:









### 六.1.5 **<font face="宋体">人员加签调用</font>**

<font face="宋体">类：</font>com.yonyou.uap.wb.sdk.StaffRestWithSign

<font face="宋体">所有的返回结果都是</font> json <font face="宋体">格式为</font><font face="Times New Roman">{“data”:””,”status”:</font>1,”msg”:”msg”}

status:<font face="宋体">状态</font><font face="Times New Roman">1</font><font face="宋体">：成功，</font><font face="Times New Roman">0</font><font face="宋体">：失败</font>

msg:<font face="宋体">消息</font>

data:<font face="宋体">里是具体数据。一下返回值都指的是</font><font face="Times New Roman">data</font><font face="宋体">里面的数据</font>



![](images/20190218122701516-26.jpg)

<font face="宋体">图23</font> 



#### 六.1.5.1 **<font face="宋体">人员增</font>/<font face="宋体">改</font>**

<font face="宋体">（注：由于实现了国际化，人员加签调用的时候添加了字段</font> name2 <font face="宋体">、</font><font face="Calibri">name3</font><font face="宋体">、</font><font face="Calibri">name4</font><font face="宋体">、</font><font face="Calibri">name5</font><font face="宋体">、</font><font face="Calibri">name6</font> <font face="宋体">创建人员的时候 根据项目具体情况结合默认语种与选择语种可以选择性，在相应的字段中添加内容</font><font face="宋体">）</font>



<font face="宋体">方法：</font>save(Map<String, String> params, String prefix) <font face="宋体">参数</font>

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

**<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>**

**<font face="宋体">格式是一下</font>json <font face="宋体">转的</font><font face="Times New Roman">map</font>**

{

    "id": "71ca2a89-eec7-4f79-acca-15d5737987b0",

    "code": "01",

    "name": "<font face="宋体">花</font>01-<font face="宋体">人员</font>",

"name2": "<font face="宋体">数据多语</font>",

"name3": "<font face="宋体">数据多语</font>",

"name4": "<font face="宋体">数据多语</font>",

"name5": "<font face="宋体">数据多语</font>",

"name6": "<font face="宋体">数据多语</font>",

    "parentid": null,

    "innercode": null,

    "creator": "U001",

    "creationtime": 1539583765457,

    "modifier": null,

    "creatorName": "<font face="宋体">系统管理员</font>",

    "modifierName": "",

    "modifiedtime": null,

    "tenantid": "tenant",

    "sysid": "wbalone",

    "enable": 0,

    "ts": null,

    "dr": 0,

    "avator": null,

    "certtypeid": "CT00001",

    "certnum": "370683199207019261",

    "gender": "G02",

    "birthdate": "1992-07-01",

    "maritalstatus": "",

    "educationbg": "E02",

    "email": "hua01@yonyou.com",

    "mobile": "13600000001",

    "participateworkdate": "2018-10-15",

    "userid": "3442453bfb4f49eebf95bd3ddfc0fe16",

    "userName": "<font face="宋体">花</font>01",

    "bdMainJobDo": [

        {

            "id": "8371e9e4-dd4e-4cc4-9893-8518574f4ecd",

            "code": null,

            "name": null,

            "parentid": null,

            "innercode": null,

            "creator": "U001",

            "creationtime": 1539583765457,

            "modifier": null,

            "modifiedtime": null,

            "tenantid": "tenant",

            "sysid": "wbalone",

            "enable": 0,

            "ts": null,

            "dr": 0,

            "staffid": "71ca2a89-eec7-4f79-acca-15d5737987b0",

            "orgid": "d0136bc3-fe19-4164-8886-3450856adf90",

            "orgName": "<font face="宋体">北京总部</font>",

            "deptid": "064c4e4f-e757-4c45-92ec-f1843fd1c47c",

            "deptName": "<font face="宋体">测试部</font>",

            "psnlcatgid": null,

            "dutyid": null,

            "positionid": "",

            "positionName": null,

            "rankid": null,

            "mainjob": null,

            "startservetime": "2018-10-15",

            "endservetime": ""

        }

    ]

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| id| String| 否| 人员<font face="Times New Roman">id</font><font face="宋体">（没有</font><font face="Times New Roman">id</font><font face="宋体">是新增，有修改）</font>|
| name | String | 是 | 名称 |
| code | String | 是 | 编码 |
| avator | String | 否 | 头像<font face="Times New Roman">url</font> |
| birthdate | String | 是 | 生效日期 |
| maritalstatus | String | 是 | 婚姻状况 |
| educationbg | String | 是 | 学历 |
| email | string | 是 | 邮箱 |
| mobile | string | 是 | 手机 |
| participateworkdate | string | 是 | 参加工作日期（格式2018-10-15） |
| <u>userid</u> | String | 否 | 关联用户<font face="Times New Roman">id</font> |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 否 | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |

**data<font face="宋体">返回值：</font>**

**<font face="宋体">无</font>**



params<font face="宋体">：</font><font face="Times New Roman">map</font><font face="宋体">。要保存</font><font face="Times New Roman">/</font><font face="宋体">修改对象的字段</font>

prefix<font face="宋体">：</font><font face="Times New Roman">null</font>

#### 六.1.5.2 **<font face="宋体">分页查询</font>**

<font face="宋体">方法：</font>pagination(Map<String, String> params, String prefix)

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| pageIndex | String | 否 | 当前查询页面 |
| pageSize | String | 否 | 每页条数 |
| keyword | Stirng | 否 | 查询关键字<font face="Times New Roman">code</font><font face="宋体">或</font><font face="Times New Roman">name</font><font face="宋体">的值</font> |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |

**data<font face="宋体">返回值：</font>**

**List:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **说明**|
| --- | --- | --- |
| id| String| 人员<font face="Times New Roman">id</font><font face="宋体">（没有</font><font face="Times New Roman">id</font><font face="宋体">是新增，有修改）</font> |
| name | String | 名称 |
| code | String | 编码 |
| avator | String | 头像<font face="Times New Roman">url</font> |
| birthdate | String | 生效日期 |
| maritalstatus | String | 婚姻状况 |
| educationbg | String | 学历 |
| email | string | 邮箱 |
| mobile | string | 手机 |
| participateworkdate | string | 参加工作日期（格式2018-10-15） |
| <u>userid</u> | String | 关联用户<font face="Times New Roman">id</font> |
| tenant_id | String | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |

#### 六.1.5.3 **<font face="宋体">根据</font>ids<font face="宋体">分页查询</font>**

<font face="宋体">方法：</font>paginationByIds(Map<String, String> queryParams)

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| pageIndex | String | 否 | 当前查询页面 |
| pageSize | String | 否 | 每页条数 |
| keyword | Stirng | 否 | 查询关键字<font face="Times New Roman">code</font><font face="宋体">或</font><font face="Times New Roman">name</font><font face="宋体">的值</font> |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| ids | String | 否 | Id<font face="宋体">集合</font><font face="Times New Roman">“,”</font><font face="宋体">分割的字符串</font> |

**data<font face="宋体">返回值：</font>**

**List:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **说明**|
| --- | --- | --- |
| id| String| 人员<font face="Times New Roman">id</font><font face="宋体">（没有</font><font face="Times New Roman">id</font><font face="宋体">是新增，有修改）</font>|
| name | String | 名称 |
| code | String | 编码 |
| avator | String | 头像<font face="Times New Roman">url</font> |
| birthdate | String | 生效日期 |
| maritalstatus | String | 婚姻状况 |
| educationbg | String | 学历 |
| email | string | 邮箱 |
| mobile | string | 手机 |
| participateworkdate | string | 参加工作日期（格式2018-10-15） |
| <u>userid</u> | String | 关联用户<font face="Times New Roman">id</font> |
| tenant_id | String | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |



#### 六.1.5.4 **<font face="宋体">根据</font>code<font face="宋体">码查询人员信息</font>**



<font face="宋体">方法：</font>getByCode(Map<String, String> params, String prefix)



**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| code | Stirng | 是 | 编码 |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |

**data<font face="宋体">返回值：</font>**

**object:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **说明**|
| --- | --- | --- |
| id| String | 人员<font face="Times New Roman">id</font><font face="宋体">（没有</font><font face="Times New Roman">id</font><font face="宋体">是新增，有修改）</font>|
| name | String | 名称 |
| code | String | 编码 |
| avator | String | 头像<font face="Times New Roman">url</font> |
| birthdate | String | 生效日期 |
| maritalstatus | String | 婚姻状况 |
| educationbg | String | 学历 |
| email | string | 邮箱 |
| mobile | string | 手机 |
| participateworkdate | string | 参加工作日期（格式2018-10-15） |
| <u>userid</u> | String | 关联用户<font face="Times New Roman">id</font> |
| tenant_id | String | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |



#### 六.1.5.5 **<font face="宋体">获取所有人员</font>**



listAll(Map<String, String> params, String prefix) <font face="宋体">获取所有人员</font>



**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
|  |  |  |  |

**data<font face="宋体">返回值：</font>**

**List:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **说明**|
| --- | --- | --- |
| id| String| 人员<font face="Times New Roman">id</font><font face="宋体">（没有</font><font face="Times New Roman">id</font><font face="宋体">是新增，有修改）</font> |
| name | String | 名称 |
| code | String | 编码 |
| avator | String | 头像<font face="Times New Roman">url</font> |
| birthdate | String | 生效日期 |
| maritalstatus | String | 婚姻状况 |
| educationbg | String | 学历 |
| email | string | 邮箱 |
| mobile | string | 手机 |
| participateworkdate | string | 参加工作日期（格式2018-10-15） |
| <u>userid</u> | String | 关联用户<font face="Times New Roman">id</font> |
| tenant_id | String | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |



#### 六.1.5.6 **<font face="宋体">根据</font>code<font face="宋体">或者</font><font face="Times New Roman">name</font><font face="宋体">模糊查询</font>**



<font face="宋体">方法：</font>listByCodeOrNameLike(Map<String, String> params, String prefix)



**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| keyword | Stirng | 否 | 查询关键字<font face="Times New Roman">code</font><font face="宋体">或</font><font face="Times New Roman">name</font><font face="宋体">的值</font> |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |

**data<font face="宋体">返回值：</font>**

**List:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **说明**|
| --- | --- | --- |
| id | String | 人员<font face="Times New Roman">id</font><font face="宋体">（没有</font><font face="Times New Roman">id</font><font face="宋体">是新增，有修改）</font>|
| name | String | 名称 |
| code | String | 编码 |
| avator | String | 头像<font face="Times New Roman">url</font> |
| birthdate | String | 生效日期 |
| maritalstatus | String | 婚姻状况 |
| educationbg | String | 学历 |
| email | string | 邮箱 |
| mobile | string | 手机 |
| participateworkdate | string | 参加工作日期（格式2018-10-15） |
| <u>userid</u> | String | 关联用户<font face="Times New Roman">id</font> |
| tenant_id | String | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |





#### 六.1.5.7 **<font face="宋体">根据内编码查模糊查询</font>**



<font face="宋体">方法：</font>listByInnercodeLike(Map<String, String> params, String prefix)



**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型** | **是否必须**| **说明**|
| --- | --- | --- | --- |
| innercode | Stirng | 否 | 内编码 |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |

**data<font face="宋体">返回值：</font>**

**List:<font face="宋体">内部数据</font>**

| **参数**| **参数类型** | **说明**|
| --- | --- | --- |
| id| String | 人员<font face="Times New Roman">id</font><font face="宋体">（没有</font><font face="Times New Roman">id</font><font face="宋体">是新增，有修改）</font>|
| name | String | 名称 |
| code | String | 编码 |
| avator | String | 头像<font face="Times New Roman">url</font> |
| birthdate | String | 生效日期 |
| maritalstatus | String | 婚姻状况 |
| educationbg | String | 学历 |
| email | string | 邮箱 |
| mobile | string | 手机 |
| participateworkdate | string | 参加工作日期（格式2018-10-15） |
| <u>userid</u> | String | 关联用户<font face="Times New Roman">id</font> |
| tenant_id | String | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |







#### 六.1.5.8 **<font face="宋体">根据</font>id<font face="宋体">批量查询人员</font>**

<font face="宋体">方法：</font>listByIds(Map<String, String> params, String prefix)



**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| ids | Stirng | 否 | 人员<font face="Times New Roman">id</font><font face="宋体">列表</font><font face="Times New Roman">“</font><font face="宋体">，</font><font face="Times New Roman">”</font><font face="宋体">分割</font> |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |

**data<font face="宋体">返回值：</font>**

**List:<font face="宋体">内部数据</font>**

| **参数**| **参数类型**| **说明**|
| --- | --- | --- |
| id| String| 人员<font face="Times New Roman">id</font><font face="宋体">（没有</font><font face="Times New Roman">id</font><font face="宋体">是新增，有修改）</font>|
| name | String | 名称 |
| code | String | 编码 |
| avator | String | 头像<font face="Times New Roman">url</font> |
| birthdate | String | 生效日期 |
| maritalstatus | String | 婚姻状况 |
| educationbg | String | 学历 |
| email | string | 邮箱 |
| mobile | string | 手机 |
| participateworkdate | string | 参加工作日期（格式2018-10-15） |
| <u>userid</u> | String | 关联用户<font face="Times New Roman">id</font> |
| tenant_id | String | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| sys_id | String | 系统<font face="Times New Roman">id</font> <font face="宋体">默认为</font><font face="Times New Roman">wbalone</font> |





#### 六.1.5.9 **<font face="宋体">删除人员</font>**





<font face="宋体">方法：</font>remove(Map<String, String> params, String prefix)

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型**| **是否必须**| **说明** |
| --- | --- | --- | --- |
| id | Stirng | 是 | 人员<font face="Times New Roman">id</font> |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |

**data<font face="宋体">返回值：</font>**

<font face="宋体">无</font>

#### 六.1.5.10 **<font face="宋体">批量停用用户方法</font>**



<font face="宋体">方法：</font>batchDisable(Map<String, String> params, String prefix)



**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| ids | Stirng | 是 | 人员<font face="Times New Roman">id</font><font face="宋体">列表</font><font face="Times New Roman">“</font><font face="宋体">，</font><font face="Times New Roman">”</font><font face="宋体">分割</font> |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |

**data<font face="宋体">返回值：</font>**

<font face="宋体">无</font>

#### 六.1.5.11 **<font face="宋体">批量启动用户</font>**

<font face="宋体">方法：</font>batchEnable(Map<String, String> params, String prefix)

<font face="宋体">参数</font>

**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| ids | Stirng | 是 | 人员<font face="Times New Roman">id</font><font face="宋体">列表</font><font face="Times New Roman">“</font><font face="宋体">，</font><font face="Times New Roman">”</font><font face="宋体">分割</font> |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |

**data<font face="宋体">返回值：</font>**

<font face="宋体">无</font>

#### 六.1.5.12 **<font face="宋体">停用</font>/<font face="宋体">启用用户</font>**

<font face="宋体">方法：</font>status(Map<String, String> params, String prefix)



**<font face="宋体">参数说明：</font>**

<font face="宋体">第二个参数传</font>null <font face="宋体">，</font><font face="Times New Roman">null</font><font face="宋体">会找默认的加签文件</font><font face="Times New Roman">WEB-INF\classes\authfile.txt</font>

<font face="宋体">第一个参数</font>map<font face="宋体">内的字段</font>

****

| **参数**| **参数类型**| **是否必须**| **说明**|
| --- | --- | --- | --- |
| id | Stirng | 是 | 人员<font face="Times New Roman">id</font> |
| tenant_id | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |
| status | string | 是 | 状态值（<font face="Times New Roman">1</font><font face="宋体">：启用，</font><font face="Times New Roman">2</font><font face="宋体">：禁用）</font> |

**data<font face="宋体">返回值：</font>**

<font face="宋体">无</font>



### 六.1.6 **<font face="宋体">人员</font>rest<font face="宋体">调用</font>**

<font face="宋体">类：</font>com.yonyou.uap.wb.sdk.UserRest

Sdk<font face="宋体">会把来自浏览器或客户端的</font><font face="Times New Roman">http</font><font face="宋体">的</font><font face="Times New Roman">head</font><font face="宋体">转发给基础档案，所以基础档案能知道会话的用户信息，会根据用户具有的数据不同显示不同的结果。</font>

![](images/20190218122701516-27.jpg)

<font face="宋体">图24</font> 

<font face="宋体">和人员的加签调用对应的方法功能一样，参数不用第二个。</font>

<font face="宋体">返回对象为</font>com.alibaba.fastjson.JSONObject <font face="宋体">，数据格式为</font><font face="Times New Roman">{“data”:””,”status”:”success”,”msg”:”msg”}</font>

status:<font face="宋体">状态</font><font face="Times New Roman">1</font><font face="宋体">：成功，</font><font face="Times New Roman">0</font><font face="宋体">：失败</font>

msg:<font face="宋体">消息</font>

data:



### 六.1.7 **<font face="宋体">岗位加签</font>****<font face="宋体">调用</font>**

<font face="宋体">类：</font>com.yonyou.uap.wb.sdk.BdPositionRestWithSign

#### 六.1.7.1 **<font face="宋体">创建岗位</font>**

<font face="宋体">方法</font>:create(BdPosition bdPosition)

<font face="宋体">参数</font>:<font face="宋体">岗位实体</font>



| **参数**| **参数类型** | **是否必须**| **说明** |
| --- | --- | --- | --- |
| code | String | 是 | 岗位编码 |
| orgId | String | 是 | 组织<font face="Times New Roman">id</font> |
| deptId | String | 是 | 部门id |
| name | String | 是 | 简体名称<font face="Times New Roman">(</font><font face="宋体">默认语种和当前语种必须</font><font face="Times New Roman">)</font> |
| name2 | String | 是 | 英文名称<font face="Times New Roman">(</font><font face="宋体">默认语种和当前语种必须</font><font face="Times New Roman">)</font> |
| name3 | String | 是 | 繁体名称(<font face="宋体">默认语种和当前语种必须</font><font face="Times New Roman">)</font> |
| tenantid | String | 否 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |

<font face="宋体">返回值</font>:<font face="宋体">保存的岗位实体 包含字段</font>

| **参数**| **参数类型** | **说明**|
| --- | --- | --- |
| id | String | 岗位id |
| code | String | 岗位编码 |
| orgId | String | 组织<font face="Times New Roman">id</font> |
| deptId | String | 部门id |
| name | String | 简体名称 |
| name2 | String | 英文名称 |
| name3 | String | 繁体名称 |
| tenantid | String | 租户<font face="Times New Roman">id</font> |
| ts | String | 时间戳 |

#### 六.1.7.2 **<font face="宋体">删除岗位</font>**

<font face="宋体">方法</font>:delete(String tenantId, String id)

<font face="宋体">参数</font>:

| **参数**| **参数类型** | **是否必须** | **说明**|
| --- | --- | --- | --- |
| tenantId | String | 否 | 租户<font face="Times New Roman">id</font> |
| id | Stirng | 是 | 岗位id |

<font face="宋体">返回集</font>:boolean . true:<font face="宋体">删除成功</font><font face="Times New Roman">.false:</font><font face="宋体">删除失败</font>

#### 六.1.7.3 **<font face="宋体">根据</font>id<font face="宋体">获取岗位</font>**



<font face="宋体">方法</font>:getById(String tenantId, String id)

<font face="宋体">参数</font>:

| **参数**| **参数类型** | **是否必须** | **说明** |
| --- | --- | --- | --- |
| tenantId | String | 是 | 租户<font face="Times New Roman">id</font> |
| id | Stirng | 是 | 岗位id |





<font face="宋体">返回结果</font>:<font face="宋体">岗位实体 包含字段</font>

| **参数**| **参数类型** | **说明**|
| --- | --- | --- |
| id | String | 岗位id |
| code | String | 岗位编码 |
| orgId | String | 组织<font face="Times New Roman">id</font> |
| deptId | String | 部门id |
| name | String | 简体名称 |
| name2 | String | 英文名称 |
| name3 | String | 繁体名称 |
| tenantid | String | 租户<font face="Times New Roman">id</font> |
| ts | String | 时间戳 |



#### 六.1.7.4 **<font face="宋体">根据</font>id<font face="宋体">更新岗位</font>**



<font face="宋体">方法</font>:update(BdPosition bdPosition)

<font face="宋体">参数</font>:<font face="宋体">岗位实体</font>



| **参数**| **参数类型** | **是否必须** | **说明**|
| --- | --- | --- | --- |
| id | String | 是 | 岗位id|
| code | String | 否 | 岗位编码 |
| orgId | String | 否 | 组织<font face="Times New Roman">id</font> |
| deptId | String | 否 | 部门id |
| name | String | 否 | 简体名称<font face="Times New Roman">(</font><font face="宋体">默认语种和当前语种必须</font><font face="Times New Roman">)</font> |
| name2 | String | 否 | 英文名称<font face="Times New Roman">(</font><font face="宋体">默认语种和当前语种必须</font><font face="Times New Roman">)</font> |
| name3 | String | 否 | 繁体名称(<font face="宋体">默认语种和当前语种必须</font><font face="Times New Roman">)</font> |
| tenantid | String | 是 | 租户<font face="Times New Roman">id</font><font face="宋体">，默认为</font><font face="Times New Roman">tenant</font> |



<font face="宋体">返回集</font>:boolean . true:<font face="宋体">删除成功</font><font face="Times New Roman">.false:</font><font face="宋体">删除失败</font>



#### 六.1.7.5 **<font face="宋体">根据</font>ids<font face="宋体">查询岗位</font>**



<font face="宋体">方法</font>:getByIds(String tenantId, List<String> ids)

<font face="宋体">参数</font>:

| **参数** | **参数类型** | **是否必须** | **说明** |
| --- | --- | --- | --- |
| tenantId | String | 是 | 租户<font face="Times New Roman">id</font> |
| ids | List<String> | 是 | 岗位ids |

<font face="宋体">返回集</font>:List<BdPosition> <font face="宋体">对象的字段</font>

| **参数** | **参数类型** | **说明**|
| --- | --- | --- |
| id | String | 岗位id |
| code | String | 岗位编码 |
| orgId | String | 组织<font face="Times New Roman">id</font> |
| deptId | String | 部门id |
| name | String | 简体名称 |
| name2 | String | 英文名称 |
| name3 | String | 繁体名称 |
| tenantid | String | 租户<font face="Times New Roman">id</font> |
| ts | String | 时间戳 |



#### 六.1.7.6 **<font face="宋体">根据</font>code<font face="宋体">查询岗位</font>**



<font face="宋体">方法</font>:getByCode(String tenantId, String code)

<font face="宋体">参数</font>:

| **参数**| **参数类型** | **是否必须** | **说明** |
| --- | --- | --- | --- |
| tenantId | String | 是 | 租户<font face="Times New Roman">id</font> |
| code | Stirng | 是 | 岗位code |





<font face="宋体">返回结果</font>:<font face="宋体">岗位实体 包含字段</font>

| **参数** | **参数类型** | **说明** |
| --- | --- | --- |
| id | String | 岗位id |
| code | String | 岗位编码 |
| orgId | String | 组织<font face="Times New Roman">id</font> |
| deptId | String | 部门id |
| name | String | 简体名称 |
| name2 | String | 英文名称 |
| name3 | String | 繁体名称 |
| tenantid | String | 租户<font face="Times New Roman">id</font> |
| ts | String | 时间戳 |



### 六.1.8 **<font face="宋体">岗位</font>**rest**<font face="宋体">调用</font>**

Sdk<font face="宋体">会把来自浏览器或客户端的</font><font face="Times New Roman">http</font><font face="宋体">的</font><font face="Times New Roman">head</font><font face="宋体">转发给基础档案，所以基础档案能知道会话的用户信息，</font><font face="宋体">会根据用户具有的数据不同显示不同的结果。</font>

<font face="宋体">方法签名和岗位加签一样</font>.

