# 元数据服务接口说明 #
##一、 调用方式 ##
  元数据服务通过com.yonyou.metadata.spi.service.ServiceFinder.findMetadataService(Class<E> clazz )调用各项服务接口。输入参数是各项服务的class对象。支持的服务类型有（均位于：com.yonyou.metadata.spi.service包下）：

	接口名称        					功能说明 
	MetadataService               元数据相关查询服务
	ExportMetaDataSqlService      导出元数据脚本服务
	MetadataPublishService        元数据发布服务
	MetadataSQLGenService         导出建表语句服务

一个参考示例如下：
	
     MetadataPublishService mps = ServiceFinder.findMetadataService(MetadataPublishService.class);
      mps.publishMetaData(data);

## 二、 元数据查询服务##

<h2 title="接口 MetadataService" class="title">接口 MetadataService</h2>
<!-- ========== METHOD SUMMARY =========== -->
<h3>方法概要</h3>
<table class="memberSummary" border="0" cellpadding="3" cellspacing="0" summary="方法概要表, 列表方法和解释">
<tr>
<th class="colFirst" scope="col">限定符和类型</th>
<th class="colLast" scope="col">方法和说明</th>
</tr>
<tr id="i0" class="altColor">
<td class="colFirst"><code>void</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#addMetadataChangeListener-com.yonyou.metadata.spi.event.MetadataChangeListener-">addMetadataChangeListener</a></span>(<a href="../../../../../com/yonyou/metadata/spi/event/MetadataChangeListener.html" title="com.yonyou.metadata.spi.event中的接口">MetadataChangeListener</a>&nbsp;l)</code>&nbsp;</td>
</tr>
<tr id="i1" class="rowColor">
<td class="colFirst"><code>java.util.Map&lt;java.lang.String,<a href="../../../../../com/yonyou/metadata/spi/Component.html" title="com.yonyou.metadata.spi中的类">Component</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#batchGetComponent-java.util.List-java.lang.String-">batchGetComponent</a></span>(java.util.List&lt;java.lang.String&gt;&nbsp;componentIDList,
                 java.lang.String&nbsp;tenantID)</code>&nbsp;</td>
</tr>
<tr id="i2" class="altColor">
<td class="colFirst"><code>java.util.Map&lt;java.lang.String,java.lang.Integer&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#batchGetCompVersion-java.util.List-java.lang.String-">batchGetCompVersion</a></span>(java.util.List&lt;java.lang.String&gt;&nbsp;componentIDList,
                   java.lang.String&nbsp;tenantID)</code>&nbsp;</td>
</tr>
<tr id="i3" class="rowColor">
<td class="colFirst"><code>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#batchGetEntities-java.util.List-java.lang.String-">batchGetEntities</a></span>(java.util.List&lt;java.lang.String&gt;&nbsp;entityIDList,
                java.lang.String&nbsp;tenantID)</code>
<div class="block"><span class="deprecatedLabel">已过时。</span>&nbsp;</div>
</td>
</tr>
<tr id="i4" class="altColor">
<td class="colFirst"><code>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Enumerate.html" title="com.yonyou.metadata.spi中的类">Enumerate</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#batchGetEnumerates-java.util.List-java.lang.String-">batchGetEnumerates</a></span>(java.util.List&lt;java.lang.String&gt;&nbsp;enumerateIDList,
                  java.lang.String&nbsp;tenantID)</code>
<div class="block"><span class="deprecatedLabel">已过时。</span>&nbsp;</div>
</td>
</tr>
<tr id="i5" class="rowColor">
<td class="colFirst"><code>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Component.html" title="com.yonyou.metadata.spi中的类">Component</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getAllComponents-java.lang.String-java.lang.String-">getAllComponents</a></span>(java.lang.String&nbsp;modulename,
                java.lang.String&nbsp;tenantID)</code>
<div class="block">根据模块名查询该模块里所有的组件</div>
</td>
</tr>
<tr id="i6" class="altColor">
<td class="colFirst"><code>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getAllEntity-java.lang.String-">getAllEntity</a></span>(java.lang.String&nbsp;tenantID)</code>
<div class="block"><span class="deprecatedLabel">已过时。</span>&nbsp;</div>
</td>
</tr>
<tr id="i7" class="rowColor">
<td class="colFirst"><code>java.util.Map&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>,<a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getAllREFEntity-java.lang.String-java.lang.String-java.lang.String-">getAllREFEntity</a></span>(java.lang.String&nbsp;namespace,
               java.lang.String&nbsp;entityName,
               java.lang.String&nbsp;tenantID)</code>
<div class="block">查询所有的REF该（entityName）的所有的Entity的方法，key是REF该Entity的Attribute,value是Attribute的Entity</div>
</td>
</tr>
<tr id="i8" class="altColor">
<td class="colFirst"><code>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getAttributeByClassID-java.lang.String-java.lang.String-">getAttributeByClassID</a></span>(java.lang.String&nbsp;classid,
                     java.lang.String&nbsp;tenantID)</code>
<div class="block">通过全限定名（完整类名）查询其所有的属性方法，</div>
</td>
</tr>
<tr id="i9" class="rowColor">
<td class="colFirst"><code>java.util.Map&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>,<a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getAttributeEntityByFK-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">getAttributeEntityByFK</a></span>(java.lang.String&nbsp;namespace,
                      java.lang.String&nbsp;name,
                      java.lang.String&nbsp;fkName,
                      java.lang.String&nbsp;tenantID)</code>
<div class="block">通过子表的类名和外键名查询其外键Attribute 及主表Entity</div>
</td>
</tr>
<tr id="i10" class="altColor">
<td class="colFirst"><code>java.util.List&lt;java.lang.String&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getBizInterfaceNames-java.lang.String-java.lang.String-">getBizInterfaceNames</a></span>(java.lang.String&nbsp;entityID,
                    java.lang.String&nbsp;tenantID)</code>
