# 编码规则API文档

## 一、简介

编码规则模块提供了外部调用的Rest服务，实现获取编码和回退编码功能。

## 二、API接口

### 1获取编码

1.1描述

请求编码规则模块获取一个编码

1.2请求方法

/billcoderest/getBillCode

1.3请求方式

post

1.4请求参数说明

| 参数 | 参数说明 |
| --- | --- |
| billObjCode | 编码对象code |
| pkAssign | 分配对象主键 |
| billVo | 待编码对象 |

1.5返回参数说明

| 参数 | 参数说明 |
| --- | --- |
| billcode | 编码模块返回的编码值 |
| status | 请求编码是否成功标志 |
| msg | 错误信息提示 |

1.6使用示例

data.put(&quot;billObjCode&quot;, billObjCode);

data.put(&quot;pkAssign&quot;, pkAssign);

data.put(&quot;billVo&quot;, billvo);

JSONObject getbillcodeinfo = RestUtils.getInstance().doPost(getcodeurl, data, JSONObject.class);

### 2回退编码

2.1描述

回退编码号，以保证编码连号的业务需要

2.2请求方法

/billcoderest/returnBillCode

2.3请求方式

Post

2.4请求参数说明

| 参数 | 参数说明 |
| --- | --- |
| billObjCode | 编码对象code |
| pkAssign | 分配对象主键 |
| billVo | 待编码对象 |
| billCode | 待回退编码值 |

2.5返回参数说明

| 参数 | 参数说明 |
| --- | --- |
| status | 是否成功标志 |
| msg | 错误信息提示 |

2.6使用示例

Map&lt;String, String&gt; data = new HashMap&lt;String, String&gt;();

data.put(&quot;billObjCode&quot;, billObjCode);

data.put(&quot;pkAssign&quot;, pkAssign);

data.put(&quot;billVo&quot;, billvo);

data.put(&quot;billCode&quot;, billCode);

JSONObject returnbillcodeinfo=RestUtils.getInstance().doPost(returncodeurl, data, JSONObject.class);