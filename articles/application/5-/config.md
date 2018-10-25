# 门户配置

门户配置包括小部件管理和布局管理。门户页面集成的主要功能点：可以使用小部件管理加入页面中需要的片段，再通过布局进行装载，最后把页面集成到需要展示的位置。

## 小部件管理

小部件管理，有多种页面片段可以装载。支持四种类型：url\xml\html片段\js。

单击【小部件管理】，进入小部件管理页面，此页面可以进行小部件的创建、查询、编辑、复制和删除等操作。

![](/articles/application/5-/images/image7.png)

 
**创建小部件**

单击〖创建小部件〗按钮，弹出下图所示窗口：

![](/articles/application/5-/images/image8.png)

 
（1）	url：可以直接填写URL链接

![](/articles/application/5-/images/image9.png)

 
（2）	xml：开发的widget路径

![](/articles/application/5-/images/image10.png)

 
比如：系统中提供了轮播图功能，直接注册以上xml路径，可加载轮播图的widget。

（3）	html：直接填写html路径或者直接在框内填写html片段

![](/articles/application/5-/images/image11.png)

![](/articles/application/5-/images/image12.png)




html片段
 
 
（4）	js：开发的js路径注册

**注意**：所有的小部件如果需要被使用，则必须通过“小部件授权”节点，对对应角色做授权后，才可以在“布局管理”节点中使用。授权的操作过程请参考本文2.3.8.6“小部件授权”章节。

## 布局管理
用户在自己的小应用上可能会有多种定制化的要求，比如由多样化的区域拼接而成，比如由多个信息空间组装成一个复杂页面等等应用。此时可以通过布局管理功能点完成，在布局管理上通过不同模板中加载小部件的方式，定制出多种多样样式的布局页面。

单击【布局管理】，进入布局管理页面，此页面可进行布局的创建、批量删除、编辑、复制、设计、删除和查询等操作。
布局管理页面如下：

![](/articles/application/5-/images/image13.png) 

**创建布局**

单击〖创建布局〗按钮，填写相应内容新建布局：

![](/articles/application/5-/images/image14.png)
 
点击〖创建并打开〗，打开设计器：

![](/articles/application/5-/images/image15.png)
 
右侧〖模板〗，选择页面所需样式布局模板：

![](/articles/application/5-/images/image16.png)
 
选择后点击〖模板〗，可关闭模板区域。

点击〖小部件〗，则可展示出在“小部件管理”中定义的小部件组件。选择小部件，并拖拽到页面中的组件区域【白色部分】：

![](/articles/application/5-/images/image17.png)
 
拖拽轮播图到模板区域：

![](/articles/application/5-/images/image18.png)
 
拖拽的数据，可以在右上角编辑属性。

比如：点击编辑图标，则弹出小部件属性区，可以编辑高度、每页显示条数等，属于widget中定义的信息。

![](/articles/application/5-/images/image19.png)
 
轮播图属性区：

![](/articles/application/5-/images/image20.png)
 
点击〖保存〗可以将布局保存。〖重置〗功能：恢复到上一步状态。〖关闭〗：关闭设计器，返回上一级页面。

**批量删除**

多选，批量删除布局。

**编辑**

编辑布局属性信息。

**复制**

用于复制布局。

**设计**

打开设计器。

**删除**

删除选中的布局信息。
