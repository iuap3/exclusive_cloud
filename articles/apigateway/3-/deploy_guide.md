# 网关一键部署使用说明（Linux版）
## 特殊说明 
文中截图为xftp6和xshell6界面截图，演示系统版本为CentOS release 6.5。若系统版本不同，请将文中命令替换为相应的命令执行。
## 环境要求
1、配置工具需要图形界面的支持，CentOS6.5图形界面安装并在windows下远程连接，请参考以下命令（若已有图形界面并能够登录请忽略）：
```
#安装图形界面
yum groupinstall "Desktop" "Desktop Platform" " X window system"
#安装完成，启动桌面系统
init 5
#确认图形界面已经启动：
runlevel
#安装xrdp工具，支持windows远程连接
yum install xrdp
yum install tigervnc-server 
service xrdp start
#打开window远程桌面，输入linux主机IP访问，弹出界面后输入用户名密码即可。
```  

2、APILink网关服务需要MySQL、Redis及Elasticsearch环境，请自行安装。  
MySQL版本要求5.6.x，需要设置大小写不敏感（lower_case_table_names = 1）。  
Redis版本要求3.0.x ，并能够无密码连接(建议通过bind_ip和防火墙进行安全控制)。  
Elasticsearch版本要求2.4.1，能够无用户名密码连接。   

3、MySQL脚本位于SQL目录中  
![MySQL脚本列表](/articles/apigateway/3-/images/deploy2.1.png)  
这里需要建两个库，一个用于网关控制台应用（private_gateway_create.sql、private_gateway_init.sql），一个用于日志管理（agent_create.sql）。

## 操作步骤
1、解压apilink-server-private.tar.gz到 /opt目录：  
![安装包位置](/articles/apigateway/3-/images/deploy3.1.png)  

在workdir目录下所部署工程如下图：  
![web应用列表](/articles/apigateway/3-/images/deploy3.2.png) 

2、进入/opt/apilink-server-private/tools目录，运行openresty-install.sh安装网关中间件。  

3、进入/opt/apilink-server-private目录，执行批处理文件sysconfig.sh（执行命令：./sysconfig.sh）进行环境配置：  
> **基础配置**  
访问协议：API接口服务地址的访问协议。  
API接口服务地址：网关对外API接口的访问地址，也是网关控制台的访问地址。  
网关地址：nginx服务的地址，单机配置默认即可，集群配置可用多个地址加分号 `;` 隔开。  

![基础配置](/articles/apigateway/3-/images/deploy3.3.png)  
<br/>
> **缓存服务**  

![缓存服务](/articles/apigateway/3-/images/deploy3.4.png)  
<br/>
> **邮件服务**  
用于接口监控预警功能。  

![邮件服务](/articles/apigateway/3-/images/deploy3.5.png)  
<br/>
> **友户通**  

![友户通](/articles/apigateway/3-/images/deploy3.6.png)  
<br/>
> **开发者中心**  

![开发者中心](/articles/apigateway/3-/images/deploy3.7.png)  
<br/>
> **日志服务**  

![日志服务](/articles/apigateway/3-/images/deploy3.8.png)  

保存配置后点击×关闭配置界面  
启动startup.sh（执行命令./startup.sh）进行一键启动  
![启动脚本1](/articles/apigateway/3-/images/deploy3.9.png)  
输入1则选择简体中文，该脚本后面的提示均使用中文；输入2则选择英文，脚本后面的提示均使用英文。  
这里选择1  
![启动脚本2](/articles/apigateway/3-/images/deploy3.10.png)  
此时选择是否要以最新配置启动，即选择用以上几步界面的配置进行启动，这里选择y，替换最新配置进行启动。
  
4、成功启动服务包括 nginx(80,443,8081)，logstash，tomcat(8080,8009,8005)。其中80，443属于可暴露给外部的端口，其他均为内部端口。
  
5、至此部署已完成，可在浏览器输入：http(s)://API接口服务地址 访问APILINK网关控制台。
初始管理员账号为isvadmin ，密码为111111

## 注意事项
1、Nginx服务如果宕机，或者由于其他因素需要停机，那么服务再次启动后，需要去网关控制台->管理员门户->数据初始化 恢复nginx内存数据。  
如下图，需要用管理员账号登录。  
![Nginx内存数据恢复](/articles/apigateway/3-/images/deploy4.1.png)  
host即配置工具中填写的网关地址（下图红框），密码为apilink  
![网关地址示意图](/articles/apigateway/3-/images/deploy4.2.png)  

2、Redis服务如果宕机，或者由于其他因素需要停机，再次启动时，可用网关控制台提供的redis数据重载进行恢复（若已有redis持久化或其他内存数据恢复机制可忽略）。  
Redis重载页面如下图，密码为apilink@2017  
![Redis内存数据恢复](/articles/apigateway/3-/images/deploy4.3.png) 
