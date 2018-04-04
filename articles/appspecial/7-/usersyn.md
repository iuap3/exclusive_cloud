# API接口文档规范



## 接口名称


```
用户新增修改删除等同步接口；用户编码、手机号、邮箱要求唯一
```

## 接口说明

```
通过同步接口，第三方系统能够实现用户得基础数据得对接；
需要通过非加签的方式进行调用；需要设置用户认证信息，请求的cookie中需要包含上下文信息
```

## 请求方式


```
1.POST /wbalone/userMGT/create：新增一个用户
如果id为空，会自动设置id、registerDate、createDate、createPerson、roles、states、islock、avator、tenantId、password、salt
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| id | String | N | 主键（新增为null） |
| loginName | String | Y | 用户编码 |
| name | String | Y | 用户名称 |
| phone | String | Y | 电话 |
| email | String | Y | 邮件 |
| remark | String | N | 备注 |


## 返回参数

```
{
  "status" : 1,
  "msg" : "保存成功!",
  "data" : "cd879720bc6b4493b55c5fb6daf76ec0"
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | data的内容为保存用户后的主键|

## 调用示例
依赖workbench-model、eiap-plus-common
```
JsonResponse result = RestUtils.getInstance().doPost(url, null, JsonResponse.class);
```


## 请求方式


```
2.POST /wbalone/userMGT4syn/createByObj：新增一个用户；能够传递更多参数
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| id | String | N | 主键 |
| loginName | String | Y | 用户编码 |
| name | String | Y | 用户名称 |
| phone | String | Y | 电话 |
| email | String | Y | 邮件 |
| remark | String | N | 备注 |
| registerDate | String | N | 注册日期 |
| createDate | String | N | 创建日期 |
| createPerson | String | N | 创建人 |
| roles | String | N | 角色 |
| states | String | N | 状态 |
| islock | String | N | 是否锁定 |
| tenantId | String | N | 租户 |
| avator | String | N | 头像 |


## 返回参数

```
{
  "status" : 1,
  "msg" : "保存成功!",
  "data" : "cd879720bc6b4493b55c5fb6daf76ec0"
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | data的内容为保存用户后的主键|

## 调用示例
依赖workbench-model、eiap-plus-common
```
String url ="http://192.168.79.161:7777/wbalone/userMGT4syn/createByObj"
WBUser user = new WBUser();
user.setName("userSyn20");
user.setLoginName("userSyn20");
user.setPhone("13121681357");
user.setEmail("liberty88@yonyou.com");
JsonResponse  result = RestUtils.getInstance().doPost(url, user, JsonResponse.class);
```

## 请求方式


```
3.GET /wbalone/userMGT/optUserState/{id}?states=0
states:启用停用状态 1表示启用，0表示停用,属性类型string
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| id | String | Y | 主键 |
| islock | String | Y | 用户编码 |


## 返回参数

```
{
  "status" : 1,
  "msg" : "保存成功!"
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 

## 调用示例
依赖workbench-model、eiap-plus-common
```
String url ="http://172.30.3.76:8880/wbalone/userMGT/optUserState/b142e2aaf51d45bcb599e4cd8bdc186b?states=0"
JsonResponse  result = RestUtils.getInstance().doGet(url, user, JsonResponse.class);
```

## 请求方式


```
4.POST /wbalone/userMGT4syn/updateByObj 
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| id | String | Y | 主键 |
| loginName | String | N | 用户编码 |
| name | String | N | 用户名称 |
| phone | String | N | 电话 |
| email | String | N | 邮件 |
| remark | String | N | 备注 |
| createDate | String | N | 创建日期 |
| createPerson | String | N | 创建人 |
| tenantId | String | N | 租户 |
| avator | String | N | 头像 |
| modifyDate | String | N | 注册日期 |
| modifyPerson | String | N | 注册日期 |

## 返回参数

```
{
  "status" : 1,
  "msg" : "修改成功！"
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 

## 调用示例
依赖workbench-model、eiap-plus-common
```
String url = "http://192.168.79.161:7777/wbalone/userMGT4syn/updateByObj";
WBUser user = new WBUser();
user.setId("3beb5ce0cfbe4b46a16be83a5b5c3c2a");
user.setName("name_updateByObj");
JsonResponse  result = RestUtils.getInstance().doPost(url, user, JsonResponse.class);
```


## 请求方式


```
5.POST /wbalone/userMGT4syn/update 
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| id | String | Y | 主键 |
| loginName | String | N | 用户编码 |
| name | String | N | 用户名称 |
| phone | String | N | 电话 |
| email | String | N | 邮件 |
| remark | String | N | 备注 |
| createDate | String | N | 创建日期 |
| createPerson | String | N | 创建人 |
| tenantId | String | N | 租户 |
| avator | String | N | 头像 |
| modifyDate | String | N | 注册日期 |
| modifyPerson | String | N | 注册日期 |

## 返回参数

```
{
  "status" : 1,
  "msg" : "修改成功！"
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 

## 调用示例
依赖workbench-model、eiap-plus-common
```
String url = "http://192.168.79.161:7777/wbalone/userMGT4syn/update";
String user = "{}";
JsonResponse  result = RestUtils.getInstance().doPost(url, user, JsonResponse.class);
```


## 请求方式


```
6.GET /wbalone/userMGT/delete/{id}
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| id | String | Y | 主键 |

## 返回参数

```
{
  "status" : 1,
  "msg" : "删除成功！"
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 

## 调用示例
依赖workbench-model、eiap-plus-common
```
String url = "http://192.168.79.161:7777/wbalone/userMGT/delete/cd879720bc6b4493b55c5fb6daf76ec0";
JsonResponse  result = RestUtils.getInstance().doPost(url, null, JsonResponse.class);
```
