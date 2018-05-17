# Rest调用

## 1、功能简介

一个现实场景：</br>
为了避免将服务暴露出来容易受到恶意访问，以及在访问服务时无法保证每次都能携带用户信息，又有访问需求的情况下，双方约定了一种访问方式，通过authfile.txt文件校验来完成服务调用，即Rest调用方式

## 2、开发环境配置</br>
pom.xml上增加如下配置，添加依赖
加签依赖

![](/articles/application/6-/images/13/rest01.png) 

版本号为maven私服版本   现行版本3.1.0-RELEASE 
 
![](/articles/application/6-/images/13/rest02.png) 
    
认证依赖
 ![](/articles/application/6-/images/13/rest03.png) 

在路径/eiap-plus/src/main/resources/下找到这两个文件

 ![](/articles/application/6-/images/13/rest04.png) 

其中applicationContext-shiro.xml 几项重要配置中</br>
登录认证：

![](/articles/application/6-/images/13/rest05.png) 
 
加签认证

 ![](/articles/application/6-/images/13/rest06.png) 

加载文件指定路径（注：sign-outer.properties 中的配置）

![](/articles/application/6-/images/13/rest07.png) 

![](/articles/application/6-/images/13/rest08.png) 
 

调用方法配置，将controller类中访问名 复制到配置中，如图中peopleRestWithSign
 
 ![](/articles/application/6-/images/13/rest09.png) 

![](/articles/application/6-/images/13/rest10.png) 

signAuth是加签调用方式：anon是非加签调用方式</br>
sign-outer.properties中的配置

 ![](/articles/application/6-/images/13/rest11.png)
 
	authfile.txt中的信息

![](/articles/application/6-/images/13/rest12.png) 
 
如果一切顺利加签调用就会成功</br>
## 3、拓展：
请求来的时候会判断所请求的服务是否是加签调用，以非加签方式调用加签则会报 400的错误信息会显示在请求头中(请求超时也会报400的错误\url校验失败也会报400)，
（建议使用RestUtils这个类进行开发，项目中RestUtils这个类中封装了post与get请求，其中doPostWithSign\doGetWithSign分别是加签调用的post与get请求方式，发送请求时会先读取配置文件内容，要读的内容就是sign-outer.properties中的内容）</br>
 如果是加签调用，将sign-outer.properties文件读到系统内，读取authfile文件中的信息与访问带来的加签内容进行比对比较，如果一致则调用成功，如果不一致则失败

RestUtils类

![](/articles/application/6-/images/13/rest13.png)  

现行版本</br>
<module.version>3.5.1-RELEASE</module.version>

