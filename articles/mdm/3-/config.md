## 安装与配置 ##
### 配置流程
在整个主数据环境搭建过程中，大体配置流程如下：

- 准备启动环境所需要的中间件，包括数据库、Tomcat、Redis、Zookeeper、JDK等；
- 对安装盘里面的war包，修改其配置文件，主要是修改数据库连接信息，redis连接信息，zookeeper连接信息等；
- 部署安装盘里面的war包到tomcat下面webapps目录中；
- 启动中间件；
- 进行license授权；
- 登录使用。


### 环境配置要求
##### 1.服务器端要求

| 操作系统        | 数据库（oracle 11g）           | JDK（1.8.0_25）  |
| ------------- |:-------------:| :-----:|
| Windows2008      | 支持      |支持|
| Windows2012      | 支持      |支持|
| Red Hat 6.4 | 支持      |支持|
| 中标麒麟      | 支持      |支持|
| Centos7      | 支持      |支持|
注意:服务器配置还需要 tomcat、redis、zookeeper、数据库服务器、license-server，服务器端应用规模硬件配置推荐（仅限私有部署）

中间件版本说明：

| 中间件        | 版本)
| ------------- |:-------------:|
| Tomcat      | 8.5.20      |
| Redis      | 3.2.9     |
| Zookeeper |  3.4.8     |
| JDK     | 1.8.0_25      |
| ORACLE      | 11g      |

服务器端应用规模硬件配置推荐（仅限私有部署）

![](/articles/mdm/3-/images/1.jpg)

##### 2.客户端配置要求

客户端硬件配置要求

![](/articles/mdm/3-/images/2.png)

客户端软件配置要求

![](/articles/mdm/3-/images/3.png)

##### 3.网络相关要求

&ensp;&ensp;&ensp;&ensp;用户通过防火墙访问应用平台服务器时，需要注意在防火墙上开放相应端口。用户可以使用单机应用或集群模式灵活配置环境，需要保证相关端口不被其他应用占用，在设置防火墙端口策略时需要注意开放上述端口。

&ensp;&ensp;&ensp;&ensp;在数据库服务器和应用服务器上不要安装或启用 DHCP、DNS、PROXY、WINS 和防火墙等服务。以Windows 系统作应用服务器的用户请将防火墙功能停止，保证数据库服务器和应用服务器，应用服务器和应用服务器间高速网络通信，强烈推荐应用服务器、数据库服务器、web 服务器间使用千兆网络进行连接，不建议安装或设置跨网关或跨防火墙通信。

&ensp;&ensp;&ensp;&ensp;应用服务器的网卡正确设置很重要。通常情况下，用户使用的是 tomcat 中间件，都要保证网卡驱动、物理连线、地址、网关、路由等被正确配置，如果环境中有网卡被启用而未连接物理网线，可能会影响应用平台 3.1 系统网络操作性能，在此建议禁用不使用的网卡。


### 配置向导

##### 1.安装 redis

redis安装，以Linux系统 centos7 为例（Windows系统同样支持），执行如下命令：

yum install epel-release

yum install redis

安装完后配置文件为/etc/redis.conf，修改以下地方，允许远程访问和要求密码。

 #bind 127.0.0.1

Requirepass test123

redis 的启动：

systemctl start redis

redis 的停止：

systemctl stop redis


##### 2.安装Zookeeper

从官网上下载 zookeeper 的安装包，安装启动即可。

推荐使用 zookeeper3.4.8


##### 3.授权说明（这里示例的licenseserver是采用了单独部署的方式）
解压 iuap-licenseserver 到 tomcat 的 webapps 下, 配置\webapps\iuap-licenseserver\WEB-INF\classes\application.properties 文件如下:

![](/articles/mdm/3-/images/4.jpg)

需要安装 redis 和 zookeeper 服务器，并在 application.properties 中进行配置。另外，可以修改 tomcat的端口，保证端口没被占用即可。

通过执行 tomcat/bin/startup.bat 启动 licenseServer 服务器。

访问 http://ip地址:8088/iuap-licenseserver，用户名：admin,初始密码：admin（可自行修改）。

![](/articles/mdm/3-/images/5.jpg)

在上图中，点击“生成硬件锁”按钮，进入生成硬件锁界面，如下：

![](/articles/mdm/3-/images/6.jpg)

在上图中，输入产品号，选择 licenseServer 所在的应用系统，点击“生成硬件锁”，会生成 hardkey.req文件。把 hardkey.req 发给用友云平台商务部，获得 license 授权许可文件 license.resp。

在 license 管理系统主界面中，点击“导入授权”按钮，上传 license.resp 文件，即可导入授权许可。

![](/articles/mdm/3-/images/7.jpg)

##### 4.安装前的系统配置（JDK）

Linux 机器上将 ip 和机器名写到/etc/hosts 里。如果配置集群有多台机器，将所有机器的 ip 和机器名都写到/etc/hosts 里。