<div class="block">查询某实体实现的所有接口</div>
</td>
</tr>
<tr id="i11" class="rowColor">
<td class="colFirst"><code>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/BusiItAttr.html" title="com.yonyou.metadata.spi中的类">BusiItAttr</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getBusiItfAttrs-java.lang.String-java.lang.String-java.lang.String-">getBusiItfAttrs</a></span>(java.lang.String&nbsp;namespace,
               java.lang.String&nbsp;name,
               java.lang.String&nbsp;tenantID)</code>&nbsp;</td>
</tr>
<tr id="i12" class="altColor">
<td class="colFirst"><code>java.util.Map&lt;java.lang.String,java.util.Map&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>,<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>&gt;&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getChildTableNameAndImpAtts-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">getChildTableNameAndImpAtts</a></span>(java.lang.String&nbsp;namespace,
                           java.lang.String&nbsp;name,
                           java.lang.String&nbsp;fkName,
                           java.lang.String&nbsp;tenantID)</code>
<div class="block"><span class="deprecatedLabel">已过时。</span>&nbsp;</div>
</td>
</tr>
<tr id="i13" class="rowColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/Component.html" title="com.yonyou.metadata.spi中的类">Component</a></code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getComponentByID-java.lang.String-java.lang.String-">getComponentByID</a></span>(java.lang.String&nbsp;componentID,
                java.lang.String&nbsp;tenantID)</code>
<div class="block">根据组件id 获取组件</div>
</td>
</tr>
<tr id="i14" class="altColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/Component.html" title="com.yonyou.metadata.spi中的类">Component</a></code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getComponentByName-java.lang.String-java.lang.String-">getComponentByName</a></span>(java.lang.String&nbsp;componentname,
                  java.lang.String&nbsp;tenantID)</code>
<div class="block">根据组件名称获取组件</div>
</td>
</tr>
<tr id="i15" class="rowColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/Component.html" title="com.yonyou.metadata.spi中的类">Component</a></code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getComponentByTypeName-java.lang.String-java.lang.String-">getComponentByTypeName</a></span>(java.lang.String&nbsp;name,
                      java.lang.String&nbsp;tenantID)</code>
<div class="block">通过实体Name得到组件</div>
</td>
</tr>
<tr id="i16" class="altColor">
<td class="colFirst"><code>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Component.html" title="com.yonyou.metadata.spi中的类">Component</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getComponents-java.lang.String-">getComponents</a></span>(java.lang.String&nbsp;tenantID)</code>
<div class="block">获取该tenantID下的所有组件</div>
</td>
</tr>
<tr id="i17" class="rowColor">
<td class="colFirst"><code>java.lang.String</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getDDL-java.lang.String-java.lang.String-">getDDL</a></span>(java.lang.String&nbsp;tableName,
      java.lang.String&nbsp;tenantID)</code>&nbsp;</td>
</tr>
<tr id="i18" class="altColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>[]</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEntities-java.lang.String:A-java.lang.String-">getEntities</a></span>(java.lang.String[]&nbsp;entityIDs,
           java.lang.String&nbsp;tenantID)</code>&nbsp;</td>
</tr>
<tr id="i19" class="rowColor">
<td class="colFirst"><code>java.util.Map&lt;java.lang.String,java.util.List&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEntitiesAEnumerateByCmpID-java.lang.String-java.lang.String-">getEntitiesAEnumerateByCmpID</a></span>(java.lang.String&nbsp;componentID,
                            java.lang.String&nbsp;tenantID)</code>
<div class="block">通过组件的ID获取其所有的Entities</div>
</td>
</tr>
<tr id="i20" class="altColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a></code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEntity-java.lang.String-java.lang.String-java.lang.String-">getEntity</a></span>(java.lang.String&nbsp;namespace,
         java.lang.String&nbsp;entityName,
         java.lang.String&nbsp;tenantID)</code>
<div class="block">根据组件名和Entity名查询相应的Entity</div>
</td>
</tr>
<tr id="i21" class="rowColor">
<td class="colFirst"><code>java.util.Map&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>,<a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEntityAndFK-java.lang.String-java.lang.String-java.lang.String-">getEntityAndFK</a></span>(java.lang.String&nbsp;namespace,
              java.lang.String&nbsp;entityClassName,
              java.lang.String&nbsp;tenantID)</code>
<div class="block">通过主表查子表，key是外键</div>
</td>
</tr>
<tr id="i22" class="altColor">
<td class="colFirst"><code>java.util.Map&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>,<a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEntityByFK-java.lang.String-java.lang.String-java.lang.String-">getEntityByFK</a></span>(java.lang.String&nbsp;namespace,
             java.lang.String&nbsp;name,
             java.lang.String&nbsp;tenantID)</code>
<div class="block">通过子表的全限定名查主表(一个BMF文件只能有一个主表,所以不需要子表的外键名) 返回外键和主表Entity</div>
</td>
</tr>
<tr id="i23" class="rowColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a></code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEntityByFK-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">getEntityByFK</a></span>(java.lang.String&nbsp;namespace,
             java.lang.String&nbsp;entityName,
             java.lang.String&nbsp;fkName,
             java.lang.String&nbsp;tenantID)</code>
<div class="block">通过表的全限定名和属性（外键）名获取其对应的实体并且在其property文件中保存以“QueryConst.QUERY_ABSTRACTELEMENT_PROPERTY_KEY_PKEY”
 为key， 以实体主属性为value的Entity</div>
</td>
</tr>
<tr id="i24" class="altColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a></code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEntityByFullName-java.lang.String-java.lang.String-">getEntityByFullName</a></span>(java.lang.String&nbsp;entityFullName,
                   java.lang.String&nbsp;tenantID)</code>
<div class="block">根据Entity全名查询相应的Entity</div>
</td>
</tr>
<tr id="i25" class="rowColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a></code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEntityByID-java.lang.String-java.lang.String-">getEntityByID</a></span>(java.lang.String&nbsp;id,
             java.lang.String&nbsp;tenantID)</code>
