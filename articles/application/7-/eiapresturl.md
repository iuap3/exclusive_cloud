
### 1.1审批面板状态接口

1

#### 1. 功能说明

获取任务按钮列表数据

#### 2. 请求体说明

| url | /eiap-plus/process/getbillbpm |
| --- | --- |
| method | POST |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3. 请求参数说明

参数以JSON格式提交，如下：

``` json
{  "billId": "8710842d-9a67-4d5a-b136-38c2f7969e17"}
```

#### 4. 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| billId | String | 单据id |

#### 5. 返回值

成功:返回用户任务信息。任务信息有三种情况未启用流程，无待办任务，有待办任务。

status code： 200

message：NoBpm：未启用BPM， NoTask:无待办。

responseBody ：

``` Json
{
  "processInstanceId": "f971f358-6acb-11e8-84a5-0686c4000fcf",
  "processDefinitionId": "eiap906303:33:bd5e5b68-695e-11e8-a704-0686c4000fcf",
  "message": "NoTask|NoBpm"，
  "taskId": "待办任务ID"
}

```



失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

### 1.2查询办结任务列表

#### 1. 功能说明

查询办结任务列表

#### 2. 请求体说明

| url | /eiap-plus/process/finishTasklist |
| --- | --- |
| method | POST |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3. 请求参数说明

参数以JSON格式提交，如下：



``` json
{
  "draw": 1,
  "length": 10,
  "order": {
    
  },
  "search": {
    "processDefinitionName": "",
    "billno": ""
  },
  "searchconfirm": {
    
  }
}
```

#### 4. 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| draw | int | 页码 |
| length | int | 页大小 |
| search | JSON格式 | 流程定义名称，单据号 |

#### 5. 返回值

成功:会返回用户任务列表信息。上述示例中的任务列表有1条数据，结果只会显示一条记录。

status code： 200

responseBody ：

``` json

{
  "recordsTotal": 1,
  "recordsFilered": 1,
  "draw": 0,
  "data": [
    {
      "id": "db4d063d-679b-11e8-b821-069f62000162",
      "processDefinitionId": "eiap341786:3:29159eda-6611-11e8-95f4-069f62000162",
      "processInstanceId": "db21141b-679b-11e8-b821-069f62000162",
      "executionId": "db4a4719-679b-11e8-b821-069f62000162",
      "name": "系统管理员提交的【督办】,单号 是QBM201820, 请审批",
      "deleteReason": "deleted",
      "assignee": "8d8866ce86624ef99fd1185b5a1e1af5",
      "startTime": 1528077973000,
      "endTime": 1528101771000,
      "durationInMillis": 23798450,
      "taskDefinitionKey": "approveUserTask4818",
      "priority": 50,
      "dueDate": 33053450773000,
      "parentTaskId": null,
      "url": "db4d063d-679b-11e8-b821-069f62000162",
      "variables": [
      ],
      "tenantId": "default_tenant_id",
      "category": "default_tenant_id",
      "processFinished": false,
      "finished": false,
      "state": "delete_processUnFinished",
      "processDefinitionName": ""
    }
  ],
  "statusCode": "200",
  "message": "操作成功"
}
```



### 1.3查询已办任务列表

1 

#### 1. 功能说明

查询已办任务列表

#### 2. 请求体说明

| url | /eiap-plus/process/doneTasklist |
| --- | --- |
| method | POST |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3. 请求参数说明

参数以JSON格式提交，如下：

``` json

{
  "draw": 1,
  "length": 10,
  "search": {
    "processDefinitionName": "流程定义名称",
    "billno": "单据号"
  }
}

```

#### 4. 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| draw | int | 页码 |
| length | int | 页大小 |
| search | JSON格式 | 流程定义名称，单据号 |

#### 5. 返回值

成功:返回已办任务列表信息。

status code： 200

responseBody ：

