# CMDM-API接口文档规范

主数据提供数据写入和查询的REST服务。各服务地址和参数说明如下。

## 主数据写入服务-insertMd

数据写入数据服务 for 主数据建模I 生成的主数据：

```
主数据写入服务-insertMd
```

## 接口说明

数据写入数据服务 for 主数据建模I 生成的主数据。根据id值是否在主数据存在，区别数据新增OR更新。

## 接口URI

1.http://ip:port/iuapmdm/cxf/mdmrs/center/com.yonyou.iuapmdm.centerService/insertMd

## 请求方式

```
POST
```

## 请求参数
1、POST请求参数：
{
    "systemCode":"远程系统编码",
    "gdCode":"主数据定义编码",
    "masterData":"主数据（json数组格式）"
}

json数组格式：
[{"id":"bin1001","code":"c1001","name":"n1001"},{"id":"bin1002","code":"c1002","name":"n1002"}]
具体属性根据配置修改。

2、header请求参数：
header里面需要两个参数,用于认证合法性:
mdmtoken:远程系统的令牌,这个值是远程系统节点，系统对应的认证令牌；
tenantid租户id (默认写：tenant)；

## 返回参数

返回结果
{
    "data":"返回的主数据(json)",
    "success":"MDM与第三方系统交互成功与否",
    "message":"结果描述信息"
}


## 主数据写入服务-insertDynaMd

数据写入数据服务 for 主数据建模II 生成的主数据：

```
主数据写入服务-insertDynaMd
```

## 接口说明

数据写入数据服务 for 主数据建模II 生成的主数据。根据id值是否在主数据存在，区别数据新增OR更新。


## 接口URI

1.http://ip:port/iuapmdm/cxf/mdmrs/center/com.yonyou.iuapmdm.centerService/insertDynaMd

## 请求方式

```
POST
```

## 请求参数

1、POST请求参数
{
    "systemCode":"远程系统编码",
    "gdCode":"新的动态建模主数据编码",
    "masterData":"主数据（json数组格式）"
}

2、header请求参数，
header里面需要两个参数,用于认证合法性:
mdmtoken:远程系统的令牌,这个值是远程系统节点，系统对应的认证令牌；
tenantid租户id (默认写：tenant)；

## 返回参数

返回结果
{
    "data":"返回的主数据(json)",
    "success":"MDM与第三方系统交互成功与否",
    "message":"结果描述信息"
}


##主数据查询服务-queryListMdByIds

数据查询数据服务 for 主数据建模I 生成的主数据：

```
主数据查询服务-queryListMdByIds
```

## 接口说明

数据写入数据服务 for 主数据建模I 生成的主数据。根据id列表查询相应远程系统的主数据详细信息。


## 接口URI

1.http://ip:port/iuapmdm/cxf/mdmrs/center/com.yonyou.iuapmdm.centerService/queryListMdByIds

## 请求方式

```
POST
```

## 请求参数

1、POST请求参数
{
    "systemCode":"远程系统编码",
    "gdCode":"主数据定义编码",
    "codes":[id（数组格式）]
}

2、header请求参数，
header里面需要两个参数,用于认证合法性:
mdmtoken:远程系统的令牌,这个值是远程系统节点，系统对应的认证令牌；
tenantid租户id (默认写：tenant)；

## 返回参数

返回结果
{
    "data":"返回的主数据(json)",
    "success":"MDM与第三方系统交互成功与否",
    "message":"结果描述信息"
}

##主数据查询服务-queryDynaListMdByIds

数据查询数据服务 for 主数据建模II 生成的主数据：

```
主数据查询服务-queryDynaListMdByIds
```

## 接口说明

数据写入数据服务 for 主数据建模II 生成的主数据。根据id列表查询相应远程系统的主数据详细信息。


## 接口URI

1.http://ip:port/iuapmdm/cxf/mdmrs/center/com.yonyou.iuapmdm.centerService/queryDynaListMdByIds

## 请求方式

```
POST
```

