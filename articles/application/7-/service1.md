# API接口文档规范

## 接口名称

```
组织、部门新增、修改、删除同步接口
```
## 接口说明



##一、组织新增、修改和删除：

（一）新增：提供两种参数同步接口

## 请求方式

1、url:http://192.168.79.161:7777/wbalone/org4syn/create
2、method:post

## 请求参数

{"id":"", "code":"org18","name":"org18","effective_date":"2017-06-22 00:00:00","principal":"jxsid002_U001","parent_id":""}

将请求的参数进行详细的说明：

| 参数  |参数类型|是否必须| 说明 |
| ---  | ----  | :---  | :--- |
| id   | String| N     | 主键 |
| code | String | Y    |编码 |
| name | String | Y    | 名称 |
|effective_date|String|Y|生效日期|
|principal|String|Y|负责人主键|
|parent_id|    | N |    |

## 返回参数

```
{"status":1,"msg":"保存成功!","data":"0729566c-3012-4e46-8ba1-bbcf5afe2e6d"}

```

## 调用示例

url: http://192.168.79.161:7777/wbalone/org4syn/createByObj


        String url = "http://192.168.79.161:7777/wbalone/org4syn/createByObj";

		Organization org = new Organization();

		org.setCode("orgsyn20");
		org.setName("orgsyn20");
		org.setEffective_date(new Date());
		org.setPrincipal("jxsid002_U001");

		JsonResponse result = RestUtils.getInstance().doPost(url, org, JsonResponse.class);

(二)修改：提供两种参数同步接口--只更新传递过来组织上有值的字段值

## 请求方式

1、url:http://192.168.79.161:7777/wbalone/org4syn/update
2、method:post

## 请求参数

{"id":"0729566c-3012-4e46-8ba1-bbcf5afe2e6d", "code":"org10","name":"org10"}

## 返回参数

{"status":1,"msg":"操作成功！"}

## 调用示例

（a）参数为对象

  String url = "http://192.168.79.161:7777/wbalone/org4syn/updateByObj";

		Organization org = new Organization();

		org.setId("0729566c-3012-4e46-8ba1-bbcf5afe2e6d");
		org.setCode("orgsyn20update");

		JsonResponse result = RestUtils.getInstance().doPost(url, org, JsonResponse.class);

（b）传递组织对象，根据传递参数更新组织

http://192.168.79.161:7777/wbalone/org4syn/updateByParam
method:post

{"id":"0729566c-3012-4e46-8ba1-bbcf5afe2e6d", "code":"","name":""}

（三）、删除

## 请求方式

1、url: http://192.168.79.161:7777/wbalone/organization/delete/{id}
2、method:get

## 返回参数

{
  "status" : 1,
  "msg" : "删除成功！"
}



##二、部门新增、修改和删除；

（一）新增：提供两种参数同步接口

## 请求方式

1、url: http://192.168.79.161:7777/wbalone/dept4syn/create
2、method:post

## 请求参数

{"id":"", "code":"dept18","name":"dept18","effective_date":"2017-06-22 00:00:00","principal":"jxsid002_U001","organization_id":"5af390ae-a993-41ec-9577-6f1110e0ae19","parent_id":""}

将请求的参数进行详细的说明：

| 参数  |参数类型|是否必须| 说明 |
| ---  | ----  | :---  | :--- |
| id   | String| N     | 主键 |
| code | String | Y    |编码 |
| name | String | Y    | 名称 |
|effective_date|String|Y|生效日期|
|principal|String|Y|负责人主键|
|organization_id|String|Y|所属组织主键|
|parent_id|    | N |    |

## 返回参数

{"status":1,"msg":"保存成功!","data":"22a57c89-54e3-4606-9fca-d1549222f996"}

## 调用示例

String url = "http://192.168.79.161:7777/wbalone/dept4syn/createByObj";

		Dept dept = new Dept();

		dept.setCode("dept20");
		dept.setName("dept20");
		dept.setEffective_date(new Date());
		dept.setPrincipal("jxsid002_U001");
		dept.setOrganization_id("5af390ae-a993-41ec-9577-6f1110e0ae19");

		JsonResponse result = RestUtils.getInstance().doPost(url, dept, JsonResponse.class);

（二）修改：提供两种参数同步接口

## 请求方式

1、url:http://192.168.79.161:7777/wbalone/dept4syn/update
2、method:post 

## 请求参数

{"id":"22a57c89-54e3-4606-9fca-d1549222f996", "code":"dept188","name":"dept188","parent_id":""}

## 返回参数

{"status":1,"msg":"操作成功！"}

## 调用示例

String url = "http://192.168.79.161:7777/wbalone/dept4syn/updateByObj";

		Dept dept = new Dept();

		dept.setId("291fcd29-48a8-498c-a862-ab19ced48b48");
		dept.setCode("dept20Update");

		JsonResponse result = RestUtils.getInstance().doPost(url, dept, JsonResponse.class);

（三）、删除


## 请求方式

1、url: http://192.168.79.161:7777/wbalone/dept/delete/{id}
2、method:get

## 返回参数

{
  "status" : 1,
  "msg" : "删除成功！"
}