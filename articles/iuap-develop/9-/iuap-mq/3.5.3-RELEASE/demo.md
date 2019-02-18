开发示例
========

### 4.1 依赖环境

添加maven依赖

\<**dependency**\>  
\<**groupId**\>com.yonyou.iuap.pap.message\</**groupId**\>  
\<**artifactId**\>iuap-message-sdk\</**artifactId**\>  
\<**version**\>\${module.version}\</**version**\>  
\</**dependency**\>

### 4.2 配置文件

增加文件msg-sdk.properties，文件中配置消息组件的地址，示例：**msgCenter.base.url**=**http://10.10.24.84:8080/iuap-saas-message-center/**

### 4.3 调用示例

消息发送可以按照纯文本发送或者选择按照预置模板进行发送，msg为json格式，内容为消息发送所需参数。

文本消息：com.yonyou.uap.msg.sdk.MessageCenterUtil.*pushTextMessage*(msg.toString());

模板消息：com.yonyou.uap.msg.sdk.MessageCenterUtil.*pushTemplateMessage*(msg.toString());

![](image/c67b28b4c7dc48d8d9ac61e0e0677640.png)

图 12