<div class="block">通过ID查询Entity对方法</div>
</td>
</tr>
<tr id="i26" class="altColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a></code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEntityByName-java.lang.String-java.lang.String-java.lang.String-">getEntityByName</a></span>(java.lang.String&nbsp;namespace,
               java.lang.String&nbsp;entityClassName,
               java.lang.String&nbsp;tenantID)</code>
<div class="block">根据Entity的Name查询相应的Entity</div>
</td>
</tr>
<tr id="i27" class="rowColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/Enumerate.html" title="com.yonyou.metadata.spi中的类">Enumerate</a></code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEnumerate-java.lang.String-java.lang.String-java.lang.String-">getEnumerate</a></span>(java.lang.String&nbsp;namespace,
            java.lang.String&nbsp;name,
            java.lang.String&nbsp;tenantID)</code>
<div class="block">通过“命名空间”、枚举的全限定名，获取其所有的枚举对象。</div>
</td>
</tr>
<tr id="i28" class="altColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/Enumerate.html" title="com.yonyou.metadata.spi中的类">Enumerate</a>[]</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEnumerates-java.lang.String:A-java.lang.String-">getEnumerates</a></span>(java.lang.String[]&nbsp;enumerateIDs,
             java.lang.String&nbsp;tenantID)</code>&nbsp;</td>
</tr>
<tr id="i29" class="rowColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/EnumItem.html" title="com.yonyou.metadata.spi中的类">EnumItem</a></code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEnumItemByName-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">getEnumItemByName</a></span>(java.lang.String&nbsp;namespace,
                 java.lang.String&nbsp;enumeratename,
                 java.lang.String&nbsp;name,
                 java.lang.String&nbsp;tenantID)</code>
<div class="block">通过“命名空间”、枚举的类名、枚举值的名，获取其所对应的枚举值对象。</div>
</td>
</tr>
<tr id="i30" class="altColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/EnumItem.html" title="com.yonyou.metadata.spi中的类">EnumItem</a></code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEnumItemByValue-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">getEnumItemByValue</a></span>(java.lang.String&nbsp;namespace,
                  java.lang.String&nbsp;name,
                  java.lang.String&nbsp;value,
                  java.lang.String&nbsp;tenantID)</code>
<div class="block">通过“命名空间”、枚举名、枚举值的value，获取其所对应的枚举值对象。</div>
</td>
</tr>
<tr id="i31" class="rowColor">
<td class="colFirst"><code>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/EnumItem.html" title="com.yonyou.metadata.spi中的类">EnumItem</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEnumItems-java.lang.String-java.lang.String-java.lang.String-">getEnumItems</a></span>(java.lang.String&nbsp;namespace,
            java.lang.String&nbsp;name,
            java.lang.String&nbsp;tenantID)</code>
<div class="block">通过“命名空间”、枚举的全限定名，获取其所有的枚举值对象。</div>
</td>
</tr>
<tr id="i32" class="altColor">
<td class="colFirst"><code>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/EnumItem.html" title="com.yonyou.metadata.spi中的类">EnumItem</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEnumItemsByCompID-java.lang.String-java.lang.String-java.lang.String-">getEnumItemsByCompID</a></span>(java.lang.String&nbsp;componentID,
                    java.lang.String&nbsp;name,
                    java.lang.String&nbsp;tenantID)</code>
<div class="block">此方法用于EnumitemList内部用于实现getEnumList的方法（由于从Enumerate中只能够获取componentID属性）</div>
</td>
</tr>
<tr id="i33" class="rowColor">
<td class="colFirst"><code>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/EnumItem.html" title="com.yonyou.metadata.spi中的类">EnumItem</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEnumItemsByID-java.lang.String-java.lang.String-">getEnumItemsByID</a></span>(java.lang.String&nbsp;enumerateid,
                java.lang.String&nbsp;tenantID)</code>&nbsp;</td>
</tr>
<tr id="i34" class="altColor">
<td class="colFirst"><code>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/EnumItem.html" title="com.yonyou.metadata.spi中的类">EnumItem</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEnumItemsByValues-java.lang.String-java.lang.String-java.lang.String-java.lang.String...-">getEnumItemsByValues</a></span>(java.lang.String&nbsp;namespace,
                    java.lang.String&nbsp;name,
                    java.lang.String&nbsp;tenantID,
                    java.lang.String...&nbsp;values)</code>
<div class="block">通过“命名空间”、枚举的全限定名、多个枚举值的value，获取其所对应的枚举值对象。</div>
</td>
</tr>
<tr id="i35" class="rowColor">
<td class="colFirst"><code>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getExtendAttribute-java.lang.String-java.lang.String-java.lang.String-">getExtendAttribute</a></span>(java.lang.String&nbsp;namespace,
                  java.lang.String&nbsp;name,
                  java.lang.String&nbsp;tenantID)</code>
<div class="block">通过类名查询其所有的扩展属性方法</div>
</td>
</tr>
<tr id="i36" class="altColor">
<td class="colFirst"><code>int</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getExtendAttributeCount-java.lang.String-java.lang.String-">getExtendAttributeCount</a></span>(java.lang.String&nbsp;name,
                       java.lang.String&nbsp;tenantID)</code>
<div class="block">通过类名查询其所有的扩展属性方法的数量，</div>
</td>
</tr>
<tr id="i37" class="rowColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a></code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getImplementsAttribute-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">getImplementsAttribute</a></span>(java.lang.String&nbsp;interfaceName,
                      java.lang.String&nbsp;interfaceAttibuteName,
                      java.lang.String&nbsp;name,
                      java.lang.String&nbsp;tenantID)</code>
