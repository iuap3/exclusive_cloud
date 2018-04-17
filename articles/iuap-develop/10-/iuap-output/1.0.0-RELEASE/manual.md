# 打印 #

## 功能简介 ##

组件主要提供打印功能。  
可以分为三种方式：  

* 界面直接打印（即调用js函数完成指定区域的打印）  

* PDF方式打印（即通过访问后台接口，根据业务数据和模板生成pdf文件供用户下载）	  

* HTML模板方式打印（即通过定义html的模板通过后台接口得到对应的json对象，将json数据导入到html模板中，形成打印界面）  


## 工作流程 ##

1. 界面直接打印时，调用相应的js函数，直接传入打印区域的ID或样式，即可打印指定的区域    

2. PDF方式打印时，首先设计好相应的模板文件，实现API中的接口  `com.yonyou.uap.ieop.print.template.PDFPrintTemplate`  

3. HTML模板方式打印时，设置html模板页面，实现接口  `com.yonyou.uap.ieop.print.template.HTMLPrintTemplate`，在web.xml配置  `com.yonyou.uap.ieop.print.controller.PrintController`的声明和必要初始化参数配置，发送Http请求到指定url，完成pdf的输出或html界面的打印。

## API接口 ##

### 模板接口 ###

**描述**  
完成pdf打印输出功能  
**请求方法**  
`/ PrintController `  // 可在web.xml中自定义地址  
**请求方式**  
`HTTP POST/GET `   
**请求参数说明**  
请求参数type, 可选值有pdf和html，其他参数根据业务需求而定，用户可在具体的模板实现类中获取请求中的参数，实现`HttpRequetEventAwear`即可  
**接口方法**  
`com.yonyou.uap.ieop.print.template.PDFPrintTemplate`  

* public String getTemplateName();  
**返回参数说明**
<table>
  <tr>
    <th><br>  参数字段<br>  </th>
    <th><br>  必选<br>  </th>
    <th><br>  类型<br>  </th>
    <th><br>  长度限制<br>  </th>
    <th><br>  说明<br>  </th>
  </tr>
  <tr>
    <td><br>  无<br>  </td>
    <td><br>   <br>  </td>
    <td><br>  String<br>  </td>
    <td><br>   <br>  </td>
    <td><br>  模板路径,以“/”开头<br>  </td>
  </tr>
</table>  

* `public String getDownloadName();` //打印下载时文件的名称  

**返回参数说明**

<table>
  <tr>
    <th><br>  参数字段<br>  </th>
    <th><br>  必选<br>  </th>
    <th><br>  类型<br>  </th>
    <th><br>  长度限制<br>  </th>
    <th><br>  说明<br>  </th>
  </tr>
  <tr>
    <td><br>  无<br>  </td>
    <td><br>   <br>  </td>
    <td><br>  String<br>  </td>
    <td><br>   <br>  </td>
    <td><br>  Pdf下载时文件的默认名字<br>  </td>
  </tr>
</table>  

* `public ITemplateDataAccessor getTemplateDataAccessor();`  
**返回参数说明**  

<table>
  <tr>
    <th><br>  参数字段<br>  </th>
    <th><br>  必选<br>  </th>
    <th><br>  类型<br>  </th>
    <th><br>  长度限制<br>  </th>
    <th><br>  说明<br>  </th>
  </tr>
  <tr>
    <td><br>  无<br>  </td>
    <td><br>   <br>  </td>
    <td><br>  ITemplateDataAccessor<br>  </td>
    <td><br>   <br>  </td>
    <td><br>  获取数据访问器<br>  </td>
  </tr>
</table>  

* `public List<IDataSource> getDataSourceList();`  
**返回参数说明**  

<table>
  <tr>
    <th><br>  参数字段<br>  </th>
    <th><br>  必选<br>  </th>
    <th><br>  类型<br>  </th>
    <th><br>  长度限制<br>  </th>
    <th><br>  说明<br>  </th>
  </tr>
  <tr>
    <td><br>  无<br>  </td>
    <td><br>   <br>  </td>
    <td><br>  List&lt;IDataSource&gt;<br>  </td>
    <td><br>   <br>  </td>
    <td><br>  获取打印数据源<br>  </td>
  </tr>
</table>  

**接口方法**  
`com.yonyou.uap.ieop.print.template.HTMLPrintTemplate`

* `public String getTemplateName();`  
**返回参数说明**  

<table>
  <tr>
    <th><br>  参数字段<br>  </th>
    <th><br>  必选<br>  </th>
    <th><br>  类型<br>  </th>
    <th><br>  长度限制<br>  </th>
    <th><br>  说明<br>  </th>
  </tr>
  <tr>
    <td><br>  无<br>  </td>
    <td><br>   <br>  </td>
    <td><br>  String<br>  </td>
    <td><br>   <br>  </td>
    <td><br>  模板路径,以“/”开头<br>  </td>
  </tr>
</table>  

* `public String getDataSourceObject ();`//打印下载时文件的名称  
**返回参数说明**  

<table>
  <tr>
    <th><br>  参数字段<br>  </th>
    <th><br>  必选<br>  </th>
    <th><br>  类型<br>  </th>
    <th><br>  长度限制<br>  </th>
    <th><br>  说明<br>  </th>
  </tr>
  <tr>
    <td><br>  无<br>  </td>
    <td><br>   <br>  </td>
    <td><br>  Object<br>  </td>
    <td><br>   <br>  </td>
    <td><br>  Json数据对应的java对象<br>  </td>
  </tr>
