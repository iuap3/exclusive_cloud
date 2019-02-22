# 使用者门户

首先从使用者门户开始介绍具体使用流程。使用者门户为使用者提供使用API的能力，使用者的整个功能操作分为API使用、授权模式和运营数据管理三个模块。

## API使用

API使用是用户使用API的整体流程。整个流程是从API的申请到API的调用以及运维数据的查看。

### API申请

使用者在调用API前第一步要对需要的API进行权限申请。申请操作是为用户提供了调用API的权限。
* 进入API中心

进入API网关首页后，选择`【API中心】`页签。API中心为API开发者将自己的服务部署在API网关提供了平台，同时为使用者提供了丰富的API服务供使用。
 
![API中心](/articles/apigateway/5-/images/img2.1.1-1.png)

* 立即使用

选择要使用的API，点击`【立即使用】`后，再点击`【申请】`。
 
![使用API](/articles/apigateway/5-/images/img2.1.1-2.png)

* API列表

进入使用者门户，上一步申请的API会在`“我的应用”`页签显示，此时API申请完成。

![API列表页面](/articles/apigateway/5-/images/img2.1.1-3.png)

### API测试

API网关为用户提供了API测试的功能，供用户测试API是否可以调通。

* 测试API

在“我的应用”页签，选择要监控的API点击【测试】按钮。这里需要注意的是，如果申请的服务开通类型是手动时，申请服务后需要API开发者审批后才可进行调用和测试操作。输入正确参数，点击【测试】按钮。其中API网关提供了两种安全可靠的认证方式，分别为apicode和appkey进行用户认证，详情请见授权模式。用户可根据需要进行不同方式的API调用。

![API测试](/articles/apigateway/5-/images/img2.1.2-1.png)

* 测试结果

请求结果中包含了请求头、请求体和响应体，可以根据返回数据判断API是否调通。若为调通，可根据响应体返回内容判断调用失败的原因。

![测试结果](/articles/apigateway/5-/images/img2.1.2-2.png)

## 授权模式

为了保护您的 API，避免恶意访问、未授权访问、应用漏洞、黑客攻击等导致的数据损失、资产损失，我们提供了多种 API 认证方式和 API 防护策略。API网关在调用API时提供两种全可靠的认证方式，分别通过apicode和appkey进行用户认证。

### apicode 授权

apicode授权方式是指，使用者进行API申请完成后，API网关会为申请完成的API提供一个apicode值，使用者可以根据apicode值进行调用API。

### appkey + appsecret 授权

您可以使用 `secret_id` 和 `secret_key` 对您的 API 进行认证管理。`secret_id` 和 `secret_key` 成对出现。授权方式是依靠一组相关的API来实现。通过创建应用将一组特定功能的API关联起来，帮助用户高效、便捷的管理API。`Appkey` 是API网关为应用提供的授权方式，使用者可以根据 `appkey` 调用此应用中的全部API。

* 添加应用

在应用授权页签，选择【添加应用】。

![添加应用](/articles/apigateway/5-/images/img2.2.2-1.png)

## 运营数据

API网关除了提供API使用和调用的权限控制外，还提供了基本运营数据的支持。

### 数据监控

服务监控是指对API调用数据的监控。API网关为数据监控提供了两个入口，分别在数据监控页签和我的应用页签，选择要监控的API的【监控】进行数据监控的查询。数据监控提供调用次数、调用字节数和调用时长的查询。此监控数据统计为五分钟一次。

* 调用次数

使用者可以选择指定API，查询当天或按月查询该API调用次数。

![调用次数](/articles/apigateway/5-/images/img2.3.1-1.png)

* 调用字节数

使用者可以选择指定API，查询当天或按月查询该API调用字节数。

![调用字节数](/articles/apigateway/5-/images/img2.3.1-2.png)

* 调用时长

使用者可以选择指定API，查询当天或按月查询该API调用时长。

![调用时长](/articles/apigateway/5-/images/img2.3.1-3.png)

### 日志查询

在日志查询页签，选择选择服务和API，可以查询到调用日志信息。

![日志查询](/articles/apigateway/5-/images/img2.3.2-1.png)

* 查询条件

查询条件：服务选择，API选择，日志时间。
通过选择服务和API确定要查询的API，通过选择时间来确定要查询日志的具体时间。

### 白名单

白名单是提供访问安全，通过对指定API设置可以访问的IP来提高安全性。

* 操作步骤

在白名单页签，勾选需要指定IP的API，增加访问白名单，进行保存。设置完成后，只有在白名单内的IP地址才能访问到指定API。实现了API不能随意访问，保证安全性。

### SDK文档

API网关提供API的SDK文档，实现方便、便捷的进行开发。

* SDK文档下载

在SDK文档页签，选择指定API的SDK文档进行下载。

![SDK文档](/articles/apigateway/5-/images/img2.3.4-1.png)
