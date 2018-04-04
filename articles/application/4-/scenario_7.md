#  功能和按钮权限快速入门

</br>


## 1.概述
通常情况下，不同用户登录同一页面进行访问时，我们希望每个用户只能看到自己有权限访问的菜单【功能点】，对于没有权限访问的菜单项目，我们希望不仅在页面上不进行显示，并且即使用户直接通过url地址进行访问也能够在后台实现拦截；当用户查看具有访问权限的菜单项目时，对于不同的员工所具有的操作权限也需要进行控制，这就需要对用户页面的按钮进行权限的控制。应用平台通过功能管理、按钮管理、菜单管理、角色管理，角色授权等功能实现了对功能和按钮的权限控制。

## 2.配置流程

本小节以督办任务为例，讲述如何通过应用平台实现对项目的功能和按钮进行管理。</br>
### 2.1 功能权限

为了实现对功能的管理需要在应用平台上进行功能注册，创建角色，功能授权，角色分配用户四个步骤。</br></br>
1）功能注册</br>
在管理中心找到功能配置下的功能管理，在功能管理中点击新建按钮进行功能的注册，如下图所示：

![](https://i.imgur.com/REpXS8o.png)

</br>
其中：</br>
链接类型选择为js插件时,链接处添加的是功能的js文件在服务器中的相对路径。</br>
是否启用权限控制：打开后应用平台才可以实现对该功能和功能前台页面按钮的权限控制。</br>
是否启用按钮权限：控制着对按钮权限的控制是否生效。</br>
对应的url：后台权限控制的拦截地址。</br>
</br>
2）创建角色</br>
在管理中心找到权限管理下的角色管理，在角色管理中点击新建，进行角色的创建。

![](https://i.imgur.com/16tkG0A.png)
</br>
3）功能授权</br>
在管理中心找到权限管理下的功能授权，在功能授权中找到需要进行授权的角色，点击功能授权按钮，进入功能授权页面，找到第一步注册的功能——督办任务和应用中心，点击左下角方框，选中功能，点击保存。

</br>
![](https://i.imgur.com/F5HEiNu.png)
</br>

4)角色分配用户</br>
在管理中心找到权限管理下的角色分配用户，在角色分配用户页面上找到步骤二创建的角色，点击分配用户按钮进行角色的分配。

![](https://i.imgur.com/xiLJ4EC.png)
</br>

登录test用户可以看到在界面左侧菜单栏中增加了应用中心功能，点击应用中心功能，可以看到在应用中心页面增加了督办任务功能点。

![](https://i.imgur.com/hxor5oU.png)

### 2.2 按钮权限

按钮权限管理需要在应用平台上进行按钮注册、按钮授权、修改功能前端html文件代码和修改功能js代码四个步骤。

1）按钮注册
</br>在管理中心找到功能配置下的按钮管理，在功能管理中新增加功能后在按钮管理左侧菜单树中也会创建对相应功能，在左侧菜单中找到新建的功能，并选中，右侧显示出该功能页面已经添加权限的按钮。点击增加，添加对新增、删除和修改三个按钮的权限控制，如下图所示：

![](https://i.imgur.com/z7ptpBC.png)
</br>
从上图可以看到在页面中可以对按钮权限进行增加、修改、启用和停用，其中增加、修改和删除提供按钮权限的增删改功能。增加时，按钮编码与步骤三button标签visible属性中buttonShowGroup['按钮编码']事件单引号中的内容一致；添加的url地址为按钮绑定事件对后台发出的请求地址；是否启动状态一栏，显示启用的按钮，会在按钮授权页面进行显示，显示停用的按钮则在按钮授权页面和功能前台界面不会进行显示，无法进行权限的控制。

![](https://i.imgur.com/88z2Lf5.png)
</br>
2）按钮授权</br>
在权限管理中找到按钮授权，在左侧找到需要进行按钮授权的角色，右侧可以看到该角色有权限进行访问的功能，选中需要进行按钮授权的功能，可以看到该功能下能够进行权限控制的按钮。
</br>

![](https://i.imgur.com/quAleI0.png)

</br>
3）功能前端界面（此处为ygdemo_yw_info.html）中将需要进行权限控制的按钮代码写为如下格式：
<pre>
    &lt;button class="u-button u-button-primary " data-bind="click: event.addClick, visible: buttonShowGroup['add']">
        增加
    &lt;/button>
    &lt;button class="u-button u-button-primary " data-bind="click: event.editClick, visible: buttonShowGroup['update']">
        编辑
    &lt;/button>
    &lt;button class="u-button u-button-primary" data-bind="click: event.delRow, visible: buttonShowGroup['delete']">
        删除
    &lt;/button>
</pre>
可以看到对需要增加按钮权限的按钮在data-bind属性中增加了visible: buttonShowGroup['按钮编码']，注意：此处单引号中的内容必须与步骤一中增加的按钮编码一致。

</br>
4）在功能的js文件（此处修改ygdemo_yw_info.js）初始化函数中增加如下代码：</br>
<pre>
    window.initButton(viewModel, element);//初始化按钮权限
</pre>
登录test用户，打开应用中心的督办任务功能，可以看到在界面中可以显示增加按钮，编辑和删除按钮被隐藏。

![](https://i.imgur.com/4XLhCSm.png)


### 2.3 网页拦截开发步骤

#### 在web.xml中进行拦截器的配置
在src/main/webapp/WEB-INF/web.xml的shiroFilter后面增加如下配置，作为经过登录认证后的权限拦截器，用户不能够通过url 非法的请求后台服务。
<pre>
    &lt;filter>
        &lt;filter-name>authorizationCheckFilter&lt;/filter-name>
        &lt;filter-class>com.yonyou.iuap.crm.ieop.security.filter.AuthorizationCheckFilter&lt;/filter-class>
    &lt;/filter>
    &lt;filter-mapping>
        &lt;filter-name>authorizationCheckFilter&lt;/filter-name>
        &lt;url-pattern>/*&lt;/url-pattern>
    &lt;/filter-mapping>
</pre>
</br>