``` json

{
  "recordsTotal": 1,
  "recordsFilered": 1,
  "draw": 0,
  "data": [
    {
      "id": "db4d063d-679b-11e8-b821-069f62000162",
      "processDefinitionId": "eiap341786:3:29159eda-6611-11e8-95f4-069f62000162",
      "processInstanceId": "db21141b-679b-11e8-b821-069f62000162",
      "executionId": "db4a4719-679b-11e8-b821-069f62000162",
      "name": "系统管理员提交的【督办】,单号 是QBM201820, 请审批",
      "deleteReason": "deleted",
      "assignee": "8d8866ce86624ef99fd1185b5a1e1af5",
      "startTime": 1528077973000,
      "endTime": 1528101771000,
      "durationInMillis": 23798450,
      "taskDefinitionKey": "approveUserTask4818",
      "priority": 50,
      "dueDate": 33053450773000,
      "parentTaskId": null,
      "url": "db4d063d-679b-11e8-b821-069f62000162",
      "variables": [
      ],
      "tenantId": "default_tenant_id",
      "category": "default_tenant_id",
      "processFinished": false,
      "finished": false,
      "state": "delete_processUnFinished",
      "processDefinitionName": ""
    }
  ],
  "statusCode": "200",
  "message": "操作成功"
}

```



失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

### 1.4查询待办任务列表

2 

#### 1. 功能说明

查询待办任务列表

#### 2. 请求体说明

| url | /eiap-plus/process/undoTasklist |
| --- | --- |
| method | POST |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3. 请求参数说明

参数以JSON格式提交，如下：

``` json
{
  "draw": 1,
  "length": 10,
  "search": {
    "processDefinitionName": "流程定义名称",
    "billno": "单据号"
  }
}
```

#### 4. 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| serviceid | String | 服务id |
| token | String | token |
| usercode | String | 用户编码 |
| getTaskBillContent | JSON格式 | 方法名 |



#### 5. 返回值

成功:返回待办任务信息。

status code： 200

responseBody ：

``` json
{
  "recordsTotal": 1,
  "recordsFilered": 1,
  "draw": 0,
  "data": [
    {
      "id": "db4d063d-679b-11e8-b821-069f62000162",
      "processDefinitionId": "eiap341786:3:29159eda-6611-11e8-95f4-069f62000162",
      "processInstanceId": "db21141b-679b-11e8-b821-069f62000162",
      "executionId": "db4a4719-679b-11e8-b821-069f62000162",
      "name": "系统管理员提交的【督办】,单号 是QBM201820, 请审批",
      "deleteReason": "deleted",
      "assignee": "8d8866ce86624ef99fd1185b5a1e1af5",
      "startTime": 1528077973000,
      "endTime": 1528101771000,
      "durationInMillis": 23798450,
      "taskDefinitionKey": "approveUserTask4818",
      "priority": 50,
      "dueDate": 33053450773000,
      "parentTaskId": null,
      "url": "db4d063d-679b-11e8-b821-069f62000162",
      "variables": [
      ],
      "tenantId": "default_tenant_id",
      "category": "default_tenant_id",
      "processFinished": false,
      "finished": false,
      "state": "delete_processUnFinished",
      "processDefinitionName": ""
    }
  ],
  "statusCode": "200",
  "message": "操作成功"
}
```



失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

### 1.5查询流程历史信息

3 

#### 1. 功能说明

查询流程历史信息

#### 2. 请求体说明

| url | /eiap-plus/process/hisTasklist |
| --- | --- |
| method | POST |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3. 请求参数说明

参数以JSON格式提交，如下：

``` json
{
  "processDefinitionId": "eiap764916:16:310e7520-6304-11e8-8d04-0686c4000fcf",
  "processInstanceId": "509533c5-630d-11e8-8d04-0686c4000fcf"
}
```

#### 4. 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| processDefinitionId | String | 流程定义id |
| processInstanceId | String | 流程实例id |

#### 5. 返回值

成功:返回流程历史信息。

status code： 200

responseBody ：

