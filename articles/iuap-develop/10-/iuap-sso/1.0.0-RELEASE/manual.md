# 第三方登录 #

## 功能简介 ##

第三方登录组件是将各主流通讯软件以及门户网站作为登录方式，通过Oauth2.0协议，在不获取第三方账号安全信息的前提下，通过第三方账号的认证，登录自有系统。  

## 工作流程 ##

通过在登录主页面加载的第三方登录按钮，调用LoginServlet。根据传入第三方系统类型，转入第三方登录系统登录界面。登录认证成功后，获取登录信息，包括第三方登录token、用户信息等。登录后调用预留处理接口，将登录信息传入，供本系统做后续操作。  
登录页面调用com.yonyou.uap.ieop.login.servlet.LoginServlet，传入登录参数。其中，登录类型LoginType=WECHAT（目前提供微信：WECHAT ，新浪微博：WEIBO，qq：QQ三种方式）。  
根据传入的登录类型，调用不同IloginPubService的实现。  
以QQ为例，调用腾讯平台提供不同接口，分别根据AppKey和AppId获取用户认证code，认证token，以及查询qq用户信息，并转化为统一用户登录信息，调用 `com.yonyou.uap.ieop.login.afterlogin.IafterloginService`默认实现类`com.yonyou.uap.ieop.login.afterlogin.ShiroAfterloginService`的doAfterLogin.  

		/**
		 * <p>Title: doAfterLogin</p>
		 * <p>Description: 第三方登录成功后，自有系统登录，并进行用户信息关联存储</p>
		 * @param request
		 * @param response
		 * @param info
		 */
		publicvoid doAfterLogin(HttpServletRequest request,
				HttpServletResponse response,LoginInfo info);  



## API接口 ##

### 微信登录 ###

**描述**  
用户通过登录微信帐号来授权，使用部分微信信息，登录本网站  
**请求方法**  

	com.yonyou.uap.ieop.login.servlet.LoginServlet  
**请求方式**  
服务调用  
**请求参数说明**  

<table>
  <tr>
    <th><br>  参数字段<br>  </th>
    <th><br>  必选<br>  </th>
    <th><br>  类型<br>  </th>
    <th><br>  长度限制<br>  </th>
    <th><br>  说明<br>  </th>
  </tr>
  <tr>
    <td><br>  LoginType<br>  </td>
    <td><br>  True<br>  </td>
    <td><br>  String<br>  </td>
    <td><br>  20<br>  </td>
    <td><br>  登录类型，此处为WECHAT<br>  </td>
  </tr>
</table>  

**返回参数说明**  
`LoginInfo info：统一用户登陆信息`  


### QQ登录 ###

同3.3.1微信登录，在传入登录类型时，为QQ  


### 新浪微博登录 ###

同3.3.1微信登录，在传入登录类型时，为WEIBO


## 开发步骤 ##

### 与Spring整合 ###

1. **引入组件**  
如果是maven项目的话，可以直接在pom文件里加入如下依赖：  

		<dependency> 
			<groupId>com.yonyou.iuap</groupId>
    		<artifactId>iuap-sso</artifactId>
    		<version>1.0.0-RELEASE</version>
		</dependency>  

2. **web.xml下配置登录servlet和回调servlet**  

		<servlet>
			<servlet-name>login_servlet</servlet-name>
		<servlet-class>com.yonyou.uap.ieop.login.servlet.LoginServlet</servlet-class>
		</servlet>
	    <servlet-mapping>
			<servlet-name>login_servlet</servlet-name>
			<url-pattern>/login_servlet</url-pattern>     
		</servlet-mapping>	
	    <!--第三方 qq登录完成后 回调-->
		<servlet>
			<servlet-name>return_servlet</servlet-name>
		<servlet-class>com.yonyou.uap.ieop.login.servlet.QQAfterLoginServlet</servlet-class> 
		</servlet>
		<servlet-mapping> 
			<servlet-name>return_servlet</servlet-name> 
			<url-pattern>/extlogin/qq</url-pattern>    
		</servlet-mapping>  

	
其中，组件已支持的回调Servlet有：  

QQ为`com.yonyou.uap.ieop.login.servlet.QQUserInfoServlet`  
微信为 `com.yonyou.uap.ieop.login.servlet.WeChatAfterLoginServlet`  
微博为`com.yonyou.uap.ieop.login.servlet.WeiboAfterLoginServlet`    
如果需要支持多个第三方平台登录，需要将对应平台的回调servlet添加到web.xml配置文件中。  


### 前台调用 ###

**前台加入第三方登录按钮并配置相应链接至servlet**  

	<a href=" /login_servlet?LoinType=QQ&isLogin=true" title="QQ">QQ登录</a>  


### 配置文件 ###

1. **配置对应第三方系统的配置文件**  
修改qqconnectconfig.properties(此文件需要放在工程的class path路径下)内容。其中：app_ID和app_KEY为支持等三方登录功能的平台申请的ID和KEY，redirect_URI为用户在第三方平台登录授权后回调的URI地址，需要与3.4.1.2中配置的回调servlet对应。

	app_ID = 101248396
	app_KEY = c1b2f603ce729addc4d63d61c915d00c.
	redirect_URI = http://xxxx:8080/xxxx/extlogin/qq  

2. **指定登录成功后的逻辑处理Service**  
本地创建`com.yonyou.uap.ieop.login.afterlogin.ShiroAfterloginService`类并继承`com.yonyou.uap.ieop.login.afterlogin.IafterloginService`
或在*LoginConfig.properties*(此文件需要放在工程的class path路径下)中配置为其他处理类
`AfterLogin.do = com.yonyou.uap.ieop.login.afterlogin.ShiroAfterloginService`
在`doAfterLogin`方法中处理自有系统原来的登录逻辑。  