关闭防火墙或开通所有需要用到的端口，如 tomcat 的 8080 或其他自定义端口。

使用 tomcat 或 weblogic 中间件时需要 oracle 的 jdk，从 oracle 官网下载安装 64 位的 1.8，并配置为

JAVA_HOME 环境变量。

Linux 配置 JAVA_HOME，如下：

JAVA_HOME=/usr/local/CMDM/java/jdk1.8.0_25

JRE_HOME=/usr/local/CMDM/java/jdk1.8.0_25

CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib

PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin

export JAVA_HOME JRE_HOME CLASS_PATH PATH

source /etc/profile

windows 配置 JAVA_HOME

![](/articles/mdm/3-/images/8.jpg)

![](/articles/mdm/3-/images/9.jpg)

![](/articles/mdm/3-/images/10.jpg)

##### 5.数据库脚本初始化以及执行顺序

脚本地址：解压 eiap.zip，iUAP_eiap\DB\data\mysql

Wbalone--->Matedata—> Uitemplate_web--> Mdbiz-->Iuap_bmp-->Iuap_eiap_example-->Bpm-->Basedoc-->iuapmdm

##### 6.核心工程配置

&ensp;&ensp;&ensp;&ensp;上面章节提到这里的示例是licenseserver授权服务和核心的业务工程服务分开部署，授权服务上面已经介绍，这里主要说明核心工程的启动配置。核心工程如下图所示：

![](/articles/mdm/3-/images/11.png)

每个war包的主要功能介绍如下：

1. basedoc：人员档案节点，主要用来管理人员；
2. eiap-plus：主要包含一些额外的功能服务，例如：按钮授权，在线用户等。
3. wbalone：提供iuap应用平台服务，主数据依赖的基础平台；
4. uui：iuap应用平台的前端依赖代码；
5. iuapmdm：提供主数据的核心服务；
6. uitemplate:提供参照依赖服务；
7. 其他：其他的war包是提供关于企业流程管理bpm的服务，其中example war包是关于流程的一个示例。

各个项目具体配置如下：

a.wbalone项目配置

在webapps\wbalone\WEB-INF\classes下面打开application.properties文件修改数据库连接信息、redis连接信息、zookeeper连接信息以及加签文件信息（依据实际情况配置），如下图。

![](/articles/mdm/3-/images/12.png)

配置数据库，主要修改数据库 ip、端口、数据库名、用户名和密码。

![](/articles/mdm/3-/images/13.png)

注意：此处配置的 redis 最好不要与其他系统共用。

![](/articles/mdm/3-/images/14.png)

备注：此处填写的地址是 authfile.txt 放到服务器上的路径，其他组件也会用到此文件，需要配置此路径。

如果对接流程管理组件，则配置流程服务地址。

![](/articles/mdm/3-/images/15.png)

打开authbac-sdk.properties文件，配置正确的加签服务地址。

![](/articles/mdm/3-/images/16.png)

打开iuap-licenseclient-conf.properties，配置licenseserver授权服务。

![](/articles/mdm/3-/images/17.png)

打开workbench-sdk.properties文件配置工作台访问路径以及加签文件。

![](/articles/mdm/3-/images/18.png)

打开logback.xml文件配置日志路径。

![](/articles/mdm/3-/images/19.png)


b.iuap-eiap-bpm-service项目配置

在\iuap-eiap-bpm-server\WEB-INF\classes下面打开application.properties文件修改数redis连接信息、云审批连接信息以及加签文件信息（依据实际情况配置），如下图。

![](/articles/mdm/3-/images/20.png)

在auth.properties文件中配置redis连接信息。

![](/articles/mdm/3-/images/21.png)

在jdbc.properties文件中配置数据库连接信息，主要修改数据库 ip、端口、catalog、数据库名、用户名和密码。

![](/articles/mdm/3-/images/22.png)

在workbench-sdk.properties中修改工作台地址以及加签文件。

![](/articles/mdm/3-/articles/mdm/3-/images/23.png)

打开logback.xml文件配置日志路径。

![](/articles/mdm/3-/images/24.png)

c.ubpm-web-rest项目配置

在\ubpm-web-rest\WEB-INF\classes下面打开application.properties文件修改文件存储系统以及zookeeper分布式锁。

![](/articles/mdm/3-/images/25.png)

打开iuap-licenseclient-conf.properties，配置licenseserver授权服务。

![](/articles/mdm/3-/images/26.png)

在\ubpm-web-rest\conf目录下打开cache.properties文件修改redis连接信息。

![](/articles/mdm/3-/images/27.png)

在db.properties文件中修改数据源连接信息。

![](/articles/mdm/3-/images/28.png)

d.ubpm-webserver-process-center2项目配置

在\ubpm-webserver-process-center2\conf目录下打开cache.properties文件修改redis连接信息。

![](/articles/mdm/3-/images/29.png)

在db.properties文件中修改数据源连接信息。

![](/articles/mdm/3-/images/30.png)