<div class="block">通过业务接口的全限定名和业务接口的属性名以及实现类的全限定名取得实现类的属性名。</div>
</td>
</tr>
<tr id="i38" class="altColor">
<td class="colFirst"><code>java.util.Map&lt;java.lang.String,java.util.Map&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>,<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>&gt;&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getImplementsAttributes-java.lang.String-java.lang.String-java.lang.String-">getImplementsAttributes</a></span>(java.lang.String&nbsp;namespace,
                       java.lang.String&nbsp;name,
                       java.lang.String&nbsp;tenantID)</code>
<div class="block">通过实现类的全限定名获取其所实现的所有业务接口在本类中对应的属性列表。</div>
</td>
</tr>
<tr id="i39" class="rowColor">
<td class="colFirst"><code>java.util.Map&lt;java.lang.String,java.lang.Object&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getImplementsAttributesByFK-java.lang.String-java.lang.String-java.lang.String-">getImplementsAttributesByFK</a></span>(java.lang.String&nbsp;namespace,
                           java.lang.String&nbsp;name,
                           java.lang.String&nbsp;tenantID)</code>
<div class="block"><span class="deprecatedLabel">已过时。</span>&nbsp;</div>
</td>
</tr>
<tr id="i40" class="altColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/event/MetadataChangeListener.html" title="com.yonyou.metadata.spi.event中的接口">MetadataChangeListener</a>[]</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getMetadataChangeListeners--">getMetadataChangeListeners</a></span>()</code>&nbsp;</td>
</tr>
<tr id="i41" class="rowColor">
<td class="colFirst"><code>java.lang.String</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getNameSpaceByCompID-java.lang.String-java.lang.String-">getNameSpaceByCompID</a></span>(java.lang.String&nbsp;id,
                    java.lang.String&nbsp;tenantID)</code>&nbsp;</td>
</tr>
<tr id="i42" class="altColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/Relation.html" title="com.yonyou.metadata.spi中的类">Relation</a></code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getRelation-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">getRelation</a></span>(java.lang.String&nbsp;namespace,
           java.lang.String&nbsp;startName,
           java.lang.String&nbsp;endName,
           java.lang.String&nbsp;tenantID)</code>
<div class="block">通过两个实体的Name，查询其关系，针对主子关系，startName是子实体名，endName是主实体名，针对REF关系，startName是主实体名，endName是子实体名。</div>
</td>
</tr>
<tr id="i43" class="rowColor">
<td class="colFirst"><code><a href="../../../../../com/yonyou/metadata/spi/Relation.html" title="com.yonyou.metadata.spi中的类">Relation</a></code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getRelationComponetId-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">getRelationComponetId</a></span>(java.lang.String&nbsp;componentID,
                     java.lang.String&nbsp;startName,
                     java.lang.String&nbsp;endName,
                     java.lang.String&nbsp;tenantID)</code>&nbsp;</td>
</tr>
<tr id="i44" class="altColor">
<td class="colFirst"><code>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Relation.html" title="com.yonyou.metadata.spi中的类">Relation</a>&gt;</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getRelations-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">getRelations</a></span>(java.lang.String&nbsp;namespace,
            java.lang.String&nbsp;startName,
            java.lang.String&nbsp;endName,
            java.lang.String&nbsp;tenantID)</code>
<div class="block">通过两个实体的Name，查询其关系，针对主子关系，startName是子实体名，endName是主实体名，针对REF关系，startName是主实体名，endName是子实体名。</div>
</td>
</tr>
<tr id="i45" class="rowColor">
<td class="colFirst"><code>boolean</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#isImplementBizInterface-java.lang.String-java.lang.String-java.lang.String-">isImplementBizInterface</a></span>(java.lang.String&nbsp;entityID,
                       java.lang.String&nbsp;interfacename,
                       java.lang.String&nbsp;tenantID)</code>
<div class="block">查询entityID对应的实体是否实现的itfName对应的业务接口</div>
</td>
</tr>
<tr id="i46" class="altColor">
<td class="colFirst"><code>void</code></td>
<td class="colLast"><code><span class="memberNameLink"><a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#removeMetadataChangeListener-com.yonyou.metadata.spi.event.MetadataChangeListener-">removeMetadataChangeListener</a></span>(<a href="../../../../../com/yonyou/metadata/spi/event/MetadataChangeListener.html" title="com.yonyou.metadata.spi.event中的接口">MetadataChangeListener</a>&nbsp;l)</code>&nbsp;</td>
</tr>
</table>
</li>
</ul>
</li>
</ul>
</div>
<div class="details">
<ul class="blockList">
<li class="blockList">
<!-- ============ METHOD DETAIL ========== -->
<ul class="blockList">
<li class="blockList"><a name="method.detail">
<!--   -->
</a>
<h3>方法详细资料</h3>
<a name="getComponentByTypeName-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getComponentByTypeName</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/Component.html" title="com.yonyou.metadata.spi中的类">Component</a>&nbsp;getComponentByTypeName(java.lang.String&nbsp;name,
                                 java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过实体Name得到组件</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>name</code> - 全限定名（完整类名），元数据支持本参数为空的查询，但具体业务场景下全限定名肯定不为空，故此处不可以为空！</dd>
<dt><span class="returnLabel">返回:</span></dt>
<dd>返回通过全限定名获取到的组件</dd>
</dl>
</li>
</ul>
<a name="getComponentByID-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getComponentByID</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/Component.html" title="com.yonyou.metadata.spi中的类">Component</a>&nbsp;getComponentByID(java.lang.String&nbsp;componentID,
                           java.lang.String&nbsp;tenantID)</pre>
<div class="block">根据组件id 获取组件</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>componentID</code> - 组件ID，元数据支持本参数为空的查询，但具体业务场景下全限定名肯定不为空，故此处不可以为空！</dd>
<dt><span class="returnLabel">返回:</span></dt>
<dd>返回通过组件ID获取到的组件</dd>
</dl>
</li>
</ul>
<a name="getComponentByName-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getComponentByName</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/Component.html" title="com.yonyou.metadata.spi中的类">Component</a>&nbsp;getComponentByName(java.lang.String&nbsp;componentname,
                             java.lang.String&nbsp;tenantID)</pre>
<div class="block">根据组件名称获取组件</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>componentName</code> - 组件名，元数据支持本参数为空的查询，但具体业务场景下全限定名肯定不为空，故此处不可以为空！</dd>
<dt><span class="returnLabel">返回:</span></dt>
<dd>返回通过组件名获取到的组件</dd>
</dl>
</li>
</ul>
<a name="getComponents-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getComponents</h4>
<pre>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Component.html" title="com.yonyou.metadata.spi中的类">Component</a>&gt;&nbsp;getComponents(java.lang.String&nbsp;tenantID)</pre>
<div class="block">获取该tenantID下的所有组件</div>
<dl>
<dt><span class="returnLabel">返回:</span></dt>
<dd>获取到的所有组件</dd>
</dl>
</li>
</ul>
<a name="getAllComponents-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getAllComponents</h4>
<pre>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Component.html" title="com.yonyou.metadata.spi中的类">Component</a>&gt;&nbsp;getAllComponents(java.lang.String&nbsp;modulename,
                                           java.lang.String&nbsp;tenantID)</pre>
