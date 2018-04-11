# 消息推送 #
## 功能简介 ##
向手机发送短信、发送电子邮件、向APP推送消息。

## 工作流程 ##
1. 消息推送  
在公司现有的消息推送平台的基础上，利用此平台的REST服务，将业务数据包装成符合要求的格式，发送到平台上，完成APP的推送。（用户的移动端也需要安装平台提供的SDK）  
2. 发送短信  
发送短信的功能是基于公司的短信推送平台，通过此平台的提供的REST服务，按照平台的要求，将数据封装提交到平台，完成短信发送任务。目前支持国内的移动、联通和电信三大网络。  
3. 发送邮件  
电子邮件服务，是基于JavaMail实现的邮件发送服务，使用JDK原生的javax.mail组件来完成发送邮件的服务。  
主要工作流程如下：  
 1. 验证登录权限Authenticator  
 2. 根据设置的Properties和Authenticator创建一个Session  
 3. 创建一个MimeMessage实例，设置这个message的收信人、主题、内容等  
 4. 发送邮件Transport.send(message);  

## API接口 ##
**描述**  
短信、邮件和APP消息推送接口  

**请求方法**  
new MessageSend(msgReceivers, msgContent).send();  

**请求参数说明**  
<table>
  <tr>
    <th><br>  参数字段<br>  </th>
    <th><br>  必选<br>  </th>
    <th><br>  类型<br>  </th>
    <th><br>  长度<br>  </th>
    <th><br>  说明<br>  </th>
  </tr>
  <tr>
    <td><br>  msgReceivers<br>  </td>
    <td><br>  True<br>  </td>
    <td><br>  MessageReceiver<br>  </td>
    <td><br>  -<br>  </td>
    <td><br>  消息的接收者，有多个的时候，按照英文”,”分割<br>  </td>
  </tr>
  <tr>
    <td><br>  msgContent<br>  </td>
    <td><br>  True<br>  </td>
    <td><br>  MessageContent<br>  </td>
    <td><br>  -<br>  </td>
    <td><br>  消息，目前仅支持文本，包含消息标题，消息内容，发送时间。其中消息标题和发送时间非必填<br>  </td>
  </tr>
</table>
- msgReceivers消息接收者。可以是邮件地址，或者是电话号码，或者是APPID。
- msgContent消息内容，可以设置消息标题、文本内容。

## 开发步骤 ##
1. 引入消息发送组件Jar包，  
如果是maven工程，可以直接使用如下依赖：  

		<dependency>
			<groupId>com.yonyou.iuap</groupId>
    		<artifactId>iuap-message</artifactId>
    		<version>1.0.0-RELEASE</version>
		</dependency>
		
2. 将配置文件放到指定目录  
![img001](img/image001.jpg)  
 - msconfig.xml用来配置消息发送者和服务器的信息，需要自行补充参数，某些服务需要跟服务商家联系开通  
 - ReturnCodeDescription.xml是服务器返回响应的状态描述文件，不需要修改  
 - 以上两个文件，都存放到工程的classpath路径下  
3. 调用消息服务的接口方法，发送消息  
![img002](img/image002.gif)  
 1. 创建消息接收者对象  
 
			//消息接收者可以是手机号码、电子邮件或者APP ID，当有多个地址的时候，用","分割
			MessageReceiver msgReceivers = newMessageReceiver("1522247xxxx");
 2. 创建消息对象  
 
			//分别是消息标题（可选）、消息内容（必选）、发送时间（可选）
			MessageContent msgContent = new SMSContent("title", "下班了，周末了", 0);
 3. 调用消息发送服务接口，发送消息  
 
			List<MessageResponse> responseList = new MessageSend(msgReceivers, msgContent).send();

## 扩展机制 ##
发送HTML内容的电子邮件：  
在设置邮件发送内容时，可以直接编写HTML代码，如下：  

	StringBuffer htmlContent = new StringBuffer();
	htmlContent.append("<h1>我是标题</h1>");
	htmlContent.append("<h3>企业互联网运营支撑平台</h3>");
	htmlContent.append("<div><img src='http://img4.3lian.com/sucai/img6/230/29.jpg'></div>");
	htmlContent.append("<a href='http://ieop.yyuap.com/'>用友应用支撑平台</a>");
	MessageContent mailContent = new EmailContent("HTML Mail测试", htmlContent.toString());
这样就可以发送HTML格式的邮件。  
