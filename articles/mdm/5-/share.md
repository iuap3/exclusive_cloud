# 主数据共享

主数据共享主要由远程系统、主数据权限、数据装载、装载日志、数据分发、分发条件、分发日志和血缘关系构成。

## 远程系统

点击【主数据共享】→【远程系统】，进入远程系统操作页面，此页面可进行远程系统的新增、修改和删除等操作。

![](/articles/mdm/5-/images/image35.png)
 
<p align="center">图：远程系统</p>


➢ 新增远程系统

单击〖新增〗按钮，弹出下图所示窗口，填写内容后点击〖确定〗。

![](/articles/mdm/5-/images/image36.png)
 
<p align="center">图：新增远程系统</p>
 

例子1：主数据分发到副本：

http://localhost:#port#/iuapmdm/cxf/mdmrs/distribute/distributeService/distributeMd?bakmappingCode(bakmappingCode为主数据对应的副本对照编码)

例子2：主数据分发到第三方服务：

http://localhost:9888/rest/cxf/rs/distribute/distributeService/distributeMd

➢ 修改远程系统

选择一条远程系统记录，单击〖修改〗按钮，弹出如下窗口，修改内容后，单击〖确定〗。

![](/articles/mdm/5-/images/image37.png)
 
<p align="center">图：修改远程系统</p>
 

➢ 删除远程系统

选择远程系统，单击〖删除〗按钮。

➢ 装载分发服务接口说明

供二次开发使用，具体如下：



```
rest服务 
1.
http://ip:port/iuapmdm/cxf/mdmrs/center/com.yonyou.iuapmdm.centerService/insertMd
post请求参数
{
"systemCode":"远程系统编码",
"gdCode":"主数据编码",
"masterData":"主数据（json数组格式）"
}
header里面需要两个参数
mdmtoken远程系统的认证令牌  
tenantid租户id  (tenant)
返回结果
{
"data":"返回的主数据(json)",
"success":"MDM与第三方系统交互成功与否",
"message":"结果描述信息"
}

2.
http://localhost:8082/iuapmdm/cxf/mdmrs/center/com.yonyou.iuapmdm.centerService/queryListMdByMdmCodes
post请求参数
{
"systemCode":"远程系统编码",
"gdCode":"主数据编码",
"codes":[主数据编码（数组格式]
}
header里面需要两个参数
mdmtoken远程系统的认证令牌  
tenantid租户id  (tenant)
返回结果
{
"data":"返回的主数据(json)",
"success":"MDM与第三方系统交互成功与否",
"message":"结果描述信息"
}

3.
http://localhost:8082/iuapmdm/cxf/mdmrs/center/com.yonyou.iuapmdm.centerService/queryListMdByIds
post请求参数
{
"systemCode":"远程系统编码",
"gdCode":"主数据定义编码",
"codes":[id（数组格式）]
}
header里面需要两个参数
mdmtoken远程系统的认证令牌  
tenantid租户id  (tenant)
返回结果
{
"data":"返回的主数据(json)",
"success":"MDM与第三方系统交互成功与否",
"message":"结果描述信息"
}

4.
物料服务
http://ip:port/iuapmdm/cxf/mdmrs/center/com.yonyou.iuapmdm.centerService/insertDynaMd
post请求参数
{
"systemCode":"远程系统编码",
"gdCode":"新的物料编码",
"masterData":"主数据（json数组格式）"
}
header里面需要两个参数
mdmtoken远程系统的认证令牌  
tenantid租户id  (tenant)
返回结果
{
"data":"返回的主数据(json)",
"success":"MDM与第三方系统交互成功与否",
"message":"结果描述信息"
}

5.
http://localhost:8082/iuapmdm/cxf/mdmrs/center/com.yonyou.iuapmdm.centerService/queryDynaListMdByMdmCodes
post请求参数
{
"systemCode":"远程系统编码",
"gdCode":"新的物料编码",
"codes":[主数据编码（数组格式）]
}
header里面需要两个参数
mdmtoken远程系统的认证令牌  
tenantid租户id  (tenant)
返回结果
{
"data":"返回的主数据(json)",
"success":"MDM与第三方系统交互成功与否",
"message":"结果描述信息"
}

6.
http://localhost:8082/iuapmdm/cxf/mdmrs/center/com.yonyou.iuapmdm.centerService/queryDynaListMdByIds
post请求参数
{
"systemCode":"远程系统编码",
"gdCode":"新的物料编码",
"codes":[id（数组格式]
}
header里面需要两个参数
mdmtoken远程系统的认证令牌  
tenantid租户id  (tenant)
返回结果
{
"data":"返回的主数据(json)",
"success":"MDM与第三方系统交互成功与否",
"message":"结果描述信息"
}




分发服务 由业务系统实现，供主数据系统调用
入参 @XmlRootElement(name="distributePostDataVO")
{
"systemCode":"远程系统编码",
"mdType":"主数据定义编码或新的物料编码",
"action":"动作，distribute",
"masterData":"主数据（json数组格式）",
}
出参 @XmlRootElement(name="distributeRetVO")
{
"mdMappings":[{"mdmCode":"mdmcode", "entityCode":"实体编码", "busiDataId":"远程id", "errorMsg":"错误信息", "success":"成功标志，布尔型"}], //数组对象@XmlRootElement(name="mdMapingVO")
"success":"成功标志，布尔型",
"message":"返回信息",
}
```