``` json
{
  "recordsTotal": 2,
  "recordsFilered": 2,
  "draw": 1,
  "data": [
    {
      "id": "50a70e36-630d-11e8-8d04-0686c4000fcf",
      "processDefinitionId": null,
      "processDefinitionUrl": null,
      "processInstanceId": null,
      "processInstanceUrl": null,
      "executionId": "系统管理员",
      "name": "提交人",
      "keyFeature": null,
      "description": "submit",
      "deleteReason": null,
      "owner": null,
      "assignee": null,
      "startTime": 1527576947367,
      "endTime": 1527576947367,
      "durationInMillis": null,
      "workTimeInMillis": null,
      "claimTime": null,
      "taskDefinitionKey": null,
      "formKey": null,
      "priority": null,
      "dueDate": null,
      "parentTaskId": null,
      "url": null,
      "variables": [
        
      ],
      "tenantId": null,
      "category": null,
      "ownerParticipant": null,
      "processFinished": false,
      "finished": false,
      "state": null,
      "processDefinitionName": null,
      "categoryResponse": null,
      "assigneeParticipant": null,
      "candidateParticipants": null,
      "historicProcessInstance": null,
      "taskComments": null,
      "activity": null
    },
    {
      "id": "50a70e36-630d-11e8-8d04-0686c4000fcf",
      "processDefinitionId": "eiap764916:16:310e7520-6304-11e8-8d04-0686c4000fcf",
      "processDefinitionUrl": "http://10.10.24.84:8080/ubpm-web-rest/service/repository/process-definitions/eiap764916:16:310e7520-6304-11e8-8d04-0686c4000fcf",
      "processInstanceId": "509533c5-630d-11e8-8d04-0686c4000fcf",
      "processInstanceUrl": "http://10.10.24.84:8080/ubpm-web-rest/service/history/historic-process-instances/509533c5-630d-11e8-8d04-0686c4000fcf",
      "executionId": "user3",
      "name": "1",
      "keyFeature": null,
      "description": "agree",
      "deleteReason": "",
      "owner": null,
      "assignee": "dfa4c7b841c247b38cf8e04074f6e330",
      "startTime": 1527576947367,
      "endTime": null,
      "durationInMillis": null,
      "workTimeInMillis": null,
      "claimTime": null,
      "taskDefinitionKey": "ApproveUserTask2",
      "formKey": null,
      "priority": 50,
      "dueDate": 33052949747371,
      "parentTaskId": null,
      "url": "50a70e36-630d-11e8-8d04-0686c4000fcf",
      "variables": [
        
      ],
      "tenantId": "default_tenant_id",
      "category": "default_tenant_id",
      "ownerParticipant": null,
      "processFinished": false,
      "finished": false,
      "state": null,
      "processDefinitionName": null,
      "categoryResponse": null,
      "assigneeParticipant": null,
      "candidateParticipants": null,
      "historicProcessInstance": null,
      "taskComments": null,
      "activity": null
    }
  ],
  "statusCode": "200",
  "message": "操作成功"
}
```



失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

### 1.6查询流程图信息

4 

#### 1. 功能说明

查询流程图信息

#### 2. 请求体说明

| url | /eiap-plus/process/processInstancediagram |
| --- | --- |
| method | GET |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3. 请求参数说明

参数以JSON格式提交，如下：

```
processDefinitionId=eiap341786%3A3%3A29159eda-6611-11e8-95f4-069f62000162&processInstanceId=c046cadb-6ae5-11e8-a55a-06e0de000f8c&_=1528445299026
```

#### 4. 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| processDefinitionId | String | 流程定义id |
| processInstanceId | String | 流程实例id |

#### 5. 返回值

成功:返回流程图信息。

status code： 200

responseBody ：