## 请求参数

1、POST请求参数
{
    "systemCode":"远程系统编码",
    "gdCode":"主数据定义编码",
    "codes":[id（数组格式）]
}

2、header请求参数，
header里面需要两个参数,用于认证合法性:
mdmtoken:远程系统的令牌,这个值是远程系统节点，系统对应的认证令牌；
tenantid租户id (默认写：tenant)；

## 返回参数

返回结果
{
    "data":"返回的主数据(json)",
    "success":"MDM与第三方系统交互成功与否",
    "message":"结果描述信息"
}

## 主数据查询服务-queryListMdByMdmCodes

数据写入数据服务 for 主数据建模I 生成的主数据：

```
主数据查询服务-queryListMdByMdmCodes
```

## 接口说明

数据写入数据服务 for 主数据建模I 生成的主数据。根据MDMCODE列表查询远程系统相应的主数据详细信息。

## 接口URI

1.http://ip:port/iuapmdm/cxf/mdmrs/center/com.yonyou.iuapmdm.centerService/queryListMdByMdmCodes

## 请求方式

```
POST
```

## 请求参数
1、POST请求参数：
{
    "systemCode":"远程系统编码",
    "gdCode":"主数据编码",
    "codes":[主数据编码（数组格式]
}

json数组格式：
[{"id":"bin1001","code":"c1001","name":"n1001"},{"id":"bin1002","code":"c1002","name":"n1002"}]
具体属性根据配置修改。

2、header请求参数：
header里面需要两个参数,用于认证合法性:
mdmtoken:远程系统的令牌,这个值是远程系统节点，系统对应的认证令牌；
tenantid租户id (默认写：tenant)；

## 返回参数

返回结果
{
    "data":"返回的主数据(json)",
    "success":"MDM与第三方系统交互成功与否",
    "message":"结果描述信息"
}


## 主数据查询服务-queryDynaListMdByMdmCodes

数据写入数据服务 for 主数据建模II 查询的主数据：

```
主数据写入服务II-insertDynaMd
```

## 接口说明

数据写入数据服务 for 主数据建模II 生成的主数据。根据MDMCODE列表查询远程系统相应的主数据详细信息。


## 接口URI

1.http://ip:port/iuapmdm/cxf/mdmrs/center/com.yonyou.iuapmdm.centerService/queryDynaListMdByMdmCodes

## 请求方式

```
POST
```

## 请求参数

1、POST请求参数
{
    "systemCode":"远程系统编码",
    "gdCode":"新的物料编码",
    "codes":[主数据编码（数组格式）]
}

2、header请求参数，
header里面需要两个参数,用于认证合法性:
mdmtoken:远程系统的令牌,这个值是远程系统节点，系统对应的认证令牌；
tenantid租户id (默认写：tenant)；

## 返回参数

返回结果
{
    "data":"返回的主数据(json)",
    "success":"MDM与第三方系统交互成功与否",
    "message":"结果描述信息"
}

--------------------
--------------------
## 分发服务规范

## 接口说明

分发数据到远程系统时，主数据主动调用远程系统的REST服务，完成主数据到远程系统的数据同步。
远程系统服务实现的标准如下：


## 接口URI

发布的服务访问地址，配置到“远程系统”节点中分发服务中。

## 请求方式

```
POST
```

## 请求参数

请求参数
 @XmlRootElement(name="distributePostDataVO")
{
    "systemCode":"远程系统编码",
    "mdType":"主数据定义编码或新的物料编码",
    "action":"动作，distribute",
    "masterData":"主数据（json数组格式）",
}


## 返回参数

@XmlRootElement(name="distributeRetVO")
{
    //数组对象@XmlRootElement(name="mdMapingVO")
    "mdMappings":[{"mdmCode":"mdmcode", "entityCode":"实体编码", "busiDataId":"远程id", "errorMsg":"错误信息", "success":"成功标志，布尔型"}], 
    "success":"成功标志，布尔型",
    "message":"返回信息"
}

--------------------