## 主数据权限

点击【主数据共享】→【主数据权限】，进入主数据权限操作页面，此页面可进行主数据权限的新增、修改和删除等操作。基于主数据建模的操作权限包括：可读、可写、可订阅。数据权限配置过滤条件，用于装载和分发数据过滤。

![](/articles/mdm/5-/images/image38.png)
 
<p align="center">图：主数据权限</p>
 
➢ 新增

选择左侧主数据，单击〖新增〗按钮新增权限，弹出新窗口，输入内容，单击〖确定〗。

![](/articles/mdm/5-/images/image39.png)
 
<p align="center">图：新增主数据权限</p>
 

**主数据**：必输项；

**权限名称**：必输项；

**权限编码**：必输项，若先选择主数据，则会自动匹配到主数据的编码和名称；

**业务系统**：下拉选择；

**过滤条件**：根据条件判断哪些数据可以分发装载；

**条件编辑**：在新增权限的时候，根据实际业务场景设置条件，比如“名称”包含…，等于…或者不等于…；

![](/articles/mdm/5-/images/image40.png)
 
<p align="center">图：条件编辑</p>


**主数据权限**：有可读、可写、可订阅的权限，通过复选框进行选择。如果选择“可读”，表示具有分发权限；选择“可写”，表示具有装载主数据权限；选择“可订阅”，表示具有自动分发权限，如果设置了业务系统对主数据的订阅权限，则维护主数据时，自动分发到业务系统。

➢ 修改

选择定义树的某个主数据，在右侧的权限列表中，选中某个权限，单击〖修改〗，弹出新窗口，修改内容，单击〖确定〗。

![](/articles/mdm/5-/images/image41.png)
 
<p align="center">图：修改主数据权限</p>
 
➢ 删除

选择一行记录，单击〖删除〗按钮，弹出确认窗口。

## 数据装载

主数据装载适用于主数据和物料主数据。

点击【主数据共享】→【数据装载】，进入手动装载操作页面，此页面可选择业务系统进行手动装载操作。

![](/articles/mdm/5-/images/image42.png)
 
<p align="center">图：手动装载</p>

单击业务系统右侧的下拉按钮，选择需要装载的业务系统。

![](/articles/mdm/5-/images/image43.png)
 
<p align="center">图：选择业务系统</p>
 


单击〖手动装载〗按钮，选择Excel文件。此Excel文件是主数据建模的〖导出excel模板〗所导出的，并需要填入内容。

**注意**：数据装载时，在EXCEL表中，如果是日期时间类型的字段，数据前需要加’，举例：入职时间是'2016-01-01 13:15:14（或者'2016/01/01 13:15:14）。

如果没有主数据编码，需要在【主数据建模】的【主数据编码】节点配置编码，保证主数据的唯一性。

然后，必须在【主数据权限】节点下对该主数据设置“可写”权限。

![](/articles/mdm/5-/images/image44.png)
 
<p align="center">图：设置可写权限</p>
 


〖保存所选〗是保存所选择的已装载主数据；

〖保存全部〗是保存全部装载的主数据；