``` json
{
  "processDefinition": {
    "id": "eiap341786:3:29159eda-6611-11e8-95f4-069f62000162",
    "name": "01",
    "key": "eiap341786",
    "version": 3,
    "deploymentId": "2902da27-6611-11e8-95f4-069f62000162",
    "isGraphicNotationDefined": true
  },
  "highLightedActivities": [
    "approveUserTask4818",
    "approveUserTask4818"
  ],
  "highLightedFlows": [
    "SequenceFlow7215"
  ],
  "activities": [
    {
      "activityId": "approveUserTask4818",
      "properties": {
        "createTime0": "2018-06-08 14:32:48",
        "createTime1": "2018-06-08 14:32:49",
        "state": "run",
        "taskNum": 2,
        "extendAttributes": "[]",
        "addsignAble": "true",
        "multiInstance": "parallel",
        "canBeRejected": "true",
        "addsignbehindAble": "true",
        "name": "A",
        "type": "approveUserTask",
        "multipleTypeoperations": "null",
        "rejectAble": "true"
      },
      "multiInstance": "parallel",
      "x": 284,
      "y": 0.0,
      "width": 90,
      "height": 60
    },
    {
      "activityId": "ApproveUserTask2",
      "properties": {
        "taskNum": 0,
        "extendAttributes": "[]",
        "addsignAble": "true",
        "multiInstance": "parallel",
        "canBeRejected": "true",
        "addsignbehindAble": "true",
        "name": "B",
        "type": "approveUserTask",
        "rejectAble": "true"
      },
      "multiInstance": "parallel",
      "x": 419,
      "y": 0.0,
      "width": 90,
      "height": 60
    },
    {
      "activityId": "startEvent6889",
      "properties": {
        "taskNum": 0,
        "type": "startEvent"
      },
      "x": 210,
      "y": 18.0,
      "width": 24,
      "height": 24
    },
    {
      "activityId": "EndNoneEvent2",
      "properties": {
        "taskNum": 0,
        "name": "ç»“æŸ",
        "type": "endEvent"
      },
      "x": 554,
      "y": 18.0,
      "width": 24,
      "height": 24
    }
  ],
  "sequenceFlows": [
    {
      "id": "sid-3E79483E-930C-47FB-B72F-2F87A58755B4",
      "name": null,
      "flow": "(approveUserTask4818)--sid-3E79483E-930C-47FB-B72F-2F87A58755B4-->(ApproveUserTask2)",
      "sourseName": "A",
      "destinationName": "B",
      "xPointArray": [
        45,
        45
      ],
      "yPointArray": [
        -716.0,
        -716.0
      ]
    },
    {
      "id": "sid-7634168B-0F09-4BDD-9D3D-7225C1B2ADCF",
      "name": null,
      "flow": "(ApproveUserTask2)--sid-7634168B-0F09-4BDD-9D3D-7225C1B2ADCF-->(EndNoneEvent2)",
      "sourseName": "B",
      "destinationName": "ç»“æŸ",
      "xPointArray": [
        45,
        12
      ],
      "yPointArray": [
        -716.0,
        -734.0
      ]
    },
    {
      "id": "SequenceFlow7215",
      "name": null,
      "flow": "(startEvent6889)--SequenceFlow7215-->(approveUserTask4818)",
      "destinationName": "A",
      "xPointArray": [
        12,
        45
      ],
      "yPointArray": [
        -734.0,
        -716.0
      ]
    }
  ]
}
```


失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

### 1.7查询流程图高亮信息

5 

#### 1. 功能说明

查询流程图高亮信息

#### 2. 请求体说明

| url | /eiap-plus/process/highlightsprocessInstance |
| --- | --- |
| method | GET |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3. 请求参数说明

参数以查询字符串格式提交，如下：

```
processInstanceId=c046cadb-6ae5-11e8-a55a-06e0de000f8c
```

#### 4. 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| processInstanceId | String | 流程实例id |

#### 5. 返回值

成功:返回流程图高亮信息。

status code： 200

responseBody ：

```json
{
  "processInstanceId": "c046cadb-6ae5-11e8-a55a-06e0de000f8c",
  "processDefinitionId":"eiap341786:3:29159eda-6611-11e8-95f4-069f62000162",
  "activities": [
    "startEvent6889",
    "approveUserTask4818",
    "approveUserTask4818"
  ],
  "currentactivities": [
    "approveUserTask4818",
    "approveUserTask4818",
    "approveUserTask4818"
  ],
  "flows": [
    "SequenceFlow7215"
  ]
}
```



