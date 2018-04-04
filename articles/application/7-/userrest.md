# API接口文档规范



## 接口名称


```
用户查询rest服务；新增修改删除等同步接口
```

## 接口说明

```
通过同步接口，第三方系统能够实现用户得基础数据得对接；
提供了两种方式，一、通过加签的方式进行调用；二、通过非加签的方式进行调用；
提供了另外两种方式：一、通过sdk方式调用；二、通过非sdk方式的调用即rest的api
```

## 一、rest非加签调用方式

非加签调用方式,需要设置用户认证信息，请求的cookie中需要包含上下文信息

```

http://ip:port/wbalone/userRest/pagingList
```

## 请求方式


```
1.GET /wbalone/userRest/pagingList：用户分页查询
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| pageNum | int | N | 当前页 |
| pageSize | int | N | 每页记录数 |
| tenantId | String | N | 租户id |
| sortColumn | String | N | 排序字段，值为auto按照loginName排序，其他值为字段 |
| {search_field:value} | String | N | 查询条件,查询条件需要以search_开头 |

## 返回参数


```
{
	status: 1,
	msg: "操作成功!",
	data: {
		content: [
			{
				id: "9c9d0d62aabc40838f021ce7699fe6cd",
				loginName: "1112",
				name: "12",
				password: "ID:7ce769f579cfdd345d22e01b73fc6061625a36a7",
				salt: "bcc50aaaabb49427",
				roles: "user",
				states: "0",
				registerDate: 1501231968000,
				tenantId: "pk_jxs_tenant_0001",
				avator: "./images/pic.jpg",
				type: null,
				phone: "13567548372",
				email: "32564678989@qq.com",
				img: null,
				islock: "false",
				remark: null,
				createDate: 1501231968000,
				createPerson: "test4",
				modifyDate: 1501298268000,
				modifyPerson: "test4",
				organizationId: null,
				organizationName: null,
				dr: 4,
				ts: null,
				plainPassword: null,
				typeId: null
			}
		],
		last: false,
		totalElements: 97,
		totalPages: 49,
		lastPage: false,
		firstPage: true,
		sort: [
			{
				direction: "ASC",
				property: "loginName",
				ignoreCase: false,
				nullHandling: "NATIVE",
				ascending: true
			}
		],
		numberOfElements: 2,
		first: true,
		size: 2,
		number: 0
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 分页查询结果|
| 其他 | 分页信息|

## 调用示例
依赖workbench-model、eiap-plus-common
```
JsonResponse result = RestUtils.getInstance().doGet(url, null, JsonResponse.class);
```

## 请求方式


```
2.GET /wbalone/userRest/getById：根据用户id获取用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |
| userId | String | Y | 用户id |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {
		id: "9c9d0d62aabc40838f021ce7699fe6cd",
		loginName: "1112",
		name: "12",
		phone: "13567548372",
		email: "32564678989@qq.com"
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
依赖workbench-model、eiap-plus-common
```
JsonResponse result = RestUtils.getInstance().doGet(url, null, JsonResponse.class);
```

## 请求方式


```
3.GET /wbalone/userRest/getByCode：根据用户编码获取用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |
| userCode | String | Y | 用户的loginName |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {
		id: "9c9d0d62aabc40838f021ce7699fe6cd",
		loginName: "1112",
		name: "12",
		password: "ID:7ce769f579cfdd345d22e01b73fc6061625a36a7",
		salt: "bcc50aaaabb49427",
		roles: "user",
		states: "0",
		registerDate: 1501231968000,
		tenantId: "pk_jxs_tenant_0001",
		avator: "./images/pic.jpg",
		type: null,
		phone: "13567548372",
		email: "32564678989@qq.com",
		img: null,
		islock: "false",
		remark: null,
		createDate: 1501231968000,
		createPerson: "test4",
		modifyDate: 1501298268000,
		modifyPerson: "test4",
		organizationId: null,
		organizationName: null,
		dr: 4,
		ts: null,
		plainPassword: null,
		typeId: null
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
依赖workbench-model、eiap-plus-common
```
JsonResponse result = RestUtils.getInstance().doGet(url, null, JsonResponse.class);
```

## 请求方式


```
4.POST /wbalone/userRest/getByIds：根据用户ids批量获取用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |
| userIds | String | Y | 用户的id |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {[
			id: "9c9d0d62aabc40838f021ce7699fe6cd",
			loginName: "1112",
			name: "12",
			phone: "13567548372",
			email: "32564678989@qq.com"
		]
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
依赖workbench-model、eiap-plus-common
```
JsonResponse result = RestUtils.getInstance().doPost(url, null, JsonResponse.class);
```

## 请求方式


```
5.GET /wbalone/userRest/getAll：获取所有用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {
		[
			id: "9c9d0d62aabc40838f021ce7699fe6cd",
			loginName: "1112",
			name: "12",
			password: "ID:7ce769f579cfdd345d22e01b73fc6061625a36a7",
			salt: "bcc50aaaabb49427",
			roles: "user",
			states: "0",
			registerDate: 1501231968000,
			tenantId: "pk_jxs_tenant_0001",
			avator: "./images/pic.jpg",
			type: null,
			phone: "13567548372",
			email: "32564678989@qq.com",
			img: null,
			islock: "false",
			remark: null,
			createDate: 1501231968000,
			createPerson: "test4",
			modifyDate: 1501298268000,
			modifyPerson: "test4",
			organizationId: null,
			organizationName: null,
			dr: 4,
			ts: null,
			plainPassword: null,
			typeId: null
		]
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
依赖workbench-model、eiap-plus-common
```
JsonResponse result = RestUtils.getInstance().doGet(url, null, JsonResponse.class);
```

## 请求方式


```
6.GET /wbalone/userRest/getByPhone：通过用户手机号获取用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |
| phone | String | Y | 用户手机号 |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {
		id: "9c9d0d62aabc40838f021ce7699fe6cd",
		loginName: "1112",
		name: "12",
		password: "ID:7ce769f579cfdd345d22e01b73fc6061625a36a7",
		salt: "bcc50aaaabb49427",
		roles: "user",
		states: "0",
		registerDate: 1501231968000,
		tenantId: "pk_jxs_tenant_0001",
		avator: "./images/pic.jpg",
		type: null,
		phone: "13567548372",
		email: "32564678989@qq.com",
		img: null,
		islock: "false",
		remark: null,
		createDate: 1501231968000,
		createPerson: "test4",
		modifyDate: 1501298268000,
		modifyPerson: "test4",
		organizationId: null,
		organizationName: null,
		dr: 4,
		ts: null,
		plainPassword: null,
		typeId: null
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
依赖workbench-model、eiap-plus-common
```
JsonResponse result = RestUtils.getInstance().doGet(url, null, JsonResponse.class);
```


## 二、rest加签调用方式

加签调用方式,不需要身份信息，需要加签文件解决认证【方法的参数返回值及请求方式与上述非加签的方式一致，需要注意的是url中userRest更改为userRestWithSign】

```

http://ip:port/wbalone/userRestWithSign/pagingList
```

## 请求方式


```
1.GET /wbalone/userRestWithSign/pagingList：用户分页查询
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| pageNum | int | N | 当前页 |
| pageSize | int | N | 每页记录数 |
| tenantId | String | N | 租户id |
| sortColumn | String | N | 排序字段，值为auto按照loginName排序，其他值为字段 |
| {search_field:value} | String | N | 查询条件,查询条件需要以search_开头 |

## 返回参数


```
{
	status: 1,
	msg: "操作成功!",
	data: {
		content: [
			{
				id: "9c9d0d62aabc40838f021ce7699fe6cd",
				loginName: "1112",
				name: "12",
				password: "ID:7ce769f579cfdd345d22e01b73fc6061625a36a7",
				salt: "bcc50aaaabb49427",
				roles: "user",
				states: "0",
				registerDate: 1501231968000,
				tenantId: "pk_jxs_tenant_0001",
				avator: "./images/pic.jpg",
				type: null,
				phone: "13567548372",
				email: "32564678989@qq.com",
				img: null,
				islock: "false",
				remark: null,
				createDate: 1501231968000,
				createPerson: "test4",
				modifyDate: 1501298268000,
				modifyPerson: "test4",
				organizationId: null,
				organizationName: null,
				dr: 4,
				ts: null,
				plainPassword: null,
				typeId: null
			}
		],
		last: false,
		totalElements: 97,
		totalPages: 49,
		lastPage: false,
		firstPage: true,
		sort: [
			{
				direction: "ASC",
				property: "loginName",
				ignoreCase: false,
				nullHandling: "NATIVE",
				ascending: true
			}
		],
		numberOfElements: 2,
		first: true,
		size: 2,
		number: 0
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 分页查询结果|
| 其他 | 分页信息|

## 调用示例
依赖workbench-model、eiap-plus-common
```
JsonResponse result = RestUtils.getInstance().doGet(url, null, JsonResponse.class);
```

## 请求方式


```
2.GET /wbalone/userRestWithSign/getById：根据用户id获取用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |
| userId | String | Y | 用户id |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {
		id: "9c9d0d62aabc40838f021ce7699fe6cd",
		loginName: "1112",
		name: "12",
		phone: "13567548372",
		email: "32564678989@qq.com"
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
依赖workbench-model、eiap-plus-common
```
JsonResponse result = RestUtils.getInstance().doGet(url, null, JsonResponse.class);
```

## 请求方式


```
3.GET /wbalone/userRestWithSign/getByCode：根据用户编码获取用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |
| userCode | String | Y | 用户的loginName |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {
		id: "9c9d0d62aabc40838f021ce7699fe6cd",
		loginName: "1112",
		name: "12",
		password: "ID:7ce769f579cfdd345d22e01b73fc6061625a36a7",
		salt: "bcc50aaaabb49427",
		roles: "user",
		states: "0",
		registerDate: 1501231968000,
		tenantId: "pk_jxs_tenant_0001",
		avator: "./images/pic.jpg",
		type: null,
		phone: "13567548372",
		email: "32564678989@qq.com",
		img: null,
		islock: "false",
		remark: null,
		createDate: 1501231968000,
		createPerson: "test4",
		modifyDate: 1501298268000,
		modifyPerson: "test4",
		organizationId: null,
		organizationName: null,
		dr: 4,
		ts: null,
		plainPassword: null,
		typeId: null
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
依赖workbench-model、eiap-plus-common
```
JsonResponse result = RestUtils.getInstance().doGet(url, null, JsonResponse.class);
```

## 请求方式


```
4.POST /wbalone/userRestWithSign/getByIds：根据用户ids批量获取用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |
| userIds | String | Y | 用户的id |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {[
			id: "9c9d0d62aabc40838f021ce7699fe6cd",
			loginName: "1112",
			name: "12",
			phone: "13567548372",
			email: "32564678989@qq.com"
		]
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
依赖workbench-model、eiap-plus-common
```
JsonResponse result = RestUtils.getInstance().doPost(url, null, JsonResponse.class);
```

## 请求方式


```
5.GET /wbalone/userRestWithSign/getAll：获取所有用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {
		[
			id: "9c9d0d62aabc40838f021ce7699fe6cd",
			loginName: "1112",
			name: "12",
			password: "ID:7ce769f579cfdd345d22e01b73fc6061625a36a7",
			salt: "bcc50aaaabb49427",
			roles: "user",
			states: "0",
			registerDate: 1501231968000,
			tenantId: "pk_jxs_tenant_0001",
			avator: "./images/pic.jpg",
			type: null,
			phone: "13567548372",
			email: "32564678989@qq.com",
			img: null,
			islock: "false",
			remark: null,
			createDate: 1501231968000,
			createPerson: "test4",
			modifyDate: 1501298268000,
			modifyPerson: "test4",
			organizationId: null,
			organizationName: null,
			dr: 4,
			ts: null,
			plainPassword: null,
			typeId: null
		]
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
依赖workbench-model、eiap-plus-common
```
JsonResponse result = RestUtils.getInstance().doGet(url, null, JsonResponse.class);
```


## 三、sdk非加签调用方式

非加签调用方式,需要设置用户认证信息，请求的cookie中需要包含上下文信息

```
(1)pom中需要依赖workbench-sdk的jar包
(2)application.properties中需要配置：
    workbench.base.url

```

## 请求方式


```
1.com.yonyou.uap.wb.sdk.UserRest#pagingList：用户分页查询
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| pageNum | int | N | 当前页 |
| pageSize | int | N | 每页记录数 |
| tenantId | String | N | 租户id |
| sortColumn | String | N | 排序字段，值为auto按照loginName排序，其他值为字段 |
| {search_field:value} | String | N | 查询条件,查询条件需要以search_开头 |

## 返回参数


```
{
	status: 1,
	msg: "操作成功!",
	data: {
		content: [
			{
				id: "9c9d0d62aabc40838f021ce7699fe6cd",
				loginName: "1112",
				name: "12",
				password: "ID:7ce769f579cfdd345d22e01b73fc6061625a36a7",
				salt: "bcc50aaaabb49427",
				roles: "user",
				states: "0",
				registerDate: 1501231968000,
				tenantId: "pk_jxs_tenant_0001",
				avator: "./images/pic.jpg",
				type: null,
				phone: "13567548372",
				email: "32564678989@qq.com",
				img: null,
				islock: "false",
				remark: null,
				createDate: 1501231968000,
				createPerson: "test4",
				modifyDate: 1501298268000,
				modifyPerson: "test4",
				organizationId: null,
				organizationName: null,
				dr: 4,
				ts: null,
				plainPassword: null,
				typeId: null
			}
		],
		last: false,
		totalElements: 97,
		totalPages: 49,
		lastPage: false,
		firstPage: true,
		sort: [
			{
				direction: "ASC",
				property: "loginName",
				ignoreCase: false,
				nullHandling: "NATIVE",
				ascending: true
			}
		],
		numberOfElements: 2,
		first: true,
		size: 2,
		number: 0
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 分页查询结果|
| 其他 | 分页信息|

## 调用示例
依赖workbench-sdk
```
Map<String, String> queryParams = new HashMap<String,String>();
queryParams.put("tenantId","tenant");
queryParams.put("pageSize","1");
queryParams.put("pageNum","20");
JsonResponse result = com.yonyou.uap.wb.sdk.UserRest.pagingList(queryParams);
```

## 请求方式


```
2.com.yonyou.uap.wb.sdk.UserRest#getById：根据用户id获取用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |
| userId | String | Y | 用户id |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {
		id: "9c9d0d62aabc40838f021ce7699fe6cd",
		loginName: "1112",
		name: "12",
		phone: "13567548372",
		email: "32564678989@qq.com"
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
```
Map<String, String> queryParams = new HashMap<String,String>();
queryParams.put("tenantId","tenant");
queryParams.put("userId","pk");
JsonResponse result = com.yonyou.uap.wb.sdk.UserRest.getById(queryParams);
```

## 请求方式


```
3.com.yonyou.uap.wb.sdk.UserRest#getByCode：根据用户编码获取用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |
| userCode | String | Y | 用户的loginName |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {
		id: "9c9d0d62aabc40838f021ce7699fe6cd",
		loginName: "1112",
		name: "12",
		password: "ID:7ce769f579cfdd345d22e01b73fc6061625a36a7",
		salt: "bcc50aaaabb49427",
		roles: "user",
		states: "0",
		registerDate: 1501231968000,
		tenantId: "pk_jxs_tenant_0001",
		avator: "./images/pic.jpg",
		type: null,
		phone: "13567548372",
		email: "32564678989@qq.com",
		img: null,
		islock: "false",
		remark: null,
		createDate: 1501231968000,
		createPerson: "test4",
		modifyDate: 1501298268000,
		modifyPerson: "test4",
		organizationId: null,
		organizationName: null,
		dr: 4,
		ts: null,
		plainPassword: null,
		typeId: null
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
```
Map<String, String> queryParams = new HashMap<String,String>();
queryParams.put("tenantId","tenant");
queryParams.put("userCode","pk");
JsonResponse result = com.yonyou.uap.wb.sdk.UserRest.getByCode(queryParams);
```

## 请求方式


```
4.com.yonyou.uap.wb.sdk.UserRest#getByIds：根据用户ids批量获取用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |
| userIds | String | Y | 用户的id |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {[
			id: "9c9d0d62aabc40838f021ce7699fe6cd",
			loginName: "1112",
			name: "12",
			phone: "13567548372",
			email: "32564678989@qq.com"
		]
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
```
Map<String, String> queryParams = new HashMap<String,String>();
queryParams.put("tenantId","tenant");
Map<String, String> postParams = new HashMap<String,String>();
queryParams.put("userIds","[xxxx01, xxxx02, xxxx03, xxxx04...]");
JsonResponse result = com.yonyou.uap.wb.sdk.UserRest.getByIds(queryParams,postParams);
```

## 请求方式


```
5.com.yonyou.uap.wb.sdk.UserRest#getByPhone：获取所有用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |
| phone | String | Y | 用户手机号 |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {
		id: "9c9d0d62aabc40838f021ce7699fe6cd",
		loginName: "1112",
		name: "12",
		password: "ID:7ce769f579cfdd345d22e01b73fc6061625a36a7",
		salt: "bcc50aaaabb49427",
		roles: "user",
		states: "0",
		registerDate: 1501231968000,
		tenantId: "pk_jxs_tenant_0001",
		avator: "./images/pic.jpg",
		type: null,
		phone: "13567548372",
		email: "32564678989@qq.com",
		img: null,
		islock: "false",
		remark: null,
		createDate: 1501231968000,
		createPerson: "test4",
		modifyDate: 1501298268000,
		modifyPerson: "test4",
		organizationId: null,
		organizationName: null,
		dr: 4,
		ts: null,
		plainPassword: null,
		typeId: null
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
```
Map<String, String> queryParams = new HashMap<String,String>();
queryParams.put("tenantId","tenant");
queryParams.put("phone","151....");
JsonResponse result = com.yonyou.uap.wb.sdk.UserRest.getByPhone(queryParams);
```

## 四、sdk加签调用方式

加签调用方式,不需要身份信息，需要加签文件解决认证【方法的参数返回值及请求方式与上述非加签的方式一致，需要注意的是url中userRest更改为userRestWithSign】

```
(1)pom中需要依赖workbench-sdk的jar包
(2)application.properties中需要配置：
    workbench.base.url

```

## 请求方式


```
1.com.yonyou.uap.wb.sdk.UserRestWithSign#pagingList：用户分页查询
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
|params ||||
| pageNum | int | N | 当前页 |
| pageSize | int | N | 每页记录数 |
| tenantId | String | N | 租户id |
| sortColumn | String | N | 排序字段，值为auto按照loginName排序，其他值为字段 |
| {search_field:value} | String | N | 查询条件,查询条件需要以search_开头 |
|prefix||||
## 返回参数


```
{
	status: 1,
	msg: "操作成功!",
	data: {
		content: [
			{
				id: "9c9d0d62aabc40838f021ce7699fe6cd",
				loginName: "1112",
				name: "12",
				password: "ID:7ce769f579cfdd345d22e01b73fc6061625a36a7",
				salt: "bcc50aaaabb49427",
				roles: "user",
				states: "0",
				registerDate: 1501231968000,
				tenantId: "pk_jxs_tenant_0001",
				avator: "./images/pic.jpg",
				type: null,
				phone: "13567548372",
				email: "32564678989@qq.com",
				img: null,
				islock: "false",
				remark: null,
				createDate: 1501231968000,
				createPerson: "test4",
				modifyDate: 1501298268000,
				modifyPerson: "test4",
				organizationId: null,
				organizationName: null,
				dr: 4,
				ts: null,
				plainPassword: null,
				typeId: null
			}
		],
		last: false,
		totalElements: 97,
		totalPages: 49,
		lastPage: false,
		firstPage: true,
		sort: [
			{
				direction: "ASC",
				property: "loginName",
				ignoreCase: false,
				nullHandling: "NATIVE",
				ascending: true
			}
		],
		numberOfElements: 2,
		first: true,
		size: 2,
		number: 0
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 分页查询结果|
| 其他 | 分页信息|

## 调用示例
依赖workbench-sdk
```
Map<String, String> queryParams = new HashMap<String,String>();
queryParams.put("tenantId","tenant");
queryParams.put("pageSize","1");
queryParams.put("pageNum","20");
JsonResponse result = com.yonyou.uap.wb.sdk.UserRestWithSign.pagingList(queryParams,"prefix");
```

## 请求方式


```
2.com.yonyou.uap.wb.sdk.UserRestWithSign#getById：根据用户id获取用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |
| userId | String | Y | 用户id |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {
		id: "9c9d0d62aabc40838f021ce7699fe6cd",
		loginName: "1112",
		name: "12",
		phone: "13567548372",
		email: "32564678989@qq.com"
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
```
Map<String, String> queryParams = new HashMap<String,String>();
queryParams.put("tenantId","tenant");
queryParams.put("userId","pk");
JsonResponse result = com.yonyou.uap.wb.sdk.UserRestWithSign.getById(queryParams,"prefix");
```

## 请求方式


```
3.com.yonyou.uap.wb.sdk.UserRestWithSign#getByCode：根据用户编码获取用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |
| userCode | String | Y | 用户的loginName |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {
		id: "9c9d0d62aabc40838f021ce7699fe6cd",
		loginName: "1112",
		name: "12",
		password: "ID:7ce769f579cfdd345d22e01b73fc6061625a36a7",
		salt: "bcc50aaaabb49427",
		roles: "user",
		states: "0",
		registerDate: 1501231968000,
		tenantId: "pk_jxs_tenant_0001",
		avator: "./images/pic.jpg",
		type: null,
		phone: "13567548372",
		email: "32564678989@qq.com",
		img: null,
		islock: "false",
		remark: null,
		createDate: 1501231968000,
		createPerson: "test4",
		modifyDate: 1501298268000,
		modifyPerson: "test4",
		organizationId: null,
		organizationName: null,
		dr: 4,
		ts: null,
		plainPassword: null,
		typeId: null
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
```
Map<String, String> queryParams = new HashMap<String,String>();
queryParams.put("tenantId","tenant");
queryParams.put("userCode","pk");
JsonResponse result = com.yonyou.uap.wb.sdk.UserRestWithSign.getByCode(queryParams,"prefix");
```

## 请求方式


```
4.com.yonyou.uap.wb.sdk.UserRestWithSign#getByIds：根据用户ids批量获取用户信息
```

## 请求参数

| 参数 | 参数类型 |是否必须| 说明 |
| --- | --- | :--- | :--- |
| tenantId | String | Y | 租户id |
| userIds | String | Y | 用户的id |

## 返回参数


```
{
	status: 1,
	msg: "操作成功！",
	data: {[
			id: "9c9d0d62aabc40838f021ce7699fe6cd",
			loginName: "1112",
			name: "12",
			phone: "13567548372",
			email: "32564678989@qq.com"
		]
	}
}
```

将返回的参数进行详细的说明：

| 参数 | 参数说明 |
| --- | :--- |
| status | 返回状态1表示success成功，0表示failed失败|
| msg | 操作提示信息| 
| data | 查询结果|

## 调用示例
```
Map<String, String> queryParams = new HashMap<String,String>();
queryParams.put("tenantId","tenant");
Map<String, String> postParams = new HashMap<String,String>();
queryParams.put("userIds","[xxxx01, xxxx02, xxxx03, xxxx04...]");
JsonResponse result = com.yonyou.uap.wb.sdk.UserRestWithSign.getByIds(queryParams,postParams,"prefix");
```
