# 独立配置项
   
## 5.1 配置应用域名后缀

开发者中心为部署在平台的应用默认生成appid.*.app.yyuap.com格式的内网访问域名，如客户期望调整*.app.yyuap.com后缀为自定义后缀，例如*.private.dctest.com，需要完成以下配置步骤（注意：配置生效后原先发布的应用将无法访问，需要重新在平台发布生成新配置的域名后缀）：

1）在开发者中心控制台（开发者中心主节点IP:8800）从【容器管理】---【所有应用】dcee目录下，找到edna应用，如下图所示，选择日期最近的一次配置，修改domain.suffix环境变量值为：*.private.dctest.com，点击【确定】即可重启edna容器使配置生效，当再次在开发者中心部署应用后，将生成配置后缀的内网访问域名;
 <div align=center>
 <img src="/articles/developer/3-/images/5-1.png"/>
        </div>
 <p align="center">图5-1</p>
 
2）修改Nginx配置，重启Nginx容器。进入 开发者中心主节点安装目录/config_center/nginx 目录下upstream-dev.conf配置文件中如图5-2所示位置配置，在开发者中心执行docker restart nginx 命令重启容器;
 <div align=center>
 <img src="/articles/developer/3-/images/5-2.png"/>
        </div>
 <p align="center">图5-2</p>
 
3）在完成上述配置后，在平台再次发布的应用，在应用控制台可以看到如图5-3所示的应用内网访问域名;
 <div align=center>
 <img src="/articles/developer/3-/images/5-3.png"/>
        </div>
 <p align="center">图5-3</p>
 
### 5.2 微服务异步调用MQ部署方案

服务治理平台提供异步调用的SDK，其运行依赖RabbitMQ中间件，MQ中间件的权限认证服务需配置成开发者中心专属云部署的eos-mq-auth对应的服务地址。针对专属云版本，微服务治理平台提供两种MQ私有化的部署方案

1.用户在服务器自主部署集群（或单节点）的方式提供RabbitMQ服务

1)按照官方文档安装RabbitMQ的单机或者镜像集群；

2)修改/etc/rabbitmq/enabled\_plugins文件,启用http认证；
```
[rabbitmq_management,rabbitmq_auth_backend_http]
```
3)修改/etc/rabbitmq/rabbitmq.conf配置文件,配置开发者中心提供的mq认证服务地址。专属云环境，需将developer.yonyoucloud.com替换成开发者中心主节点IP;
```
loopback_users.guest=false
listeners.tcp.default=5672
hipe_compile=true
management.listener.port=15672
management.listener.ssl=false
		
auth_backends.1=http
auth_http.user_path=http://developer.yonyoucloud.com/eos-mq-auth/auth/user
auth_http.vhost_path=http://developer.yonyoucloud.com/eos-mq-auth/auth/vhost
auth_http.resource_path=http://developer.yonyoucloud.com/eos-mq-auth/auth/resource
auth_http.topic_path=http://developer.yonyoucloud.com/eos-mq-auth/auth/topic
```
2.使用开发者中心微服务治理平台提供的Docker镜像，在任意一台可以访问开发者中心主节点，且安装docker的服务器上启动一个RabbitMQ单节点实例。

启动实例命令为：
```
docker run -d \
--name rabbitmq_eos \
-p 15672:15672 \
-p 5672:5672 \
-v /data/developercenter_enterprise/data_center/eos-mq:/var/lib/rabbitmq/data \
-e RABBITMQ_NODENAME=rabbitmq_eos \
-e RABBITMQ_AUTH_SERVER=开发者中心微服务治理平台eos-mq-auth服务地址 \
开发者中心镜像仓库/yonyoucloud-middleware/eos-rabbitmq:async
```
>专属云环境，需要将“开发者中心镜像仓库”替换成开发者中心主节点IP:5000；“开发者中心微服务治理平台eos-mq-auth服务地址”替换为开发者中心主节点IP。

### 5.3 公网域名绑定应用方案

开发者中心专属云部署于客户内网，客户应用在开发者中心部署后，需要将应用作为公网应用对外服务，并采用公网域名作为访问入口时，可采取如图5-4所示两个网络拓扑方案进行配置：

1.客户有自建的反向代理服务：

1）客户反向代理服务将公网域名的请求代理到开发者中心的反向代理服务（nginx）80端口（通常为开发者中心主节点IP：80）；

2）开发者中心应用域名绑定（图5-5），选择【容器服务】---【应用管理】点击产品，产品线，应用页签进入应用控制台，点击【域名】页签，点击【域名绑定】，勾选自有域名，然后填入应用对应的公网域名，确定即可；

3）客户也可以绕过开发者中心的反向代理服务（nginx）和统一接入服务（crexy），将客户反向代理服务的公网域名请求直接负载到开发者中心负载均衡服务（marathon-lb）或上，虽然这样可以实现通过公网域名访问应用的目的，但同时也绕过了应用的访问监控等平台服务，不利于应用的管理工作，所以不推荐；
 <div align=center>
 <img src="/articles/developer/3-/images/5-4.png"/>
        </div>
 <p align="center">图5-4</p>
 
2.客户没有自建的反向代理服务器：
1）将开发者中心统一接入服务单独部署到公网入口服务器（公网DNS服务解析域名后指向的主机）作为反向代理服务，成为公网访问请求的入口；

2）如方案1第二步，在开发者中心进行域名绑定操作；
 <div align=center>
 <img src="/articles/developer/3-/images/5-5.png"/>
        </div>
 <p align="center">图5-5</p>
 
### 5.4 邮件服务配置

为开发者中心配置邮件服务，首先需要用户提供开发者中心可以访问的邮件服务器（IP和端口）、用于发送邮件的邮箱账户和密码，其次依据提供的信息分别修改user和cas两个应用的同名配置文件mail.properties，重启user和cas即可启用启用邮件服务。
 <div align=center>
 <img src="/articles/developer/3-/images/5-6.png"/>
        </div>
 <p align="center">图5-6</p>
 
具体配置步骤如下：

1）分别进入 开发者中心主节点安装目录/config_center/dcee/user和cas目录下，找到mail.properties文件，内容如图3-20所示；

2）修改smtphost的值为邮件服务器的IP地址（不要写域名，容器内无法解析）；

3）修改password值为发件邮箱密文形式密码，密文形式密码由友互通对明文密码经加密算法生成，在配置密码前请联系友互通索取密文形式密码；

4）修改sender的值为发件邮箱地址，subject可自行定义，username的值为发件邮箱前缀；

5）保存上述修改后，登录到运维平台控制台（开发者中心主节点ip:8800），【容器管理】---【所有应用】，在dcee目录下找到user和cas，分别重启应用（如图3-21）后即可使用邮件发送功能;
 <div align=center>
 <img src="/articles/developer/3-/images/5-7.png"/>
        </div>
 <p align="center">图5-7</p>