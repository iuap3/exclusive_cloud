## 应用平台在Windows下一键部署使用说明

1.	解压iuap-server安装压缩包到C盘根目录下：

 ![](/articles/application/3-/images/111.png)

2.
在tomcat\webapp目录下部署所需工程如下图：
 
  ![](/articles/application/3-/images/222.png)


2.	首次进行环境配置时，需要删除C:\iuap-server\configtool目录下 data.config .
此后一键部署则不需要删除data.config
然后进入目录C:\iuap-server下
点击启动批处理文件sysconfig.bat 进行环境配置，启动批处理文件后具体配置见下图：
a.
  ![](/articles/application/3-/images/333.png)
b.
  ![](/articles/application/3-/images/444.png)
c.
 ![](/articles/application/3-/images/555.png)
 
d.
 
 ![](/articles/application/3-/images/666.png)
e.
  ![](/articles/application/3-/images/777.png)
f.
 
 ![](/articles/application/3-/images/888.png)
保存配置之后点击×关闭配置界面

启动startup.bat进行一键启动

3.	成功启动界面包括 reids ，Zookeeper，Tomcat
 
 ![](/articles/application/3-/images/999.png)