</table>  

* `public String getPrintPage ();`  
**返回参数说明**  

<table>
  <tr>
    <th><br>  参数字段<br>  </th>
    <th><br>  必选<br>  </th>
    <th><br>  类型<br>  </th>
    <th><br>  长度限制<br>  </th>
    <th><br>  说明<br>  </th>
  </tr>
  <tr>
    <td><br>  无<br>  </td>
    <td><br>   <br>  </td>
    <td><br>  String<br>  </td>
    <td><br>   <br>  </td>
    <td><br>  模板处理的界面，以“/”开头<br>  </td>
  </tr>
</table>  


### 界面打印 ###

调用响应的js函数即可
Js函数说明：  

	printByElementId(e_id)；// 打印e_id元素的区域
	printByElementIds（e_ids）// 打印e_ids元素的区域，e_ids为一个数组
	printByElementCss（e_css）// 打印css样式为e_css元素区域
	printByElementCsses（e_csses）；// 打印css样式为e_csses元素区域，e_csses为数组  


## 开发步骤 ##

### 界面直接打印 ###

1. 前台页面引入组件*PrintModel.js*  

		<!-- 需要jQuery支持 -->
			<script type="text/javascript" src="resources/js/jquery-2.1.4.min.js"></script>
			<!-- 界面直接打印组件JS -->
			<script type="text/javascript" src="resources/js/PrintModel.js"></script>

2. HTML模板方式时需要实现函数getStyle(返回css样式定义字符串)、getTemplate(返回模板定义字符串)、getJsonData(返回模板需要的json对象)方法  

3. 调用相应的JS函数，传入打印区域的ID或样式，即可打印指定的区域  

### PDF模板方式打印 ###

1. 引入组件Jar包，
如果是maven工程，可以直接使用如下依赖：  

		<!-- 打印功能 -->
		<dependency>
			<groupId>com.yonyou.iuap</groupId>
    		<artifactId>iuap-output</artifactId>
    		<version>1.0.0-RELEASE</version>
		</dependency>  


2. 设计模板文件放置到相应目录下，实现接口PDFPrintTemplate，如果实现类中需要HttpServletReuqet和HttpServletResponse对象可实现HttpRequestEventAware和HttpResponseEventAware   
一个例子：  

		package com.yonyou.print.service;
		import java.util.ArrayList;
		import java.util.List;
		import javax.servlet.http.HttpServletRequest;
		import nc.ui.pub.print.DefaultDataSource;
		import nc.ui.pub.print.IDataSource;
		import com.yonyou.uap.ieop.print.aware.HttpRequestEventAware;
		import com.yonyou.uap.ieop.print.template.AbstractFileTemplate;
		
		public class PdfPrintTemplateImpl extends AbstractFileTemplate implements HttpRequestEventAware{
			private HttpServletRequest request;
	
		@Override
		public List<IDataSource> getDataSourceList() {
			List<IDataSource> list = new ArrayList<IDataSource>();
			IDataSource datasource = new DefaultDataSource();
			List<Object> objects = new ArrayList<Object>();
			OrderForm printData = getData();
			objects.add(printData);
			datasource.setObjects(objects);
			list.add(datasource);
			return list;
		}
	
		@Override
		public String getTemplateName() {
			// 读取配置文件
			return request.getServletContext().getRealPath("/templates/order.otf");
		}
		 
		public OrderForm getData() {
			// 通过订单号查询订单数据，这里直接写出了
			OrderForm orderForm = new OrderForm();
			orderForm.setOrderno("20150920001");
			orderForm.setOrderdate("2015-09-12 12:45:45");
			orderForm.setCustomername("刘XX");
			orderForm.setPhoneno("134XXXXXXXX");
			orderForm.setCustomeraddr("北京市海淀区北清路68号用友软件园");
			orderForm.setTotalamount(57.30);
	
			List<OrderItem> items = new ArrayList<OrderItem>();
			OrderItem item1 = new OrderItem();
			item1.setNo("000001");
			item1.setName("图解CSS3：核心技术与案例实战 ");
			item1.setCount(1);
			item1.setAmount(57.30);
			items.add(item1);
			orderForm.setItems(items);
			return orderForm;
		}
	
		@Override
		public void setHttpRequest(HttpServletRequest request) {
			this.request = request;
		}
		}	 


3. 在web.xml中配置PrintController和初始化参数，界面中通过ajax请求配置的url，完成响应功能。  

	<!-- 打印 -->
		<servlet>
			<description></description>
			<display-name>PrintController</display-name>
			<servlet-name>PrintController</servlet-name>
			<servlet-class>com.yonyou.uap.ieop.print.controller.PrintController</servlet-class>
			<init-param>
				<param-name>pdfPrintTemplate</param-name>
				<param-value>com.yonyou.print.service.PdfPrintTemplateImpl</param-value>
			</init-param>
		</servlet>
		<servlet-mapping>
			<servlet-name>PrintController</servlet-name>
			<url-pattern>/PrintController</url-pattern>
		</servlet-mapping>  



## 扩展机制 ##

用户可根据模板来源的不同，以多种方式实现PDFPrintTemplate接口，将实现类配置到web.xml中即可使用。也可通过PDFPrintEngine类和PDFPrintTemplate接口实现类完成pdf文件的生成










