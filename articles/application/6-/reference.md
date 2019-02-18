# 参照组件开发示例

## 数据库表

表名：REF\_REFINFO

| 列名 | 字段含义 | 类型 |
| --- | --- | --- |
| PK\_REF | 主键 | VARCHAR2(200) |
| REFNAME | 参照名称 | VARCHAR2(300) |
| REFCODE | 参照编码 | VARCHAR2(300) |
| REFCLASS | 参照类 | VARCHAR2(300) |
| REFURL | 参照的URL | VARCHAR2(300) |
| MD\_ENTITYPK | 元数据实体主键 | VARCHAR2(300) |
| PRODUCTTYPE | 产品类型 | VARCHAR2(300) |
| TENANTID | 租户id | VARCHAR2(300) |

## 后端开发流程

1. 配置环境  
组件采用Maven进行编译和打包发布，其对外提供的依赖方式如下  
&lt;dependency&gt;  
&lt;groupId&gt;com.yonyou.iuap&lt;/groupId&gt;      
&lt;artifactId&gt;uitemplate_common&lt;/artifactId&gt;      
&lt;version&gt;2.0.1-ref-profession-RC001&lt;/version&gt;        
&lt;/dependency&gt;

2. 创建参照事件控制类（Controller），继承平台参照事件控制类（也可以不继承，有同样的方法即可），设置数据查询信息实体返回值。

3. 在数据库的REF\_REFINFO表中进行注册。

### 单表型参照