失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

### 1.8查询资源分配信息

6 

#### 1. 功能说明

查询资源分配信息

#### 2. 请求体说明

| url | /eiap-plus/appResAllocate/queryBpmTemplateAllocate |
| --- | --- |
| method | GET |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3. 请求参数说明

参数以查询字符串格式提交，如下：

```
funccode=ygdemo_yw_info&nodekey=003
```

#### 4. 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| funccode | String | 功能编码（应用注册时的功能编码） |
| nodekey | String | 节点key（资源分配时的nodekey） |

#### 5. 返回值

成功:返回资源分配信息。

status code： 200

responseBody ：

``` json
{
  "success": "success",
  "detailMsg": {
    "data": {
      "metaDefinedName": "TemplateAllocate",
      "namespace": "iuap_qy", 
      "pk": "fdf11261-9544-4d5a-a103-0770fc993d07",
      "funcid": "27228e888fc34440835fec75f8fe57b7",
      "funccode": "ygdemo_yw_info",
      "restype": "bpm",
      "nodekey": "003",
      "pk_res": "eiap764916",
      "res_name": "lzy001",
      "res_code": "eiap764916",
      "tenant_id": "tenant"
    }
  }
}
```



失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

### 1.9查询流程启用状态信息



#### 1. 功能说明

查询流程启用、停用状态信息

#### 2. 请求体说明

| url | /eiap-plus/appResAllocateRelate/getEnableBpm |
| --- | --- |
| method | GET |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3. 请求参数说明

参数以查询字符串格式提交，如下：

```
funcCode=ygdemo_yw_info
```

#### 4. 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| funcCode | String | 功能编码（应用注册时的功能编码） |

#### 5. 返回值

成功:返回流程启用状态信息，Y启用，N停用。

status code： 200

responseBody ：

``` json
{
  "success": "success",
  "detailMsg": {
    "data": "Y"
  }
}
```



失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

### 1.10查询审批人信息

8 

#### 1. 功能说明

查询审批人信息

#### 2. 请求体说明

| url | /eiap-plus/task/assignee/getlist |
| --- | --- |
| method | POST |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3. 请求参数说明

参数以JSON格式提交，如下：

``` json
{
  "taskId": "8b2323b5-6961-11e8-9b4f-066af2000fa6",
  "processInstanceId": "ffba94a6-6953-11e8-9b4f-066af2000fa6",
  "processDefinitionId": "eiap115643:1:a82b918f-6939-11e8-9b4f-066af2000fa6",
  "comment": "审批意见", 
  "approvetype": "signAdd",
  "pageSize": 10,
  "pageNum": 1
}
```

#### 4. 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| taskId | String | 任务id |
| processInstanceId | String | 流程实例Id |
| processDefinitionId | String | 流程定义Id |
| comment | String | 审批意见 |
| approvetype | String | 审批类型 （delegate:改派;signAdd：加签） |
| pageSize | int | 页大小 |
| pageNum | int | 页码 |

#### 5. 返回值

成功:会返回用户列表信息。

status code： 200

responseBody ：

``` json
{
  "msg": "操作成功!",
  "data": {
    "number": 0,
    "last": true,
    "firstPage": true,
    "lastPage": true,
    "numberOfElements": 4,
    "size": 10,
    "totalPages": 1,
    "sort": [
      {
        "nullHandling": "NATIVE",
        "ignoreCase": false,
        "property": "loginName",
        "ascending": true,
        "direction": "ASC"
      }
    ],
    "content": [
      {
        "code": "11",
        "enable": true,
        "name": "11",
        "id": "645c031a64b74b04a26f40cb7ea6d6ae",
        "sysadmin": false,
        "revision": 0
      },
      {
        "code": "12",
        "enable": true,
        "name": "12",
        "id": "9943304b46df4a85a4e43c95653159d2",
        "sysadmin": false,
        "revision": 0
      }
    ],
    "first": true,
    "totalElements": 4
  },
  "status": 1
}
```



