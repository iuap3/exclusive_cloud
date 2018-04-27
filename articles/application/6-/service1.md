## 1.概述


基于应用开发平台开发的应用是具备微服务能力，需要对新建的应用单独做身份认证控制，否则新建的应用就可以不通过登录认证直接进行访问，而这些应用如何解决会话信息及身份认证的问题呢？应用开发平台已经提供了一套机制，可以结合应用开发平台的统一登录，并且各个应用通过使用登录认证组件，结合下文中的相关配置，将应用开发平台和各个应用部署到同一个域名下就能够解决，实现各自应用的身份认证问题；


## 2.项目配置

在开发工具Devtool中新建iuap项目后对配置文件进行的修改如下：</br>
1) src/main/webapp/WEB-INF路径下的web.xml文件，打开该文件中对shiroFilter的配置。</br>
<pre>
&lt;filter>
    &lt;filter-name>shiroFilter&lt;/filter-name>  				 
    &lt;filter-class>
        org.springframework.web.filter.DelegatingFilterProxy
    &lt;/filter-class>
    &lt;init-param>s
        &lt;param-name>targetFilterLifecycle&lt;/param-name>
        &lt;param-value>true&lt;/param-value>
    &lt;/init-param>
&lt;/filter>

&lt;filter-mapping>
    &lt;filter-name>shiroFilter&lt;/filter-name>
    &lt;url-pattern>/*&lt;/url-pattern>
&lt;/filter-mapping>
</pre>
2) src/main/resources下的applicationContext-shiro.xml对Id为webTokenProcessor和maTokenProcessor的bean增加如下配置：
<pre>
&lt;property name="exacts">
    &lt;list>
        &lt;value type="java.lang.String">tenantid&lt;/value>
        &lt;value type="java.lang.String">userId&lt;/value>
        &lt;value type="java.lang.String">userType&lt;/value>
        &lt;value type="java.lang.String">typeAlias&lt;/value>
    &lt;/list>
&lt;/property>
</pre>
3) 修改新建应用src/main/resources目录下的application.properties配置文件，该文件中对redis.url、redis.session.url和sysid的配置需要与wbalone中application.properties文件对应的配置一致。

完成上述配置后就可以通过wbalone实现对应用的登录身份认证管理。