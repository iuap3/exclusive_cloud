# 参照开放接口规范

## 请求参数说明

请求参数就是RefViewModelVO这个vo里的成员变量。

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| refUIType | RefUITypeEnum(枚举） | 否 | 参照类型（CommonRef,RefGrid，RefGridTree，RefTree,Custom） |
| defaultFieldCount | int | 否 | 默认显示字段中的显示字段数----表示显示前几个字段  |
| strFieldCode | String[] | 否 | 业务表的字段编码 |
| strFieldName | String[] | 否 | 业务表的字段中文名  |
| strHiddenFieldCode | String[] | 否 | 隐藏的字段  |
| refCodeNamePK | String[] | 否 | 参照编码&#124;名称&#124;主键  |
| isUseDataPower | boolean | 否 | 是否使用数据权限，默认为true |
| isMultiSelectedEnabled | boolean | 否 | 参照是否多选，默认为false |
| isNotLeafSelected | boolean | 否 | 非叶子节点是否可选，默认为true  |
| isCheckListEnabled | boolean | 否 | 焦点进入是否显示搜索的数据，默认不显示（false） |
| rootName | String | 否 | 树的根节点  |
| dataPowerOperation\_Code | String | 否 | 权限动作编码 |
| isReturnCode | boolean | 否 | input框显示编码还是显示名称        ，默认为false |
| pk\_group | String | 否 | 集团 |
| pk\_org | String | 否 | 组织 |
| pk\_user | String | 否 | 用户 |
| isMatchPkWithWherePart | boolean | 否 | 参照setpk匹配数据时，是否包含参照WherePart的开关，默认为true |
| pk\_val | String[] | 否 | 主键数组 |
| isClassDoc | String[] | 否 | 是否为档案类型，默认为false |
| filterPks | String[] | 否 | 按给定的Pks进行过滤 |
| refCode | String | 是 | 参照码，用来查询refUrl |
| callback | String | 是 | Jsonp跨域请求参数 |
| clientParam | String | 否 | 联动参数  |
| cfgParam | String | 否 | 配置参数 |
| content | String | 否 | 用于模糊查询匹配 |
| refModelUrl | String | 否 | 参照模板url |
| condition | String | 否 | 判断条件参数 |
| treeloadData | String | 否 | 默认为false，设置树是否为懒加载 |
| sysId | String | 否 | 系统id |
| tenantId | String | 否 | 租户id |
| refModelClassName | String | 否 | 自定义参照模型类  |
| refModelHandlerClass | String | 否 | 参照后端业务切入处理类 |
| pks | String | 否 | 数据的pk  |
| refName | String | 否 | 参照名称 |
| isTreeAsync | boolean | 否 | 同步加载树，默认false |
| refClientPageInfo | RefClientPageInfo | 否 | 分页信息类 |
| transmitParam | String | 否 | 每一列显示几条数据 |

## 获取参照数据头接口

请求体

| URl | /newref/rest/iref\_ctr/refInfo |
| --- | --- |
| Method | GET |

常用请求参数：

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| refCode | String | 是 | 参照码，用来查询refUrl |
| callback | String | 是 | Jsonp跨域请求参数 |

返回值：

{

&quot;refUIType&quot;:&quot;RefGrid&quot;,

&quot;refCode&quot;:&quot;anotherNewUser&quot;,

&quot;defaultFieldCount&quot;:3,

&quot;strFieldCode&quot;:[&quot;refcode&quot;,&quot;refname&quot;,&quot;mobile&quot;,&quot;email&quot;],

&quot;strFieldName&quot;:[&quot;编码&quot;,&quot;名称&quot;,&quot;手机号&quot;,&quot;邮箱&quot;],

&quot;rootName&quot;:&quot;用户列表&quot;, &quot;refModelUrl&quot;:&quot;/wbalone/anotherNewUserRef/&quot;,

&quot;refName&quot;:&quot;用户&quot;,

 &quot;refClientPageInfo&quot;:{

&quot;pageSize&quot;:100,

&quot;currPageIndex&quot;:0,

&quot;pageCount&quot;:0

},

&quot;refVertion&quot;:&quot;NewRef&quot;

}

参数说明：&quot;refVertion&quot;显示当前参照版本，NewRef表示新参照。其他参数详见6.1请求参数说明。

## 获取树参照

请求体

| URl | /newref/rest/iref\_ctr/blobRefTree |
| --- | --- |
| Method | GET |

常用请求参数：

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| refCode | String | 是 | 参照码，用来查询refUrl |
| callback | String | 是 | Jsonp跨域请求参数 |
| treeloadData | String | 否 | 默认为false，设置树是否为懒加载 |
| sysId | String | 否 | 系统id |
| content | String | 否 | 查询字段，如果有值，根据其值进行查询 |
| condition | String | 否 | 可以传递一些判断条件 |

返回值：

{

&quot;data&quot;:[{&quot;children&quot;:[

{&quot;children&quot;:[

{&quot;children&quot;:[],&quot;pid&quot;:&quot;5f2f4f27-85ea-4faf-9440-953285b4f5dd&quot;,&quot;id&quot;:&quot;b6184b42-0f85-48e5-b0d9-1db4ea62000f&quot;,&quot;refcode&quot;:&quot;4ee&quot;,&quot;refpk&quot;:&quot;b6184b42-0f85-48e5-b0d9-1db4ea62000f&quot;,&quot;isLeaf&quot;:&quot;true&quot;,&quot;refname&quot;:&quot;edd&quot;

}],&quot;pid&quot;:&quot;a1a41833-6652-421b-b375-115dedce94ae&quot;,&quot;id&quot;:&quot;5f2f4f27-85ea-4faf-9440-953285b4f5dd&quot;,&quot;refcode&quot;:&quot;1&quot;,&quot;refpk&quot;:&quot;5f2f4f27-85ea-4faf-9440-953285b4f5dd&quot;,&quot;isLeaf&quot;:false,&quot;refname&quot;:&quot;1&quot;},

{&quot;children&quot;:[],&quot;pid&quot;:&quot;a1a41833-6652-421b-b375-115dedce94ae&quot;,&quot;id&quot;:&quot;d0136bc3-fe19-4164-8886-3450856adf90&quot;,&quot;refcode&quot;:&quot;beijing&quot;,&quot;refpk&quot;:&quot;d0136bc3-fe19-4164-8886-3450856adf90&quot;,&quot;isLeaf&quot;:&quot;true&quot;,&quot;refname&quot;:&quot;北京总部&quot;},

{&quot;children&quot;:[],&quot;pid&quot;:&quot;a1a41833-6652-421b-b375-115dedce94ae&quot;,&quot;id&quot;:&quot;921c9b4a-976e-4624-bff7-eee035a7e9c0&quot;,&quot;refcode&quot;:&quot;wl&quot;,&quot;refpk&quot;:&quot;921c9b4a-976e-4624-bff7-eee035a7e9c0&quot;,&quot;isLeaf&quot;:&quot;true&quot;,&quot;refname&quot;:&quot;用友网络&quot;}],  &quot;pid&quot;:&quot;null&quot;,&quot;id&quot;:&quot;a1a41833-6652-421b-b375-115dedce94ae&quot;,&quot;refcode&quot;:&quot;01&quot;,&quot;refpk&quot;:&quot;a1a41833-6652-421b-b375-115dedce94ae&quot;,&quot;isLeaf&quot;:false,&quot;refname&quot;:&quot;用友&quot;}],

&quot;page&quot;:{&quot;pageSize&quot;:100,&quot;currPageIndex&quot;:0,&quot;pageCount&quot;:0},

&quot;allpks&quot;:null

}

参数说明：

  data：参照树数据

children:子节点

pid:父节点的id

id：该节点的id

refcode：节点编码

refpk:当前树节点的唯一标识

isLeaf：是否是叶子节点

refname：节点名称

page：分页信息（在树型参照中无用）

pageSize：每页数据数量

currPageIndex：当前页

pageCount：总页数

## 获取通用参照

请求体

| URl | /newref/rest/iref\_ctr/commonRefsearch |
| --- | --- |
| Method | GET |

常用请求参数：

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| refCode | String | 是 | 参照码，用来查询refUrl |
| callback | String | 是 | Jsonp跨域请求参数 |
| sysId | String | 否 | 系统id |
| content | String | 否 | 查询字段，如果有值，根据其值进行查询 |
| locale | String | 否 | 国际化字段 |
| transmitParam | String | 否 | 每一列显示几条数据 |

返回值：

{

&quot;data&quot;:[

{&quot;peocode&quot;:&quot;liukunlin&quot;,&quot;refpk&quot;:&quot;14e0220f-1a86-4861-8f74-23323e321263&quot;,&quot;id&quot;:&quot;14e0220f-1a86-4861-8f74-23323e321263&quot;,&quot;refcode&quot;:&quot;liukunlin&quot;,&quot;peoname&quot;:&quot;刘坤琳&quot;,&quot;refname&quot;:&quot;刘坤琳&quot;},

{&quot;peocode&quot;:&quot;liu&quot;,&quot;refpk&quot;:&quot;14e0220f-1a86-4861-8f74-3332332kjffo&quot;,&quot;id&quot;:&quot;14e0220f-1a86-4861-8f74-3332332kjffo&quot;,&quot;refcode&quot;:&quot;liu&quot;,&quot;peoname&quot;:&quot;刘志鹏&quot;,&quot;refname&quot;:&quot;刘志鹏&quot;}

]

,&quot;page&quot;:{&quot;pageSize&quot;:100,&quot;currPageIndex&quot;:0,&quot;pageCount&quot;:1},

&quot;allpks&quot;:null

}

参数说明：

peocode：人员编码（属性）

peoname:人员名称（属性）

refpk:当前数据的唯一标识

id:当前数据的id（属性）

refcode：参照数据编码

refname：参照数据的名称

## 获取单表参照

请求体

| URl | /newref/rest/iref\_ctr/blobRefTree |
| --- | --- |
| Method | GET |

常用请求参数：

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| refCode | String | 是 | 参照码，用来查询refUrl |
| callback | String | 是 | Jsonp跨域请求参数 |
| sysId | String | 否 | 系统id |
| content | String | 否 | 查询字段，如果有值，根据其值进行查询 |
| condition | String | 否 | 可以传递一些判断条件 |
| refClientPageInfo.currPageIndex | String | 否 | 当前页码-1 |
| refClientPageInfo.pageSize | String | 否 | 每页数据数量 |

返回值：

{

&quot;data&quot;:[{&quot;mobile&quot;:&quot;15011430236&quot;,&quot;refcode&quot;:&quot;yy1&quot;,&quot;refpk&quot;:&quot;a97b2813fbba431cbd1110b3f61b1a58&quot;,  &quot;id&quot;:&quot;a97b2813fbba431cbd1110b3f61b1a58&quot;,&quot;refname&quot;:&quot;用户1&quot;,&quot;email&quot;:&quot;yangyingx@yonyou.com&quot;}],

&quot;page&quot;:{&quot;pageSize&quot;:50,&quot;currPageIndex&quot;:0,&quot;pageCount&quot;:1},

&quot;allpks&quot;:null

}

参数说明

mobile,refcode,refname,email为数据头自定义的属性

refpk:当前数据的唯一标识

id:当前数据的id（属性）

page：分页信息

pageSize：每页数据数量

currPageIndex：当前页

pageCount：总页数

## 获取树表参照

（左侧树）请求体

| URl | /newref/rest/iref\_ctr/blobRefTree |
| --- | --- |
| Method | GET |

常用请求参数：

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| refCode | String | 是 | 参照码，用来查询refUrl |
| sysId | String | 否 | 系统id |
| content | String | 否 | 查询字段，如果有值，根据其值进行查询 |

返回值同树型参照。

（右侧表）请求体

| URl | /newref/rest/iref\_ctr/blobRefTreeGrid |
| --- | --- |
| Method | GET |

常用请求参数：

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| refCode | String | 是 | 参照码，用来查询refUrl |
| condition | String | 是 | 树的节点id，用于查询表数据 |
| sysId | String | 否 | 系统id |
| content | String | 否 | 查询字段，如果有值，根据其值进行查询 |

返回值同单表型参照。

## 通过pk(id)匹配

请求体

| URl | /newref/rest/iref\_ctr/matchPKRefJSON |
| --- | --- |
| Method | POST |

常用请求参数：

参数以json格式提交

{   refCode:&quot;xxx&quot;,

id:&quot;xxx&quot;,

callback:&quot;xxx&quot;

}

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| refCode | String | 是 | 参照码，用来查询refUrl |
| callback | String | 是 | Jsonp跨域请求参数 |
| pk\_val | String | 否 | 匹配的id |

返回值：

{

&quot;data&quot;:[{&quot;refcode&quot;:&quot;001&quot;,&quot;refpk&quot;:&quot;8f503c57-7532-47b4-b038-955866e151be&quot;,&quot;id&quot;:&quot;8f503c57-7532-47b4-b038-955866e151be&quot;,&quot;refname&quot;:&quot;用友1&quot;}],

&quot;page&quot;:{&quot;pageSize&quot;:50,&quot;currPageIndex&quot;:0,&quot;pageCount&quot;:0},

&quot;allpks&quot;:null

}

参数说明：

data:返回的数据

refpk:当前数据的唯一标识

id:当前数据的id（属性）

refcode：参照数据编码

refname：参照数据的名称

page：分页信息

pageSize：每页数据数量

currPageIndex：当前页

pageCount：总页数

## 校验手动录入的参照数据

请求体

| URl | /newref/rest/iref\_ctr/matchBlurRefJSON |
| --- | --- |
| Method | POST |

常用请求参数：

参数以json格式提交

{   refCode:&quot;xxx&quot;,

id:&quot;xxx&quot;,

callback:&quot;xxx&quot;

}

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| refCode | String | 是 | 参照码，用来查询refUrl |
| callback | String | 是 | Jsonp跨域请求参数 |
| content | String | 是 | 输入的内容 |

如果输入的数据不合法，输入的数据会消失；如果输入的数据无误，返回查询结果。

返回值：

{

&quot;data&quot;:[{&quot;refcode&quot;:&quot;001&quot;,&quot;refpk&quot;:&quot;8f503c57-7532-47b4-b038-955866e151be&quot;,&quot;id&quot;:&quot;8f503c57-7532-47b4-b038-955866e151be&quot;,&quot;refname&quot;:&quot;用友1&quot;}],

&quot;page&quot;:{&quot;pageSize&quot;:50,&quot;currPageIndex&quot;:0,&quot;pageCount&quot;:0},

&quot;allpks&quot;:null

}

参数说明：

data:返回的数据

refpk:当前数据的唯一标识

id:当前数据的id（属性）

refcode：参照数据编码

refname：参照数据的名称

page：分页信息

pageSize：每页数据数量

currPageIndex：当前页

pageCount：总页数