失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

### 1.11获取驳回环节列表

9 

#### 1. 功能说明

获取驳回环节列表

#### 2. 请求体说明

| url | /eiap-plus/task/rejecttask/bfreject |
| --- | --- |
| method | POST |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3. 请求参数说明

参数以JSON格式提交，如下：

``` json
{
  "taskId": "8b2323b5-6961-11e8-9b4f-066af2000fa6",
  "processInstanceId": "ffba94a6-6953-11e8-9b4f-066af2000fa6",
  "processDefinitionId": "eiap115643:1:a82b918f-6939-11e8-9b4f-066af2000fa6",
  "comment": "意见",
  "approvetype": "rejectToActivity"
}
```

#### 4. 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| taskId | String | 任务id |
| processInstanceId | String | 流程实例ID |
| processDefinitionId | String | 流程定义ID |
| comment | String | 意见 |
| approvetype | String | 审批类型（rejectToActivity：驳回到环节） |

#### 5. 返回值

成功:返回驳回环节列表信息。

status code： 200

responseBody ：

``` json
{
  "flag": "success",
  "rejectlist": [
    {
      "activityId": "markerbill",
      "activityName": "制单人",
      "condition": null,
      "participants": null
    }
  ]
}
```



失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

### 1.12审批动作执行信息

#### 1 功能说明

审批动作执行接口，审批包括同意与不同意。

#### 2 请求体说明

| url | /eiap-plus/task/completetask/approveCard |
| --- | --- |
| method | POST |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3 请求参数说明

参数以JSON格式提交，如下：

``` json
{
  "taskId": "50a70e36-630d-11e8-8d04-0686c4000fcf",
  "processInstanceId": "509533c5-630d-11e8-8d04-0686c4000fcf",
  "processDefinitionId": "eiap764916:16:310e7520-6304-11e8-8d04-0686c4000fcf",
  "comment": "同意",
  "approvetype": "agree"
}
```

#### 4 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| taskId | String | 任务id |
| processInstanceId | String | 流程实例Id |
| processDefinitionId | String | 流程定义Id |
| approvetype | String | 审批类型（agree:同意，ungree：不同意） |
| comment | String | 审批意见 |

#### 5 返回值

成功:返回审批动作执行信息。

status code： 200

responseBody ：

``` json
{
  "msg": "审批成功!",
  "flag": "success"
}
```



失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

### 1.13驳回动作执行信息

#### 1 功能说明

驳回动作执行信息

#### 2 请求体说明

| url | /eiap-plus/task/rejecttask/reject |
| --- | --- |
| method | POST |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3 请求参数说明

参数以JSON格式提交，如下：

``` json
{
  "taskId": "a5fd2397-631a-11e8-8d04-0686c4000fcf",
  "processInstanceId": "6316ad5a-631a-11e8-8d04-0686c4000fcf",
  "activityId": "ApproveUserTask2",
  "approvetype": "rejectToActivity",
  "comment": "4444444"
}
```

#### 4 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| approvetype | String | 审批类型 |
| taskId | String | 任务Id |
| processInstanceId | String | 流程实例Id |
| activityId | String | 驳回到环节的Id |
| comment | String | 审批意见 |



#### 5 返回值

成功:返回驳回动作执行信息。

status code： 200

responseBody ：

``` json
{
  "msg": "驳回成功!",
  "flag": "success"
}
```



失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

### 1.14驳回到制单人动作执行信息



#### 1 功能说明

驳回到制单人动作，流程终止。

#### 2 请求体说明

| url | /eiap-plus/task/rejecttask/reject |
| --- | --- |
| method | POST |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3 请求参数说明

参数以JSON格式提交，如下：

``` json
{
  "taskId": "a5fd2397-631a-11e8-8d04-0686c4000fcf",
  "processInstanceId": "6316ad5a-631a-11e8-8d04-0686c4000fcf",
  "activityId": "ApproveUserTask2",
  "approvetype": "rejectToBillMaker",
  "comment": "4444444"
}
```