a.继承：com.yonyou.iuap.ref.sdk.refmodel.model.AbstractTreeGridRefModel

  b.实现方法：getRefModelInfo  blobRefSearch

 	@Controller
	@RequestMapping(value = "/danbiao")
	public class DanBiaoRefController extends AbstractTreeGridRefModel {
   
    	@RequestMapping( value = {"/getRefModelInfo"},method = {RequestMethod.POST})
    	@ResponseBody
    	public RefViewModelVO getRefModelInfo(@RequestBody RefViewModelVO refViewModel) {
        
        return refViewModel;
    	}
  
	    @RequestMapping(value = {"/blobRefSearch"}, method = {RequestMethod.POST})
	    @ResponseBody
	    public Map<String, Object> blobRefSearch(@RequestBody RefViewModelVO refViewModelVO) {
	        Map<String, Object> results = new HashMap<>();
	        return results;
	    }
        

getRefModelInfo方法要来设置数据头和数据体，blobRefTree方法用来获取数据体。

具体示例如下：
![](/articles/application/6-/images/reference/9.png)

![](/articles/application/6-/images/reference/10.png)

![](/articles/application/6-/images/reference/11.png)


通过getRefModelInfo方法设置了参照页面的参照名称，以及表参照的表头，通过blobRefTree方法查询了数据体，设置了分页参数。

在数据库中进行注册：
![](/articles/application/6-/images/reference/12.png)




### 树型参照

 a.继承：com.yonyou.iuap.ref.sdk.refmodel.model.AbstractTreeGridRefModel

 b.实现方法：blobRefTree

	@Controller
	@RequestMapping(value = "/tree")
	public class TreeRefController extends AbstractTreeGridRefModel {
	    @RequestMapping(value = {"/blobRefTree"}, method = {RequestMethod.POST})
	    @ResponseBody
	    public Map<String, Object> blobRefTree(@RequestBody RefViewModelVO refViewModelVO) {
	        Map<String, Object> results = new HashMap<>();
	        return results;
	    }	


blobRefTree方法用来获取参照树。

具体示例如下：
![](/articles/application/6-/images/reference/13.png)

![](/articles/application/6-/images/reference/14.png)


在数据库中进行注册：
![](/articles/application/6-/images/reference/15.png)




### 树表型参照

 a.继承：com.yonyou.iuap.ref.sdk.refmodel.model.AbstractTreeGridRefModel

 b.实现方法：blobRefTree getRefModelInfo  blobRefSearch

	@Controller
	@RequestMapping(value = "/treeGrid")
	public class TreeGridRefController extends AbstractTreeGridRefModel {
	@RequestMapping( value = {"/getRefModelInfo"},method = {RequestMethod.POST})
	    @ResponseBody
	    public RefViewModelVO getRefModelInfo(@RequestBody RefViewModelVO refViewModel) {
	        
	        return refViewModel;
	    }
	
	    @RequestMapping(value = {"/blobRefTree"}, method = {RequestMethod.POST})
	    @ResponseBody
	    public Map<String, Object> blobRefTree(@RequestBody RefViewModelVO refViewModelVO) {
	        Map<String, Object> results = new HashMap<>();
	        return results;
	    }	
	
	@RequestMapping(value = {"/blobRefSearch"}, method = {RequestMethod.POST})
	    @ResponseBody
	    public Map<String, Object> blobRefSearch(@RequestBody RefViewModelVO refViewModelVO) {
			Map<String, Object> results = new HashMap<>();
			        return results;
			    }	


getRefModelInfo方法要来设置数据头，blobRefTree方法用来获取参照树，blobRefTree方法用来获取单表数据体。

具体示例如下：
![](/articles/application/6-/images/reference/16.png)

![](/articles/application/6-/images/reference/17.png)

![](/articles/application/6-/images/reference/18.png)

![](/articles/application/6-/images/reference/19.png)


在数据库中进行注册：
![](/articles/application/6-/images/reference/20.png)


### 通用型参照

 a.继承：com.yonyou.iuap.ref.sdk.refmodel.model.AbstractCommonRefModel

 b.实现方法：getCommonRefData

	@Controller
	@RequestMapping(value = "/checkbox")
	public class CheckboxRefController extends AbstractTreeGridRefModel {
	
	    @RequestMapping(value = {"/getCommonRefData"}, method = {RequestMethod.POST})
	    @ResponseBody
	    public Map<String, Object> getCommonRefData(@RequestBody RefViewModelVO refViewModelVO) {
	        Map<String, Object> results = new HashMap<>();
	        return results;
	    }	


getCommonRefData方法用来获取参照数据。

具体示例如下：


![](/articles/application/6-/images/reference/21.png)

![](/articles/application/6-/images/reference/22.png)


在数据库中进行注册：
![](/articles/application/6-/images/reference/23.png)


## 前端开发流程

1. 加载:  
a.利用es模块加载的方式引入参照组件,引入 createModal 方法;  
b.利用script引入参照打包的js文件(CDN:https://cdn.yonyoucloud.com/yyuap/ref/1.1.1/yyuap-ref.js),window上绑定createModal方法;  
c.利用amd或者require加载,会暴露出createModal方法;  
d. (推荐)支持npm加载方式(npm install yyuap-ref);引入方式: import createModal from yyuap-ref

2. 使用:  
a.根据下述参数说明进行配置,作为一个option参数;  
b.利用createModal(option)即可创建参照;

3. 注销:  
a.createModal方法的返回值为destory方法,调用可注销参照.  
b.已集成到按钮组件中,无需单独调用destory方法,在点击保存、取消时自动注销.

4. 参数说明
  
		const option = {   
			title:'弹窗标题',
			refType:3,
			isRadio:false,
			hasPage:true,
			backdrop:true,
			showLine:false,
			openIconClassName:"uf-reduce-s-o",
			closeIconClassName:"uf-add-s-o", 
	   	 	multiple:false,
	    	treeloadData:false, 
	    	defaultExpandAll:false, 
	    	isDefaultSelect:true, 
	    	checkAllChildren:false, 
	    	paginationOption: {gap: false}, 
	    	emptyBtn:true, 
	    	tabData:[ ],
	    	buttonText:{ok:"确定",cancel:"取消"},//按钮文字配置
	    	param:{       
		  		refCode:'test_treeTable',        
		  		locale:'zh_CN'    },
	    	refModelUrl:{
	        	TreeUrl:'http://workbench.yyuap.com/ref/rest/iref_ctr/blobRefTree', //树请求
	        	TreeGridUrl:'http://workbench.yyuap.com/ref/rest/iref_ctr/blobRefTree', //树表树请求
	        	GridUrl:'http://workbench.yyuap.com/ref/rest/iref_ctr/commonRefsearch',//单选多选请求
	        	TableBodyUrl:'http://workbench.yyuap.com/ref/rest/iref_ctr/blobRefTreeGrid',//表体请求
	        	TableBarUrl:'http://workbench.yyuap.com/ref/rest/iref_ctr/refInfo',//表头请求
	        	totalDataUrl:'http://workbench.yyuap.com/ref/diwork/iref_ctr/matchPKRefJSON',//根据refcode请求完整数据
	    	},
	    	checkedArray:[],//已选中数据回填
	    	onCancel: function (p) {
	        	console.log(p)
	    	},
	    	onSave: function (sels) {
	        	console.log(sels);
	    	},
	    	onBeforeAjax: function (p) {
	        	console.log(p)
	    	},
	    	onAfterAjax: function (p) {
	        	console.log(p);
	    	},
	    	className:'',
		}

| **参数** | **说明** | **类型** |
| --- | --- | --- |
| title | 弹窗标题，适配 reftype =1234 | string |
| refType | 参照类型：1:树形 2.单表 3.树卡型 4.多选 5.其他（默认树卡） | number |
| isRadio | 单表参照是否单选：1.true 单选 2.false多选 default:false | boolean |
| hasPage | 分页标志 true:带分页 false:不带分页 default:false | boolean |
| backdrop | 是否显示遮罩层 可不传 default:true | boolean |
| showLine | 树参照是否带有连线 default:false | boolean |
| openIconClassName | 树参照展开图标设置 （只支持tinper-bee图标） | String |
| closeIconClassName | 树参照关闭图标设置 （只支持tinper-bee图标） | String |
| paginationOption | 分页组件参数传递 （只支持tinper-pagination 1.0.2版本参数） | object |
| treeloadData | 树参照是否异步加载 default:false | boolean |
| tabData | 标签数据 每个tab标签含有title与key option中可增加defaultActiveKey作为默认tab标签 | Array&lt;Object&gt; |
| param | url的请求参数 | object |
| refModelUrl | url的请求地址 | object |
| checkedArray | 已选择数据项 | Array&lt;Object&gt; |
| onCancel | 取消时回调 | function |
| onSave | 确认时回调 | function |
| onBeforeAjax | 打开参照时回调 | function |
| onAfterAjax | 数据请求成功后回调 | function |
| className | 参照样式控制 | String |
| totalDataUrl | checkedArray为[&#39;refcode&#39;],请求数据 | String |
| multiple | 树参照下提供复选 (单树) | Boolean |
| defaultExpandAll | 树参照节点是否全部展开 | Boolean |
| isDefaultSelect | 树表默认是否选择第一个节点(默认true) | Boolean |
| refShowClassCode | 树表/表 类型 分类传参健值 |   |
