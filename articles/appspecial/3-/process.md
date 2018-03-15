##1.配置流程
在整个专属云应用环境搭建过程中，大体配置流程如下：

- 准备启动环境所需要的中间件，包括数据库、Tomcat、Redis、Zookeeper、JDK、FastDFS、Rabbitmq等；
- 对安装盘里面的war包，修改其配置文件，主要是修改数据库连接信息，Redis连接信息，Zookeeper连接信息，FastDFS的连接信息，Rabbitmq的连接信息等；
- 部署安装盘里面的war包到tomcat下面webapps目录中；
- 启动中间件；
- 数据库脚本初始化；
- 进行license授权；
- 启动应用服务(tomcat)
- 登录使用。
##2.环境配置要求
###2.1服务器端要求
![](https://i.imgur.com/pHjchut.jpg)


服务器端应用规模硬件配置推荐（仅限私有部署）
![](https://i.imgur.com/TQskwrj.jpg)
 
###2.2客户端配置要求
客户端硬件配置要求
![](https://i.imgur.com/J5fMgC1.jpg)

客户端软件配置要求
![](https://i.imgur.com/VFzTnSG.jpg)
###2.3网络相关要求
&ensp;&ensp;&ensp;&ensp;用户通过防火墙访问应用平台服务器时，需要注意在防火墙上开放相应端口。用户可以使用单机应用或集群模式灵活配置环境，需要保证相关端口不被其他应用占用，在设置防火墙端口策略时需要注意开放上述端口。 

   
在数据库服务器和应用服务器上不要安装或启用 DHCP、DNS、PROXY、WINS 和防火墙等服务。以Windows 系统作应用服务器的用户请将防火墙功能停止，保证数据库服务器和应用服务器，应用服务器和应用服务器间高速网络通信，强烈推荐应用服务器、数据库服务器、web 服务器间使用千兆网络进行连接，不建议安装或设置跨网关或跨防火墙通信。

&ensp;&ensp;&ensp;&ensp;应用服务器的网卡正确设置很重要。通常情况下，用户使用的是 tomcat 中间件，都要保证网卡驱动、物理连线、地址、网关、路由等被正确配置，如果环境中有网卡被启用而未连接物理网线，可能会影响应用平台 3.1 系统网络操作性能，在此建议禁用不使用的网卡。
##3安装配置向导
###3.1安装操作系统
以Cent OS为例。
###3.2安装JDK
1． Oracle官网下载jdk linux安装包，这里以jdk-7u71-linux-x64.tar.gz为例
2． 解压安装包
tar zxvf jdk-7u71-linux-x64.tar.gz

3．移到相应的位置
mv jdk1.7.0_71 /usr/local/

4．备份系统环境变量
cp /etc/profile /home/mj/

5．编辑系统环境变量
vi /etc/profile
输入i
加入内容如下：
export JAVA_HOME=/usr/local/jdk1.7.0_71
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH
注意标点符号，JAVA_HOME是刚才mv到路径

6．加载刚设置的变量
source /etc/profile

7．测试是否安装成功
输入 java -version 然后会显示jdk的版本信息等

###3.3安装数据库
以mysql-5.6.33-linux-glibc2.5-x86_64.tar.gz为例

1．解压

&ensp;&ensp;#解压

&ensp;&ensp;tar -zxvf mysql-5.6.33-linux-glibc2.5-x86_64.tar.gz

&ensp;&ensp;#复制解压后的mysql目录

&ensp;&ensp;cp -r mysql-5.6.33-linux-glibc2.5-x86_64 /usr/local/mysql

2．添加用户组和用户

&ensp;&ensp;#添加用户组

&ensp;&ensp;groupadd mysql

&ensp;&ensp;#添加用户mysql 到用户组mysql

&ensp;&ensp;useradd -g mysql mysql


3．安装

cd /usr/local/mysql/<br>mkdir ./data/mysql

chown -R mysql:mysql ./

./scripts/mysql_install_db --user=mysql --datadir=/usr/local/mysql/data/mysql

cp support-files/mysql.server /etc/init.d/mysqld

chmod 755 /etc/init.d/mysqld

cp support-files/my-default.cnf /etc/my.cnf
 
&ensp;&ensp;#修改启动脚本

&ensp;&ensp; /etc/init.d/mysqld
 
&ensp;&ensp;#修改项：

&ensp;&ensp;basedir=/usr/local/mysql/

&ensp;&ensp;datadir=/usr/local/mysql/data/mysql
 
&ensp;&ensp;#启动服务

&ensp;&ensp;service mysqld start
 
&ensp;&ensp;#测试连接

&ensp;&ensp;./mysql/bin/mysql -uroot
 
&ensp;&ensp;#加入环境变量，编辑 /etc/profile，这样可以在任何地方用mysql命令了

&ensp;&ensp;`export PATH=$PATH:/usr/local/mysql//bin<br>source /etc/profile`
 
 
&ensp;&ensp;#启动/重启mysql

&ensp;&ensp;service mysqld start/restart

&ensp;&ensp;#关闭mysql

&ensp;&ensp;service mysqld stop

&ensp;&ensp;#查看运行状态

&ensp;&ensp;service mysqld status

4． 给root用户授权

直接授权

GRANT ALL PRIVILEGES ON *.* TO ‘root’@'%’ IDENTIFIED BY ‘youpassword’ WITH GRANT OPTION;

5．设置表不区分大小写

&ensp;&ensp;1、Linux下mysql安装完后是默认：区分表名的大小写，不区分列名的大小写；

&ensp;&ensp;2、用root帐号登录后，在/etc/my.cnf中的[mysqld]后添加添加lower_case_table_names=1，重启MYSQL服务，这时已设置成功：不区分表名的大小写；
lower_case_table_names参数详解：
lower_case_table_names=0
其中0：区分大小写，1：不区分大小写

###3.4安装Redis
1. 先到Redis官网(redis.io)下载redis安装包(以2.8.19为例)
2. 将其下载到/lamp目录下
3. 解压并进入其目录
 ![](https://i.imgur.com/fYApW1T.jpg)
4. 编译源程序
　　
  

    命令make 
　　cd src
　　make install PREFIX=/usr/local/redis

5. 将配置文件移动到redis目录
 ![](https://i.imgur.com/qlJJoop.jpg)
6. 启动redis服务
　![](https://i.imgur.com/hJXAyvn.jpg)　 
7. 默认情况，Redis不是在后台运行，我们需要把redis放在后台运行
　　vim /usr/local/redis/etc/redis.conf
　　将daemonize的值改为yes
 ![](https://i.imgur.com/uM7XryQ.jpg)
8. 让redis开机自启
　　vim /etc/rc.local
　　加入
　　/usr/local/redis/bin/redis-server /usr/local/redis/etc/redis-conf

###3.5安装Zookeeper
1．创建 /usr/local /zookeeper 文件夹：
    mkdir -p /usr/local /zookeeper
 
2．进入到 /usr/local/zookeeper 目录中：
    cd /usr/local/zookeeper
 
3．下载 zookeeper-3.4.10.tar.gz：
    wget https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/zookeeper-3.4.10/zookeeper-3.4.10.tar.gz
 
4．解压缩 zookeeper-3.4.10.tar.gz：
    tar -zxvf zookeeper-3.4.10.tar.gz
 
5、进入到 /usr/local/zookeeper/zookeeper-3.4.10/conf 目录中：
    cd zookeeper-3.4.10/conf/
 
6、复制 zoo_sample.cfg 文件的并命名为为 zoo.cfg：
    cp zoo_sample.cfg zoo.cfg

7、用 vim 打开 zoo.cfg 文件并修改其内容为如下：
    # The number of milliseconds of each tick
 
    # zookeeper 定义的基准时间间隔，单位：毫秒
    tickTime=2000
 
    # The number of ticks that the initial 
    # synchronization phase can take
    initLimit=10
    # The number of ticks that can pass between 
    # sending a request and getting an acknowledgement
    syncLimit=5
    # the directory where the snapshot is stored.
    # do not use /tmp for storage, /tmp here is just 
    # example sakes.
    # dataDir=/tmp/zookeeper
 
    # 数据文件夹
    dataDir=/usr/local/zookeeper/zookeeper-3.4.10/data
 
    # 日志文件夹
    dataLogDir=/usr/local/zookeeper/zookeeper-3.4.10/logs
 
    # the port at which the clients will connect
    # 客户端访问 zookeeper 的端口号
    clientPort=2181
 
    # the maximum number of client connections.
    # increase this if you need to handle more clients
    #maxClientCnxns=60
    #
    # Be sure to read the maintenance section of the 
    # administrator guide before turning on autopurge.
    #
    # http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
    #
    # The number of snapshots to retain in dataDir
    #autopurge.snapRetainCount=3
    # Purge task interval in hours
    # Set to "0" to disable auto purge feature
    #autopurge.purgeInterval=1

8、保存并关闭 zoo.cfg 文件:
    
9、进入到 /usr/local/zookeeper/zookeeper-3.4.10/bin 目录中：

    
cd ../bin/
 
10、用 vim 打开 /etc/ 目录下的配置文件 profile：
  
 vim /etc/profile

并在其尾部追加如下内容：
 
    # idea - zookeeper-3.4.10 config start - 2016-09-08
 
    export ZOOKEEPER_HOME=/usr/local/zookeeper/zookeeper-3.4.10/
    export PATH=$ZOOKEEPER_HOME/bin:$PATH
    export PATH
 
    # idea - zookeeper-3.4.10 config start - 2016-09-08
 
11、使 /etc/ 目录下的 profile 文件即可生效：
    
source /etc/profile

12、启动 zookeeper 服务：
   
 zkServer.sh start
    
如打印如下信息则表明启动成功：

    
ZooKeeper JMX enabled by default
    
Using config: /usr/local/zookeeper/zookeeper-3.4.10/bin/../conf/zoo.cfg
    
Starting zookeeper ... STARTED
 
13、查询 zookeeper 状态：
    
zkServer.sh status
 
14、关闭 zookeeper 服务：
    
zkServer.sh stop
   
 如打印如下信息则表明成功关闭：
    
ZooKeeper JMX enabled by default
    
Using config: /usr/local/zookeeper/zookeeper-3.4.10/bin/../conf/zoo.cfg
    
Stopping zookeeper ... STOPPED
 
15、重启 zookeeper 服务：
    
zkServer.sh restart
    
如打印如下信息则表明重启成功：
    
ZooKeeper JMX enabled by default
    
Using config: /usr/local/zookeeper/zookeeper-3.4.10/bin/../conf/zoo.cfg
    
ZooKeeper JMX enabled by default
    
Using config: /usr/local/zookeeper/zookeeper-3.4.10/bin/../conf/zoo.cfg
    
Stopping zookeeper ... STOPPED
    
ZooKeeper JMX enabled by default
    
Using config: /usr/local/zookeeper/zookeeper-3.4.10/bin/../conf/zoo.cfg
    
Starting zookeeper ... STARTED

###3.6安装Rabbitmq

安装依赖文件：

　　yum install gcc glibc-devel make ncurses-devel openssl-devel xmlto

1．Erlang

&ensp;&ensp;1.1下载

	
&ensp;&ensp;&ensp;&ensp;wget http://erlang.org/download/otp_src_19.0.tar.gz
	
&ensp;&ensp;&ensp;&ensp;tar -xzvf otp_src_19.0.tar.gz
	
	
&ensp;&ensp;1.2安装
		
&ensp;&ensp;&ensp;&ensp;./configure --prefix=/opt/erlang
		
&ensp;&ensp;&ensp;&ensp;;make && make install
	
&ensp;&ensp;1.3
		
&ensp;&ensp;&ensp;&ensp;配置Erlang环境变量：
		
&ensp;&ensp;&ensp;&ensp;/etc/profile.d目录下增加文件:
			
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;erlang.sh
			
              
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;增加下面的环境变量:
				
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;#set erlang environment
				
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;export PATH=$PATH:/opt/erlang/bin

&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;source /etc/profile
	
2． RabbitMq
	
&ensp;&ensp;下载
		
&ensp;&ensp;&ensp;&ensp;wget 

http://www.rabbitmq.com/releases/rabbitmq-server/v3.6.5/rabbitmq-server-generic-unix-3.6.5.tar.xz
	
&ensp;&ensp;解压
		
&ensp;&ensp;&ensp;&ensp;xz -d rabbitmq-server-generic-unix-3.6.5.tar.xz
		
&ensp;&ensp;&ensp;&ensp;tar -xvf rabbitmq-server-generic-unix-3.6.5.tar -C /opt
		
&ensp;&ensp;&ensp;&ensp;重命名为rabbitmq

	
&ensp;&ensp;配置rabbitmq环境变量：
		
&ensp;&ensp;&ensp;&ensp;/etc/profile.d目录下增加文件:
			
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;rabbitmq.sh
			
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;增加下面的环境变量:
				
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;#set rabbitmq environment
				
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;export PATH=$PATH:/opt/rabbitmq/sbin
			　		

&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;source  /etc/profile使得文件生效

3． RabbitMQ服务启动

&ensp;&ensp;3.1 启动：

 &ensp;&ensp;&ensp;&ensp;nohup rabbitmq-server -detached &
		
  &ensp;&ensp;&ensp;&ensp; #使用nohup命令来使得脱机或注销之后，Job依旧可以继续运行

&ensp;&ensp;3.2 查看服务状态：

&ensp;&ensp;&ensp;&ensp;rabbitmqctl status

&ensp;&ensp;3.3 关闭:

&ensp;&ensp;&ensp;&ensp;rabbitmqctl stop
		
&ensp;&ensp;启动应用

&ensp;&ensp;&ensp;&ensp;rabbitmqctl start_app

&ensp;&ensp;关闭应用

&ensp;&ensp;&ensp;&ensp;rabbitmqctl stop_app
	
&ensp;&ensp;查看所有队列信息:

&ensp;&ensp;&ensp;&ensp;rabbitmqctl list_queues
	
&ensp;&ensp;清除所有队列

&ensp;&ensp;&ensp;&ensp;rabbitmqctl reset


4.远程访问配置

&ensp;&ensp;添加用户:rabbitmqctl add_user admin admin

&ensp;&ensp;添加权限:rabbitmqctl set_permissions -p "/" admin ".*" ".*" ".*"

&ensp;&ensp;修改用户角色:rabbitmqctl set_user_tags admin administrator

&ensp;&ensp;重启---

&ensp;&ensp;访问:http://server-name:15672
&ensp;&ensp;用户密码:admin/admin

###3.7安装FastDFS
参照《FastDFS安装配置手册.docx》
##4.启动所有的中间件
启动数据库，Redis,  Zookeeper,  Rabbitmq, FastDFS等服务。
##5.数据库脚本初始化以及执行顺序
![](https://i.imgur.com/bi50XXf.jpg) 

1. 先执行ddl文件夹下的脚本。
1. 再执行dml文件夹下的脚本。
1. 最后再执行test文件夹下的脚本。

##6.授权说明（这里示例的licenseserver是采用了单独部署的方式）
解压 iuap-licenseserver 到 tomcat 的 webapps 下, 配置\webapps\iuap-licenseserver\WEB-INF\classes\application.properties 文件如下:
![](https://i.imgur.com/JdoXNWW.jpg)
修改redis和ZK的连接配置信息。另外，可以修改 tomcat的端口，保证端口没被占用即可。

通过执行 tomcat/bin/startup.bat 启动 licenseServer 服务器。

访问 http://ip地址:8088/iuap-licenseserver，用户名：admin,初始密码：admin（可自行修改）。
  ![](https://i.imgur.com/VDQQUks.jpg)

在上图中，点击“生成硬件锁”按钮，进入生成硬件锁界面，如下：
![](https://i.imgur.com/869HGxP.jpg)

在上图中，输入产品号，选择 licenseServer 所在的应用系统，点击“生成硬件锁”，会生成 hardkey.req文件。把 hardkey.req 发给用友云平台商务部，获得 license 授权许可文件 license.resp。

在 license 管理系统主界面中，点击“导入授权”按钮，上传 license.resp 文件，即可导入授权许可。
![](https://i.imgur.com/QSBwi3V.jpg)

##7.核心工程配置
上面章节提到这里的示例是licenseserver授权服务和核心的业务工程服务分开部署，授权服务上面已经介绍，这里主要说明核心工程的启动配置。核心工程如下图所示：

![](https://i.imgur.com/QSg5R0P.jpg)
 

每个war包的主要功能介绍如下：

- basedoc：人员档案节点，主要用来管理人员；
- cloud_print_service：云打印服务；
- eiap-plus：主要包含一些额外的功能服务，例如：按钮授权，在线用户等；
- example：示例结点服务；
- iuap-eiap-bpm-service：bmp流程服务；
- iuap-print：打印服务；
- iuap-saas-billcode-service：编码规则服务；
- iuap-saas-busilog-service：业务日志服务；
- iuap-saas-dispatch-service：调度任务服务；
- iuap-saas-filesystem-service：文件系统服务；
- iuap-saas-message-center：消息服务；
- print_service：打印服务；
- ubpm-web-process-center2，ubpm-web-rest， ubpm-webserver-process-center2：BMP流程定义服务；
- uitemplate_web：参照服务；
- uui：iuap应用平台的前端依赖代码；
- wbalone：提供iuap应用平台服务；

###7.1各个项目具体配置如下
####7.1.1wbalone项目配置
在webapps\wbalone\WEB-INF\classes下面打开application.properties文件修改数据库连接信息、redis连接信息、zookeeper连接信息以及加签文件信息（依据实际情况配置），如下图。
![](https://i.imgur.com/4ApMAgU.jpg) 
配置数据库，主要修改数据库 ip、端口、数据库名、用户名和密码。
![](https://i.imgur.com/NRECMYp.jpg) 
注意：此处配置的 redis 最好不要与其他系统共用。
 ![](https://i.imgur.com/N7EtDWV.jpg)

备注：此处填写的地址是 authfile.txt 放到服务器上的路径，其他组件也会用到此文件，需要配置此路径。
如果对接流程管理组件，则配置流程服务地址。
 ![](https://i.imgur.com/bb2JzzL.jpg)
打开authbac-sdk.properties文件，配置正确的加签服务地址。
 ![](https://i.imgur.com/lwLBayO.jpg)

打开iuap-licenseclient-conf.properties，配置licenseserver授权服务。
 ![](https://i.imgur.com/lzrLKgf.jpg)
打开workbench-sdk.properties文件配置工作台访问路径以及加签文件。
 ![](https://i.imgur.com/doB4PMO.jpg)
打开logback.xml文件配置日志路径。
 ![](https://i.imgur.com/HvByxFl.jpg)

####7.1.2其它项目配置
其它项目的配置类似。注意，有的服务的DB配置在jdbc.properties文件中。
##8.启动应用服务
查看文件 apache-tomcat-8.5.20\conf\server.xml 里的 8005,8080 端口，如果这两个端口已被占用，可修改为其他端口。
 ![](https://i.imgur.com/twGuZHx.jpg)
通过执行./apache-tomcat-8.5.20/bin/startup.sh 启动;

启动完毕后，登录地址：http://172.20.4.241:8080/wbalone

系统管理员：admin 默认密码：123456a

备注：请及时修改密码

双击./apache-tomcat-8.5.20/bin/shutdown.sh 停止;

或者通过 kill 命令杀掉 tomcat 的进程。 (kill -9 进程号)

##9.登录
在浏览器中输入：http://xxx.xxx.xxx.xxx:8080/wbalone，用户名/密码：admin/123456a

登录页
![](https://i.imgur.com/GNLrQyI.jpg)


主界面
![](https://i.imgur.com/ZMhD773.jpg)

![](https://i.imgur.com/rNGsS83.jpg)



