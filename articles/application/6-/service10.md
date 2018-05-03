## 参照开发

### 业务需求

参照开发主要用于用户抽取常用数据，进行选取，例如组织、部门、岗位等。

### 功能说明

newref组件为业务提供了选择组织、部门、岗位等选择。


1.提供通用型参照（单选多选）

2.提供表型参照

3.提供树形参照

4.提供树表型参照

## 整体设计

### 依赖环境

组件采用Maven进行编译和打包发布，其对外提供的依赖方式如下：

<div class="lines">
    <div class="line alt1">
        <table>
            <tbody>
                <tr>
                    <td class="number"><code>1</code></td>
                    <td class="content">
                        <code class="plain">&lt;dependency&gt;</code>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    <div class="line alt2">
        <table>
            <tbody>
                <tr>
                    <td class="number"><code>2</code></td>
                    <td class="content">
                        <code class="spaces">&nbsp;&nbsp;</code>
                        <code class="plain">&lt;groupId&gt;com.yonyou.iuap.pap.uitemplate&lt;/groupId&gt;</code>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    <div class="line alt1">
        <table>
            <tbody>
                <tr>
                    <td class="number"><code>3</code></td>
                    <td class="content">
                        <code class="spaces">&nbsp;&nbsp;</code>
                        <code class="plain">&lt;artifactId&gt;newref&lt;/artifactId&gt;</code>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    <div class="line alt2">
        <table>
            <tbody>
                <tr>
                    <td class="number"><code>4</code></td>
                    <td class="content">
                        <code class="spaces">&nbsp;&nbsp;</code>
                        <code class="plain">&lt;version&gt;${iuap.modules.version}&lt;/version&gt;</code>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    <div class="line alt1">
        <table>
            <tbody>
                <tr>
                    <td class="number"><code>5</code></td>
                    <td class="content"><code class="plain">&lt;/dependency&gt;</code></td>
                </tr>
            </tbody>
        </table>
    </div>
</div>

${iuap.modules.version} 为平台在maven私服上发布的组件的version。


### 功能说明

newref组件为业务提供了选择组织、部门、岗位等选择。
1.提供通用型参照（单选多选）

提供链接：newref/rest/iref_ctr/commonRefsearch

2.提供表型参照

提供链接：
表头：newref/rest/iref_ctr/refInfo
表体：newref/rest/iref_ctr/blobRefTreeGrid

3.提供树形参照

提供链接：newref/rest/iref_ctr/blobRefTree

4.提供树表型参照
提供链接：
树：newref/rest/iref_ctr/blobRefTree
表头：newref/rest/iref_ctr/refInfo
表体：newref/rest/iref_ctr/blobRefTreeGrid



主要工作流程如下：

1.调用参照的相应入口链接。

2.参照系统根据前端传入refcode和其他RefViewModelVO参数获取具体的参照数据。

3.参照系统整理参照数据并返回到前端。

4.前端根据数据展示相应参照

## 使用说明

### 组件包说明

newref组件向外提供参照数据获取的总入口。

### 传值说明

根据 com.yonyou.iuap.ref.model.RefViewModelVO 的类属性进行传值。


### 开发步骤

可直接参考岗位参照工程：

1.列表参照

     a.继承：com.yonyou.iuap.ref.sdk.refmodel.model.AbstractCommonRefModel
     b.实现方法：getCommonRefData

2.列表参照

     a.继承：com.yonyou.iuap.ref.sdk.refmodel.model.AbstractTreeGridRefModel
     b.实现方法：getRefModelInfo  blobRefSearch

3.树形参照

     a.继承：com.yonyou.iuap.ref.sdk.refmodel.model.AbstractTreeGridRefModel
     b.实现方法：blobRefTree 
     

4.树表参照

     a.继承：com.yonyou.iuap.ref.sdk.refmodel.model.AbstractTreeGridRefModel
     b.实现方法：blobRefTree getRefModelInfo  blobRefSearch

开发参照返回数据格式示例

1.列表参照

        {
            "dataList": [{
                    "refremark": "备注",
                    "refpk": "d1a7941a-b342-4d3f-9e20-efc1663f9a0a",
                    "refcode": "code1",
                    "refname": "事业部"…
                },
                {
                    "refremark": "备注",
                    "refpk": "id",
                    "refcode": "参照编码",
                    "refname": "参照名称"…
                },
                "refViewModel": {
                    "refCode":   "1",
                    "refClientPageInfo": {
                        "pageSize":  50,
                        "currPageIndex":  0,
                        "pageCount":  2
                    }
                }
        }

2.列表参照
    
    表头：
        {
            "refUIType": "RefGridTree",
            "refCode": "1",
            "defaultFieldCount": 3,
            "strFieldCode": [
                "refcode",
                "refname",
                "memo"
            ],
            "strFieldName": [
                "编码",
                "名称",
                "备注"
            ],
            "rootName": "岗位列表",
            "refName": "岗位参照",
            "refClientPageInfo": {
                "pageSize": 100,
                "currPageIndex": 0,
                "pageCount": 0
            }
        }
   
    表体：
        {
            "dataList": [{
                "pid": "",
                "refpk": "857c41b7-e1a3-11e5-aa70-0242ac11001d",
                "refcode": "code2",
                "id": "d1a7941a-b342-4d3f-9e20-efc1663f9a0a",
                "refname": "经理"
            }, {
                "pid": "",
                "refpk": "id",
                "refcode": "编码",
                "id": "id",
                "refname": "名称"
            }],
            "refViewModel": {
                "refClientPageInfo": {
                    "pageSize": 100,
                    "currPageIndex": 0,
                    "pageCount": 0
                }
            }
        }

3.树形参照

        {
            "dataList": [{
                "pid": "b5067e31-fbbf-449c-af4d-081e5325c9ea",
                "refpk": "857c41b7-e1a3-11e5-aa70-0242ac11001d",
                "refcode": "code4",
                "id": "857c41b7-e1a3-11e5-aa70-0242ac11001d",
                "refname": "市场部"
            }, {
                "pid": "",
                "refpk": "b5067e31-fbbf-449c-af4d-081e5325c9ea",
                "refcode": "code3",
                "id": "b5067e31-fbbf-449c-af4d-081e5325c9ea",
                "refname": "总部"
            }],
            "refViewModel": {
                "refClientPageInfo": {
                    "pageSize": 100,
                    "currPageIndex": 0,
                    "pageCount": 0
                }
            }
        }     

4.树表参照
    
        见列表和树形参照