在\ubpm-web-rest\WEB-INF\classes下面打开application.properties文件修改文件存储系统。

![](/articles/mdm/3-/images/31.png)

e.basedoc项目配置

在basedoc\WEB-INF\classes\properties下面打开文件applicationContext.properties修改数据库连接信息、redis连接信息、zookeeper连接信息等。

![](/articles/mdm/3-/images/32.png)

在basedoc\WEB-INF\classes下面打开文件sdk.properties修改本机服务地址。

![](/articles/mdm/3-/images/33.png)

打开workbench-sdk.properties文件配置工作台访问路径以及加签文件。

![](/articles/mdm/3-/images/34.png)

f.eiap-plus项目配置

在eiap-plus\WEB-INF\classes目录下打开application.properties修改数据库连接信息、redis连接信息、客户端身份文件路径、流程管理配置、本系统地址、zookeeper连接信息等。

![](/articles/mdm/3-/images/35.png)

![](/articles/mdm/3-/images/36.png)

![](/articles/mdm/3-/images/37.png)

打开authbac-sdk.properties文件，配置正确的加签服务地址。

![](/articles/mdm/3-/images/38.png)

打开workbench-sdk.properties文件配置工作台访问路径以及加签文件。

![](/articles/mdm/3-/images/39.png)

g.uitemplate项目配置

在uitemplate_web\WEB-INF\conf\uitemplate目录下打开datasource.properties修改数据库连接信息、redis连接信息。

![](/articles/mdm/3-/images/40.png)

打开uitemplate.properties文件修改参照地址。

![](/articles/mdm/3-/images/41.png)

h.iuapmdm项目配置

在iuapmdm\WEB-INF\classes目录下打开application.properties文件修改数据库信息，redis连接信息、bpm配置信息、文件存储系统配置信息。

![](/articles/mdm/3-/images/42.png)

![](/articles/mdm/3-/images/43.png)

在bpm-sdk.properties文件中配置bpm连接信息。

![](/articles/mdm/3-/images/44.png)

打开iuap-licenseclient-conf.properties，配置licenseserver授权服务。

![](/articles/mdm/3-/images/45.png)

打开sdk.properties文件配置加签文件以及本机服务地址

![](/articles/mdm/3-/images/46.png)

打开sdk-yyuap.properties文件配置加签文件以及本机服务地址

![](/articles/mdm/3-/images/47.png)



##### 7.部署到 tomcat 单机
&ensp;&ensp;&ensp;&ensp;以Centos7为例,将配置好的eiap.zip和iuapmdm.zip拷贝到/usr/local/CMDM/home1/apache-tomcat-8.5.20/webapps 目录下：


查看文件 apache-tomcat-8.5.20\conf\server.xml 里的 8005,8080 端口，如果这两个端口已被占用，可修改为其他端口、

![](/articles/mdm/3-/images/48.jpg)

通过执行./apache-tomcat-8.5.20/bin/startup.sh 启动;

启动完毕后，登录地址：http://172.20.4.241:8080/wbalone

系统管理员：admin	默认密码：123456a

主数据管理员：mdmadmin001	无默认密码，需要 admin 登录系统-管理中心-用户管理节点，为

mdmadmin001 重置密码为 123456a。备注：请及时修改密码

双击./apache-tomcat-8.5.20/bin/shutdown.sh 停止;

或者通过 kill 命令杀掉 tomcat 的进程。	(kill -9 进程号)

##### 8.部署到 tomcat 集群

nginx以在 linux 上安装为例。

以	centos7 为例，执行如下命令

 yum install epel-release

yum install nginx


安装完后配置文件为/etc/nginx/nginx.conf

nginx 的启动


./nginx


nginx 的停止


./nginx–s stop

以机器 172.20.4.241 为例。在此服务器上搭建两套 tomcat 且配置一致，但端口号保证不重复。

例如：第一套 tomcat：172.20.4.241:8080

第二套 tomcat：172.20.4.241:8081

在172.20.4.241上安装 nginx，配置/etc/nginx/nginx.conf，例子如下

[root@centos191 ~]# cat /etc/nginx/nginx.conf

![](/articles/mdm/3-/images/49.png)

红色是修改的部分。

分别启动两套 tomcat 环境，确保环境正常；然后启动 nginx。

此时就可通过 http://172.20.4.241/wbalone 访问环境了，集群搭建完毕。

备注：nginx 集群中的多个 Tomcat 也可在不同机器上搭建，只要在 nginx.conf 中配置相应的 ip:port 即可。

### 卸载及常见问题
+ 代码卸载: 
对于使用 tomcat 中间件用户，停止中间件的所有服务，删除 webapps 目录下个所有 war 包即可。 
+ 数据库卸载: 
对于 oracle 数据库用户，删除应用平台应用的数据库即可。
+ 安装升级过程常见问题和注意事项: 
本版不涉及升级
+ 常见问题和注意事项:
Redis 服务器最好不要与其他系统公用。