<div class="block">根据模块名查询该模块里所有的组件</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>moduleName</code> - 模块名，元数据支持本参数为空的查询，但具体业务场景下全限定名肯定不为空，故此处不可以为空！</dd>
<dt><span class="returnLabel">返回:</span></dt>
<dd>返回通过模块名获取到的所有组件</dd>
</dl>
</li>
</ul>
<a name="isImplementBizInterface-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>isImplementBizInterface</h4>
<pre>boolean&nbsp;isImplementBizInterface(java.lang.String&nbsp;entityID,
                                java.lang.String&nbsp;interfacename,
                                java.lang.String&nbsp;tenantID)</pre>
<div class="block">查询entityID对应的实体是否实现的itfName对应的业务接口</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>entityID</code> - </dd>
<dd><code>interfaceName</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
<dd>参数对应的业务接口数量</dd>
</dl>
</li>
</ul>
<a name="getBizInterfaceNames-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getBizInterfaceNames</h4>
<pre>java.util.List&lt;java.lang.String&gt;&nbsp;getBizInterfaceNames(java.lang.String&nbsp;entityID,
                                                      java.lang.String&nbsp;tenantID)</pre>
<div class="block">查询某实体实现的所有接口</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>entityID</code> - 实体ID</dd>
<dt><span class="returnLabel">返回:</span></dt>
<dd>所有的接口字符串</dd>
</dl>
</li>
</ul>
<a name="getAttributeByClassID-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getAttributeByClassID</h4>
<pre>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>&gt;&nbsp;getAttributeByClassID(java.lang.String&nbsp;classid,
                                                java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过全限定名（完整类名）查询其所有的属性方法，</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>name</code> - 全限定名（完整类名），元数据支持本参数为空的查询，但具体业务场景下全限定名肯定不为空，故此处不可以为空！</dd>
<dt><span class="returnLabel">返回:</span></dt>
<dd>返回通过全限定名获取到的属性列表。</dd>
</dl>
</li>
</ul>
<a name="getExtendAttributeCount-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getExtendAttributeCount</h4>
<pre>int&nbsp;getExtendAttributeCount(java.lang.String&nbsp;name,
                            java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过类名查询其所有的扩展属性方法的数量，</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>name</code> - 类名，元数据支持本参数为空的查询，但具体业务场景下类肯定不为空，故此处不可以为空！</dd>
<dt><span class="returnLabel">返回:</span></dt>
<dd>返回通过全限定名获取到的扩展属性数量。</dd>
</dl>
</li>
</ul>
<a name="getExtendAttribute-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getExtendAttribute</h4>
<pre>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>&gt;&nbsp;getExtendAttribute(java.lang.String&nbsp;namespace,
                                             java.lang.String&nbsp;name,
                                             java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过类名查询其所有的扩展属性方法</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>name</code> - 类名，元数据支持本参数为空的查询，但具体业务场景下类名肯定不为空，故此处不可以为空！</dd>
<dt><span class="returnLabel">返回:</span></dt>
<dd>返回通过全限定名获取到的扩展属性</dd>
</dl>
</li>
</ul>
<a name="getEntityByID-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEntityByID</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&nbsp;getEntityByID(java.lang.String&nbsp;id,
                     java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过ID查询Entity对方法</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>id</code> - Entity的ID</dd>
<dt><span class="returnLabel">返回:</span></dt>
<dd>返回通过ID查询获取到的Entity</dd>
</dl>
</li>
</ul>
<a name="getAllEntity-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getAllEntity</h4>
<pre>@Deprecated
java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&gt;&nbsp;getAllEntity(java.lang.String&nbsp;tenantID)</pre>
<div class="block"><span class="deprecatedLabel">已过时。</span>&nbsp;</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getEntityByName-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEntityByName</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&nbsp;getEntityByName(java.lang.String&nbsp;namespace,
                       java.lang.String&nbsp;entityClassName,
                       java.lang.String&nbsp;tenantID)</pre>
<div class="block">根据Entity的Name查询相应的Entity</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>entityClassName</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
<dt><span class="throwsLabel">抛出:</span></dt>
<dd><code><a href="../../../../../com/yonyou/metadata/spi/service/MetaDataException.html" title="com.yonyou.metadata.spi.service中的类">MetaDataException</a></code></dd>
</dl>
</li>
</ul>
<a name="getEntityByFullName-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEntityByFullName</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&nbsp;getEntityByFullName(java.lang.String&nbsp;entityFullName,
                           java.lang.String&nbsp;tenantID)</pre>
<div class="block">根据Entity全名查询相应的Entity</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>entityFullName</code> - 形如：nameSpace.EntityName</dd>
<dt><span class="returnLabel">返回:</span></dt>
<dt><span class="throwsLabel">抛出:</span></dt>
<dd><code><a href="../../../../../com/yonyou/metadata/spi/service/MetaDataException.html" title="com.yonyou.metadata.spi.service中的类">MetaDataException</a></code></dd>
</dl>
</li>
</ul>
<a name="getEntity-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEntity</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&nbsp;getEntity(java.lang.String&nbsp;namespace,
                 java.lang.String&nbsp;entityName,
                 java.lang.String&nbsp;tenantID)</pre>
<div class="block">根据组件名和Entity名查询相应的Entity</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>nameSpace</code> - </dd>
<dd><code>entityName</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
<dt><span class="throwsLabel">抛出:</span></dt>
<dd><code><a href="../../../../../com/yonyou/metadata/spi/service/MetaDataException.html" title="com.yonyou.metadata.spi.service中的类">MetaDataException</a></code></dd>
</dl>
</li>
</ul>
<a name="getEntityAndFK-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEntityAndFK</h4>
<pre>java.util.Map&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>,<a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&gt;&nbsp;getEntityAndFK(java.lang.String&nbsp;namespace,
                                               java.lang.String&nbsp;entityClassName,
                                               java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过主表查子表，key是外键</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>entityClassName</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getImplementsAttributes-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getImplementsAttributes</h4>
<pre>java.util.Map&lt;java.lang.String,java.util.Map&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>,<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>&gt;&gt;&nbsp;getImplementsAttributes(java.lang.String&nbsp;namespace,
                                                                                           java.lang.String&nbsp;name,
                                                                                           java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过实现类的全限定名获取其所实现的所有业务接口在本类中对应的属性列表。在严格的业务场景中建议使用public Attribute getImplementsAttribute（
 String，String，String,String)方法。 （！！！本方法不适用于一个实现类同时实现多个业务接口，并且不同业务接口中有相同字段名称的情况！！！）</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>name</code> - 实现类的全限定名</dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
<dd>key 是业务接口的的Name，value是 这个接口对应的属性列表</dd>
</dl>
</li>
</ul>
<a name="getImplementsAttribute-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getImplementsAttribute</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>&nbsp;getImplementsAttribute(java.lang.String&nbsp;interfaceName,
                                 java.lang.String&nbsp;interfaceAttibuteName,
                                 java.lang.String&nbsp;name,
                                 java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过业务接口的全限定名和业务接口的属性名以及实现类的全限定名取得实现类的属性名。</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>interfaceName</code> - 业务接口的全限定名</dd>
<dd><code>interfaceAttibuteName</code> - 业务接口属性的全限定名</dd>
<dd><code>name</code> - 实现类的全限定名</dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
<dd>实现类对应的属性名</dd>
</dl>
</li>
</ul>
<a name="getImplementsAttributesByFK-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getImplementsAttributesByFK</h4>
<pre>@Deprecated
java.util.Map&lt;java.lang.String,java.lang.Object&gt;&nbsp;getImplementsAttributesByFK(java.lang.String&nbsp;namespace,
                                                                                          java.lang.String&nbsp;name,
                                                                                          java.lang.String&nbsp;tenantID)</pre>
<div class="block"><span class="deprecatedLabel">已过时。</span>&nbsp;</div>
<div class="block">通过子表外键的全限定名获取其主表所实现的所有业务接口对应的属性列表，由于此接口承担了太多的需求，故0.0.2之后的版本不在提供对于此接口的支持。</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>name</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
<dd>key 是主表的tablename，value是 业务接口对应的属性列表，额外加一个key是"FOREIGN@KEY"，value是子表和主表的外键（Attribute）</dd>
</dl>
</li>
</ul>
<a name="getEntityByFK-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEntityByFK</h4>
<pre>java.util.Map&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>,<a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&gt;&nbsp;getEntityByFK(java.lang.String&nbsp;namespace,
                                              java.lang.String&nbsp;name,
                                              java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过子表的全限定名查主表(一个BMF文件只能有一个主表,所以不需要子表的外键名) 返回外键和主表Entity</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>name</code> - </dd>
<dd><code>fkName</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getBusiItfAttrs-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getBusiItfAttrs</h4>
<pre>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/BusiItAttr.html" title="com.yonyou.metadata.spi中的类">BusiItAttr</a>&gt;&nbsp;getBusiItfAttrs(java.lang.String&nbsp;namespace,
                                           java.lang.String&nbsp;name,
                                           java.lang.String&nbsp;tenantID)</pre>
</li>
</ul>
<a name="getChildTableNameAndImpAtts-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getChildTableNameAndImpAtts</h4>
<pre>@Deprecated
java.util.Map&lt;java.lang.String,java.util.Map&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>,<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>&gt;&gt;&nbsp;getChildTableNameAndImpAtts(java.lang.String&nbsp;namespace,
                                                                                                            java.lang.String&nbsp;name,
                                                                                                            java.lang.String&nbsp;fkName,
                                                                                                            java.lang.String&nbsp;tenantID)</pre>
<div class="block"><span class="deprecatedLabel">已过时。</span>&nbsp;</div>
<div class="block">由于在实际业务中外键保存在子表中，所以不会出现该业务场景。
 通过主表的全限定名和外键名称获取子表的表明（Key)及，子表所实现的接口的Attribute-----在子表中映射属性的key-value键值对</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>name</code> - </dd>
<dd><code>fkName</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getAttributeEntityByFK-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getAttributeEntityByFK</h4>
<pre>java.util.Map&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>,<a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&gt;&nbsp;getAttributeEntityByFK(java.lang.String&nbsp;namespace,
                                                       java.lang.String&nbsp;name,
                                                       java.lang.String&nbsp;fkName,
                                                       java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过子表的类名和外键名查询其外键Attribute 及主表Entity</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>name</code> - 子表的类名</dd>
<dd><code>fkName</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
<dd>key:外键Attribute，Value:主表Entity</dd>
</dl>
</li>
</ul>
<a name="getEnumerate-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEnumerate</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/Enumerate.html" title="com.yonyou.metadata.spi中的类">Enumerate</a>&nbsp;getEnumerate(java.lang.String&nbsp;namespace,
                       java.lang.String&nbsp;name,
                       java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过“命名空间”、枚举的全限定名，获取其所有的枚举对象。</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>namespace</code> - </dd>
<dd><code>name</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getEnumItemsByCompID-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEnumItemsByCompID</h4>
<pre>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/EnumItem.html" title="com.yonyou.metadata.spi中的类">EnumItem</a>&gt;&nbsp;getEnumItemsByCompID(java.lang.String&nbsp;componentID,
                                              java.lang.String&nbsp;name,
                                              java.lang.String&nbsp;tenantID)</pre>
<div class="block">此方法用于EnumitemList内部用于实现getEnumList的方法（由于从Enumerate中只能够获取componentID属性）</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>componentID</code> - </dd>
<dd><code>name</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getEnumItems-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEnumItems</h4>
<pre>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/EnumItem.html" title="com.yonyou.metadata.spi中的类">EnumItem</a>&gt;&nbsp;getEnumItems(java.lang.String&nbsp;namespace,
                                      java.lang.String&nbsp;name,
                                      java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过“命名空间”、枚举的全限定名，获取其所有的枚举值对象。</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>namespace</code> - </dd>
<dd><code>name</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getEnumItemByName-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEnumItemByName</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/EnumItem.html" title="com.yonyou.metadata.spi中的类">EnumItem</a>&nbsp;getEnumItemByName(java.lang.String&nbsp;namespace,
                           java.lang.String&nbsp;enumeratename,
                           java.lang.String&nbsp;name,
                           java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过“命名空间”、枚举的类名、枚举值的名，获取其所对应的枚举值对象。</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>namespace</code> - </dd>
<dd><code>name</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getEnumItemByValue-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEnumItemByValue</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/EnumItem.html" title="com.yonyou.metadata.spi中的类">EnumItem</a>&nbsp;getEnumItemByValue(java.lang.String&nbsp;namespace,
                            java.lang.String&nbsp;name,
                            java.lang.String&nbsp;value,
                            java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过“命名空间”、枚举名、枚举值的value，获取其所对应的枚举值对象。</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>namespace</code> - </dd>
<dd><code>value</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getEnumItemsByValues-java.lang.String-java.lang.String-java.lang.String-java.lang.String...-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEnumItemsByValues</h4>
<pre>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/EnumItem.html" title="com.yonyou.metadata.spi中的类">EnumItem</a>&gt;&nbsp;getEnumItemsByValues(java.lang.String&nbsp;namespace,
                                              java.lang.String&nbsp;name,
                                              java.lang.String&nbsp;tenantID,
                                              java.lang.String...&nbsp;values)</pre>
<div class="block">通过“命名空间”、枚举的全限定名、多个枚举值的value，获取其所对应的枚举值对象。</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>namespace</code> - </dd>
<dd><code>name</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dd><code>values</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getEntitiesAEnumerateByCmpID-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEntitiesAEnumerateByCmpID</h4>
<pre>java.util.Map&lt;java.lang.String,java.util.List&gt;&nbsp;getEntitiesAEnumerateByCmpID(java.lang.String&nbsp;componentID,
                                                                            java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过组件的ID获取其所有的Entities</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>componentID</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getEnumItemsByID-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEnumItemsByID</h4>
<pre>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/EnumItem.html" title="com.yonyou.metadata.spi中的类">EnumItem</a>&gt;&nbsp;getEnumItemsByID(java.lang.String&nbsp;enumerateid,
                                          java.lang.String&nbsp;tenantID)</pre>
</li>
</ul>
<a name="addMetadataChangeListener-com.yonyou.metadata.spi.event.MetadataChangeListener-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>addMetadataChangeListener</h4>
<pre>void&nbsp;addMetadataChangeListener(<a href="../../../../../com/yonyou/metadata/spi/event/MetadataChangeListener.html" title="com.yonyou.metadata.spi.event中的接口">MetadataChangeListener</a>&nbsp;l)</pre>
</li>
</ul>
<a name="removeMetadataChangeListener-com.yonyou.metadata.spi.event.MetadataChangeListener-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>removeMetadataChangeListener</h4>
<pre>void&nbsp;removeMetadataChangeListener(<a href="../../../../../com/yonyou/metadata/spi/event/MetadataChangeListener.html" title="com.yonyou.metadata.spi.event中的接口">MetadataChangeListener</a>&nbsp;l)</pre>
</li>
</ul>
<a name="getMetadataChangeListeners--">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getMetadataChangeListeners</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/event/MetadataChangeListener.html" title="com.yonyou.metadata.spi.event中的接口">MetadataChangeListener</a>[]&nbsp;getMetadataChangeListeners()</pre>
</li>
</ul>
<a name="getEntityByFK-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEntityByFK</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&nbsp;getEntityByFK(java.lang.String&nbsp;namespace,
                     java.lang.String&nbsp;entityName,
                     java.lang.String&nbsp;fkName,
                     java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过表的全限定名和属性（外键）名获取其对应的实体并且在其property文件中保存以“QueryConst.QUERY_ABSTRACTELEMENT_PROPERTY_KEY_PKEY”
 为key， 以实体主属性为value的Entity</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>namespace</code> - </dd>
<dd><code>name</code> - </dd>
<dd><code>fkName</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getRelation-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getRelation</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/Relation.html" title="com.yonyou.metadata.spi中的类">Relation</a>&nbsp;getRelation(java.lang.String&nbsp;namespace,
                     java.lang.String&nbsp;startName,
                     java.lang.String&nbsp;endName,
                     java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过两个实体的Name，查询其关系，针对主子关系，startName是子实体名，endName是主实体名，针对REF关系，startName是主实体名，endName是子实体名。
 此方法对于主子关系复合REF关系不适用（此场景下仅返回REF关系，请合理评估风险）。建议使用相同入参的getRelations方法。</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>namespace</code> - </dd>
<dd><code>startName</code> - </dd>
<dd><code>endName</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getRelations-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getRelations</h4>
<pre>java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Relation.html" title="com.yonyou.metadata.spi中的类">Relation</a>&gt;&nbsp;getRelations(java.lang.String&nbsp;namespace,
                                      java.lang.String&nbsp;startName,
                                      java.lang.String&nbsp;endName,
                                      java.lang.String&nbsp;tenantID)</pre>