〖清除〗是清除前台装载的主数据。

## 装载日志

点击【主数据共享】→【装载日志】，进入装载日志操作页面。

![](/articles/mdm/5-/images/image45.png)
 
<p align="center">图：装载日志</p>
 

在此页面左侧选择查询项：

主数据：点击主数据右侧分类按钮，选择主数据。

物料主数据：点击主数据右侧分类按钮，选择物料主数据。

业务系统：点击业务系统右侧下拉按钮，选择业务系统。

操作状态：点击操作状态右侧下拉按钮，选择操作状态。

起始时间：点击起始时间右侧的日历按钮，选择起始时间。

结束时间：点击结束时间右侧的日历按钮，选择结束时间。

单击〖查询〗按钮进行查询。页面右侧显示查询结果。单击〖清除〗删除查询条件。

## 数据分发

包括数据分发、分发条件和分发日志。“数据分发”将主数据手动分发到业务系统，分发对象是主数据和物料主数据。

点击【主数据共享】→【数据分发】，进入主数据分发页面，在该页面左侧是条件选择框，右侧是分发和过滤的相应选择操作区域。

主数据分发支持多子表。

![](/articles/mdm/5-/images/image46.png)
 
<p align="center">图：主数据分发</p>
 

选择主数据，单击“业务系统”右侧的![](/articles/mdm/5-/images/image47.png)，弹出下拉窗口进行业务系统的定位选择。选择完成后，单击〖查询〗，显示查询结果。

➢ 分发

在分发前，需要配置绑定业务系统的主数据权限为“可读”。如果选择“可订阅”，则维护主数据时，将实时分发变更数据到业务系统。

选择主数据，单击〖分发〗按钮，完成主数据的分发，成功后提示分发成功。	

分发结果可以查看分发日志。

➢ 高级查询

选择主数据，单击〖高级查询〗，按钮，弹出下图窗口，单击〖选择〗选择需要过滤的选项，单击〖确定〗完成过滤。单击〖重置〗按钮重置查询条件。

![](/articles/mdm/5-/images/image48.png)
 
<p align="center">图：高级查询</p>
 

## 分发条件

点击【主数据共享】→【分发条件】，进入分发条件页面，在此可以进行分发条件的新增、修改、删除操作。

![](/articles/mdm/5-/images/image49.png)
 
<p align="center">图：分发条件</p>
 

➢ 新增

单击〖新增〗按钮新增分发条件，弹出新窗口，输入内容，*号为必填项，单击〖确定〗。

![](/articles/mdm/5-/images/image50.png)
 
<p align="center">图：新增分发条件</p>
 

➢ 修改和删除

选中一行，单击〖修改〗按钮或〖删除〗按钮，进行修改或删除。

## 分发日志

点击【主数据共享】→【分发日志】，进入分发日志页面，左边是日志条件查询选项，右边是查询结果。

![](/articles/mdm/5-/images/image51.png)
 
<p align="center">图：分发日志</p>
 

在此页面左侧选择查询项：

主数据：点击主数据右侧分类按钮，选择主数据。

物料主数据：点击主数据右侧分类按钮，选择物料主数据。

业务系统：点击业务系统右侧下拉按钮，选择业务系统。

操作状态：点击操作状态右侧下拉按钮，选择操作状态。

起始时间：点击起始时间右侧的日历按钮，选择起始时间。

结束时间：点击结束时间右侧的日历按钮，选择结束时间。

单击〖查询〗按钮进行查询。单击〖清除〗删除查询条件。

## 血缘关系

点击【主数据共享】→【血缘关系】，进入血缘关系页面。页面左侧是主数据的分类树，右边是相应的主数据列表及血缘关系列表。

![](/articles/mdm/5-/images/image52.png)
 
<p align="center">图：血缘关系</p>
 

单击主数据列表的一条主数据，血缘关系列表会列出当前主数据的所有记录，便于对当前主数据的历史操作进行追踪。

➢ 主数据条件查询

输入编码，单击〖查询〗，可查询出相应的主数据。

➢ 高级查询

单击〖高级查询〗，左侧是待选条件，单击〖选择〗按钮进行选择，然后单击〖确定〗进行查询。单击〖重置〗按钮重置查询条件。