#### 4 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| approvetype | String | 审批类型 |
| taskId | String | 任务Id |
| processInstanceId | String | 流程实例Id |
| comment | String | 审批意见 |

#### 5 返回值

成功:返回驳回到制单人动作信息。

status code： 200

responseBody ：

``` json
{
  "msg": "驳回成功!",
  "flag": "success"
}
```



失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

### 1.15加签动作执行信息



#### 1 功能说明

加签动作执行信息

#### 2 请求体说明

| url | /eiap-plus/task/signaddtask/signadd |
| --- | --- |
| method | POST |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3 请求参数说明

参数以JSON格式提交，如下：

``` json
{
  "approvetype": "signAdd",
  "taskId": "70b8432d-631f-11e8-9137-0686c4000fcf",
  "processInstanceId": "6316ad5a-631a-11e8-8d04-0686c4000fcf",
  "userIds": [
    "0ee562466635490c940dd5a5738f7812"
  ],
  "comment": "用友"
}
```

#### 4 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| approvetype | String | 审批类型 |
| taskId | String | 任务ID |
| processInstanceId | String | 流程实例ID |
| userIds | JSON格式 | 加签人员列表 |
| comment | String | 审批意见 |

#### 5 返回值

成功:返回加签动作执行信息。

status code： 200

responseBody ：

``` json
{
  "msg": "加签成功!",
  "flag": "success"
}
```



失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

### 1.16改派动作执行信息


#### 1. 功能说明

改派动作执行信息

#### 2. 请求体说明

| url | /eiap-plus/task/delegatetask/delegate |
| --- | --- |
| method | POST |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3. 请求参数说明

参数以JSON格式提交，如下：

``` json
{
  "approvetype": "delegate",
  "taskId": "70b8432d-631f-11e8-9137-0686c4000fcf",
  "processInstanceId": "6316ad5a-631a-11e8-8d04-0686c4000fcf",
  "userId": "0ee562466635490c940dd5a5738f7812",
  "comment": "888"
}
```

#### 4. 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| approvetype | String | 审批类型 |
| taskId | String | 任务Id |
| processInstanceId | String | 流程实例Id |
| userId | String | 改派到用户的Id |
| comment | String | 审批意见 |

#### 5. 返回值

成功:返回改派动作信息。

status code： 200

responseBody ：

``` json
{
  "flag": "success",
  "msg": "改派成功!"
}
```



失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

### 1.17弃审动作执行信息


#### 1 功能说明

弃审动作执行信息

#### 2 请求体说明

| url | /eiap-plus/task/withdrawtask/withdraw |
| --- | --- |
| method | POST |
| header | {"Content-Type":"application/json;charset=UTF-8"} |

#### 3 请求参数说明

参数以JSON格式提交，如下：

``` json
{
  "taskId": "d5d67039-631a-11e8-8d04-0686c4000fcf",
  "processInstanceId": "d5c3ab59-631a-11e8-8d04-0686c4000fcf",
  "processDefinitionId": "eiap508870:4:c3bc57e8-631a-11e8-8d04-0686c4000fcf",
  "comment": "77777777777777777777",
  "approvetype": "withdraw"
}
```

#### 4 参数描述

| 参数名 | 类型 | 意义 |
| --- | --- | --- |
| taskId | String | 任务id |
| processInstanceId | String | 流程实例Id |
| processDefinitionId | String | 流程定义Id |
| comment | String | 审批意见 |
| approvetype | String | 审批类型 |

#### 5 返回值

成功:返回弃审执行信息。

status code： 200

responseBody ：

``` json
{
  "flag": "success",
  "msg": "弃审成功!"
}
```



失败：

| HTTP Status Code | 可能原因 |
| --- | --- |
| 400 | 参数不合法 |
| 401 | token无效/过期，或用户名与Token不对应 |
| 405 | Method Not Allowed，HTTP请求方法错误 |
| 415 | Unsupported Media Type，没有设置请求头的Content-type |
| 500 | 内部服务器错误 |