<div class="block">通过两个实体的Name，查询其关系，针对主子关系，startName是子实体名，endName是主实体名，针对REF关系，startName是主实体名，endName是子实体名。</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>namespace</code> - </dd>
<dd><code>startName</code> - </dd>
<dd><code>endName</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getRelationComponetId-java.lang.String-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getRelationComponetId</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/Relation.html" title="com.yonyou.metadata.spi中的类">Relation</a>&nbsp;getRelationComponetId(java.lang.String&nbsp;componentID,
                               java.lang.String&nbsp;startName,
                               java.lang.String&nbsp;endName,
                               java.lang.String&nbsp;tenantID)</pre>
</li>
</ul>
<a name="batchGetEntities-java.util.List-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>batchGetEntities</h4>
<pre>@Deprecated
java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&gt;&nbsp;batchGetEntities(java.util.List&lt;java.lang.String&gt;&nbsp;entityIDList,
                                                     java.lang.String&nbsp;tenantID)</pre>
<div class="block"><span class="deprecatedLabel">已过时。</span>&nbsp;</div>
<div class="block">该方法已经废弃，请使用<a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEntities-java.lang.String:A-java.lang.String-"><code>getEntities(String[], String)</code></a></div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>entityIDList</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getEntities-java.lang.String:A-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEntities</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>[]&nbsp;getEntities(java.lang.String[]&nbsp;entityIDs,
                     java.lang.String&nbsp;tenantID)</pre>
