## 概述

### portal架构和框架

![](/articles/portal/6-/images/6-d-1.png)

### 登录框架

Portal登陆框架采用的是现在比较流行且技术相对成熟的框架shiro，通过它的认证体系机制完成门户基本的登录认证功能，采用的用户权限模型说明图如下：

![](/articles/portal/6-/images/6-o-1.png)

说明：

1.用户信息用 LoginAccount 表示，最简单的用户信息可能只包含用户名 loginName 及密码 password 两个属性。实际应用中可能会包含用户是否被禁用，用户信息是否过期等信息。

2.用户权限信息用 Role 与 Permission 表示，Role 与 Permission 之间构成多对多关系。Permission 可以理解为对一个资源的操作，Role 可以简单理解为 Permission 的集合。

3.用户信息与 Role 之间构成多对多关系。表示同一个用户可以拥有多个 Role，一个 Role 可以被多个用户所拥有


### 导航框架

导航模板可以在外层框架模板里定义，外层框架模板平台会默认提供一个类似分享销客风格的html，如/workbench/index.html。当默认模板不满足业务层需求时，可以自定义业务模板。仅需要在用户登录时通过接口
iuap.portal.runtime.service.ILayoutViewService方法getFrameTemplateUrl返回即可。



### 布局

预置目录：手工预置到表pt_layout完成预置布局

备注：

Widget Url支持相对路径，支持业务扩展补全功能。
站点分类layoutid需要填写，设计时参照纷享销客界面元素，现在类似分类。
           
预置内容注意点：

1.顶层html元素仅有一个

2.不能包含有style属性

```
<div class="row"> // 顶层元素必须1个
	<div class="col-md-6 ui-grid" id="widgetbox1">
		<WidgetBox id="widgetbox1_t">
		<Widget
			gadgetURL="/workbench/gadgets/MyPublish.xml"   style=”不应该有”  //这里应删除
			id="MyPublish"></Widget>
	           …
	</div>
</div>
```


### Widget

见developWidget



### 设计器

### 主要特征

事件订阅包括：事件的订阅和事件的发布，涉及前端组件：messenger.js

应用点：

1.门户前台应用层业务参数接入

2.Widget间通信（临时）


### 单点登录

CAS集成