</li>
</ul>
<a name="batchGetEnumerates-java.util.List-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>batchGetEnumerates</h4>
<pre>@Deprecated
java.util.List&lt;<a href="../../../../../com/yonyou/metadata/spi/Enumerate.html" title="com.yonyou.metadata.spi中的类">Enumerate</a>&gt;&nbsp;batchGetEnumerates(java.util.List&lt;java.lang.String&gt;&nbsp;enumerateIDList,
                                                          java.lang.String&nbsp;tenantID)</pre>
<div class="block"><span class="deprecatedLabel">已过时。</span>&nbsp;</div>
<div class="block">该方法已经废弃，请使用<a href="../../../../../com/yonyou/metadata/spi/service/MetadataService.html#getEnumerates-java.lang.String:A-java.lang.String-"><code>getEnumerates(String[], String)</code></a></div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>enumerateIDList</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getEnumerates-java.lang.String:A-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getEnumerates</h4>
<pre><a href="../../../../../com/yonyou/metadata/spi/Enumerate.html" title="com.yonyou.metadata.spi中的类">Enumerate</a>[]&nbsp;getEnumerates(java.lang.String[]&nbsp;enumerateIDs,
                          java.lang.String&nbsp;tenantID)</pre>
</li>
</ul>
<a name="batchGetCompVersion-java.util.List-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>batchGetCompVersion</h4>
<pre>java.util.Map&lt;java.lang.String,java.lang.Integer&gt;&nbsp;batchGetCompVersion(java.util.List&lt;java.lang.String&gt;&nbsp;componentIDList,
                                                                      java.lang.String&nbsp;tenantID)</pre>
</li>
</ul>
<a name="batchGetComponent-java.util.List-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>batchGetComponent</h4>
<pre>java.util.Map&lt;java.lang.String,<a href="../../../../../com/yonyou/metadata/spi/Component.html" title="com.yonyou.metadata.spi中的类">Component</a>&gt;&nbsp;batchGetComponent(java.util.List&lt;java.lang.String&gt;&nbsp;componentIDList,
                                                            java.lang.String&nbsp;tenantID)</pre>
</li>
</ul>
<a name="getNameSpaceByCompID-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getNameSpaceByCompID</h4>
<pre>java.lang.String&nbsp;getNameSpaceByCompID(java.lang.String&nbsp;id,
                                      java.lang.String&nbsp;tenantID)</pre>
</li>
</ul>
<a name="getAllREFEntity-java.lang.String-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockList">
<li class="blockList">
<h4>getAllREFEntity</h4>
<pre>java.util.Map&lt;<a href="../../../../../com/yonyou/metadata/spi/Attribute.html" title="com.yonyou.metadata.spi中的类">Attribute</a>,<a href="../../../../../com/yonyou/metadata/spi/Entity.html" title="com.yonyou.metadata.spi中的类">Entity</a>&gt;&nbsp;getAllREFEntity(java.lang.String&nbsp;namespace,
                                                java.lang.String&nbsp;entityName,
                                                java.lang.String&nbsp;tenantID)</pre>
<div class="block">查询所有的REF该（entityName）的所有的Entity的方法，key是REF该Entity的Attribute,value是Attribute的Entity</div>
<dl>
<dt><span class="paramLabel">参数:</span></dt>
<dd><code>namespace</code> - </dd>
<dd><code>entityName</code> - </dd>
<dd><code>tenantID</code> - </dd>
<dt><span class="returnLabel">返回:</span></dt>
</dl>
</li>
</ul>
<a name="getDDL-java.lang.String-java.lang.String-">
<!--   -->
</a>
<ul class="blockListLast">
<li class="blockList">
<h4>getDDL</h4>
<pre>java.lang.String&nbsp;getDDL(java.lang.String&nbsp;tableName,
                        java.lang.String&nbsp;tenantID)</pre>
</li>
</ul>
</li>
</ul>
</li>
</ul>
</div>
</div>
<div class="bottomNav"><a name="navbar.bottom">
<!--   -->
</a>
</body>
</html>
