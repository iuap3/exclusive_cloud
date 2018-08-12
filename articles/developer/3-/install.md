# 安装向导
> 1.开发者中心, Kubernetes，Prometheus可按集群部署主机规划方案，在不同主机并行安装：<br/>
2.DNS需要docker环境支持，而DNS安装目前不包含docker安装过程，建议按推荐的主机规划，在开发者中心已经添加好的从节点上安装DNS服务；
  
## 3.1 开发者中心安装

### 3.1.1 cluster 模式安装

1.使用XShell等工具连接至规划的安装开发者中心主节点主机，将开发者中心（developercenter_enterprise.tar.gz）与镜像仓库（docker-registry-data.tar.gz）安两个装包拷贝至该主机某一目录下，如/data目录，并保证该磁盘有足够的剩余空间。

2.在安装包目录下，仅解压developercenter_enterprise.tar.gz安装包

解压缩文件的命令为:
```
tar -xvf developercenter_enterprise.tar.gz
```
 <div align=center>
 <img src="/articles/developer/3-/images/3-1.png"/>
 </div>
 <p align="center">图3-1</p>

3.进入解压后的目录，并执行安装命令
```
cd developercenter_enterprise
./install.sh
```
 <div align=center>
 <img src="/articles/developer/3-/images/3-2.png"/>
 </div>
 <p align="center">图3-2</p>

4.选择本机IP地址

如果本机仅有一个ip，则安装程序自动选择该ip，并直接进入之后的安装过程；如果本机有多个ip，则需要输入序号选择ip地址（请选择可以和其他主机或虚拟机相区分的内网IP地址）。
 <div align=center>
 <img src="/articles/developer/3-/images/3-3.png"/>
 </div>
 <p align="center">图3-3</p>
 
5.配置kubernetes，prometheus，dns服务的IP地址，IP地址可按安装规划填写。
>1)如果不计划使用VIP类型资源池，不安装kubernetes，可以填任意IP替代，建议填入一个规划中空闲的IP，可以在未来要启用VIP资源池，安装kubernetes时，直接在此IP对应的主机安装kubernetes，便于升级系统功能。<br/>
2)如果需要接入客户自建的DNS服务，则需要在此步骤填入客户自己的DNS服务IP
 <div align=center>
 <img src="/articles/developer/3-/images/3-4.png"/>
 </div>
 <p align="center">图3-4</p>
 
6.等待系统自动完成基础组件的安装

安装过程自动进行，涉及安装步骤包括：系统配置，如关闭SeLinux、配置nexus源等；安装docker环境，并启动镜像仓库、拉取所需镜像、启动并配置nginx、zookeeper、redis、数据库MySQL等。当出现如图3-4所示的“Congratulations! Developer Center Enterprise Edition has been installed successfully!”提示时，则表示基础组件安装完成。检查启动结果：红色代表未通过，绿色代表通过，黄色代表通过，但需引起注意。
 <div align=center>
 <img src="/articles/developer/3-/images/3-5.png"/>
 </div>
 <p align="center">图3-5</p>

### 3.1.2 standalone 模式安装
>standalone模式安装通常用于教学实验场景，满足教学演示的一般需求，其含义是将开发者中心各应用启动在一台机器上，不包含kubernetes、Prometheus、DNS和监控大盘等模块的安装，这些模块也不能安装在该节点上（因待安装组件端口冲突）
除以下两步操作与cluster模式不同之外，其余操作均相同。

1.当脚本执行至安装模式选择界面时，选择“2”，即选择standalone模式
 <div align=center>
 <img src="/articles/developer/3-/images/3-6.png"/>
 </div>
 <p align="center">图3-6</p>
 
2.参考附录1组件启动批次列表，在启动组件时修改“独立部署模式”标识为是的组件约束条件，使其只能运行在开发者中心主节点上，修改方法如下：

1）登录运维平台，进入“发布管理”菜单，选择待修改应用，例如confcenter
 <div align=center>
 <img src="/articles/developer/3-/images/3-7.png"/>
 </div>
 <p align="center">图3-7</p>
 
2）修改下图中的“UNLIKE”为“LIKE” 后保存发布该应用即可。
 <div align=center>
 <img src="/articles/developer/3-/images/3-8.png"/>
 </div>
 <p align="center">图3-8</p>
 
### 3.1.3 环境配置与启动

#### 3.1.2.1 licence授权

专属云服务默认试用期30天，若需要更长时间的试用期授权，需在使用前申请licence。
 <div align=center>
 <img src="/articles/developer/3-/images/3-9.png"/>
 </div>
 <p align="center">图3-9</p>
 
1）登录iuap-licenseserver地址：<label>http://开发者中心主节点ip:8882/iuap-licenseserver/login<label>，初始用户名密码：admin/admin，点击生成硬件锁，产品号填写任意自定义名称，仅勾选开发者中心主节点主机实际网卡IP对应条目，点击生成硬件锁，下载得到的文件备用
 <div align=center>
 <img src="/articles/developer/3-/images/3-10.png"/>
 </div>
 <p align="center">图3-10</p>
 
2）在udn申请授权，访问[http://udn.yyuap.com/license.php]，自行注册社区普通用户后登陆，选择uapv6，点击申请license，按要求填入信息，上传上一个步骤生成的硬件锁文件（上传文件需要浏览器支持flash插件，可以只输入必填项，但是务必正确填写可用的邮箱地址），点击申请后，邮箱会收到授权文件license.resp
 <div align=center>
 <img src="/articles/developer/3-/images/3-11.png"/>
 </div>
 <p align="center">图3-11</p>
 
3）登陆iuap-licenseserver导入授权文件，licence授权成功
 <div align=center>
 <img src="/articles/developer/3-/images/3-12.png"/>
 </div>
 <p align="center">图3-12</p>
 
#### 3.1.2.2 添加开发者中心从节点
>注意：此时添加的主机仅用来完整启动开发者中心平台，不能再作为资源池添加的主机来运行业务应用，建议至少两个从节点。

1）使用浏览器访问 <label>http://开发者中心主节点ip地址:8800</label>，登陆云运维平台后台，初始用户名：admin 密码：admin 。
 <div align=center>
 <img src="/articles/developer/3-/images/3-13.png"/>
 </div>
 <p align="center">图3-13</p>
 
2）点击菜单中的【增加节点】按钮。
 <div align=center>
 <img src="/articles/developer/3-/images/3-14.png"/>
 </div>
 <p align="center">图3-14</p>
 
3）输入待添加节点机的ip地址、root用户密码以及节点前缀，点击【+】按钮。节点前缀即节点名称，其内容可自定义。云运维平台支持批量添加节点机，只需按照页面内提示按照格式填入文本框中即可，但需注意尽量避免多节点同时安装，主节点压力过大可能导致节点安装失败！
 <div align=center>
 <img src="/articles/developer/3-/images/3-15.png"/>
 </div>
 <p align="center">图3-15</p>
 
4）下拉页面，点击【安装】按钮开始节点安装。
 <div align=center>
 <img src="/articles/developer/3-/images/3-16.png"/>
 </div>
 <p align="center">图3-16</p>
 
5）点击节点管理，可查看安装进度。节点机会自动进行安装docker、配置网络环境等操作。需手动刷新节点管理列表，更新安装进度，等待所有节点添加完成。
 <div align=center>
 <img src="/articles/developer/3-/images/3-17.png"/>
 </div>
 <p align="center">图3-17</p>

#### 3.1.2.3 发布平台服务

所有节点安装完成后，点击菜单中的【发布管理】,即可开始启动开发者中心各模块。

1）确认基础组件状态。在云运维平台中，各基础组件的状态已用颜色标识，如绿色表示该模块正常运行。点击该模块名称，可对其进行启动、关闭、重启等操作。
 <div align=center>
 <img src="/articles/developer/3-/images/3-18.png"/>
 </div>
 <p align="center">图3-18</p>

2）发布开发者中心各组件

开发者中心的各模块的部署有优先级区别，请参见《附录1：云运维平台-开发者中心各组件启动批次及说明》分批次发布开发者中心后台应用，某些发布系统中存在的模块，如不在附录1文档的发布批次中，不推荐发布。开发者中心可分为4批应用发布。请确保每一批次的应用部署成功后，再发布下一批次应用。当运行状态为running且设置的启动实例数与运行的实例数一致时，可以判定应用启动成功，如图3-19红框所示为启动成功状态。
 <div align=center>
 <img src="/articles/developer/3-/images/3-19.png"/>
 </div>
 <p align="center">图3-19</p>

#### 3.1.2.4 安装验证

1）点击菜单【容器管理-所有应用】，查看组件的发布运行状态。
 <div align=center>
 <img src="/articles/developer/3-/images/3-20.png"/>
 </div>
 <p align="center">图3-20</p>
 
2）点击选择dcee文件夹，即可查看所有开发者中心组件。
 <div align=center>
 <img src="/articles/developer/3-/images/3-21.png"/>
 </div>
 <p align="center">图3-21</p>
 
3）如果各组件都成功运行，则云运维平台启动完毕；如果有应用没有启动成功，则需要参考第5步安装异常问题处理对启动失败原因进行排查处理。
 <div align=center>
 <img src="/articles/developer/3-/images/3-22.png"/>
 </div>
 <p align="center">图3-22</p>
 
4）当应用全部启动成功时，在浏览器中输入<label>http://开发者中心主节点ip/portal</label> 即可访问安装完成的开发者中心。
 <div align=center>
 <img src="/articles/developer/3-/images/3-23.png"/>
        </div>
 <p align="center">图3-23</p>
 
## 3.2 Kubernetes安装

1）主节点安装

下载developercenter_kubernetes.tar.gz安装包，解压后运行目录下的main.sh，跟参数 本机ip 按提示安装即可
```
./main.sh 本机ip
```
 <div align=center>
 <img src="/articles/developer/3-/images/3-24.png"/>
        </div>
 <p align="center">图3-24</p>
 
2）安装验证

安装完毕后，可执行如下图命令确认，kubernetes安装是否成功。
```
kubectl get nodes
```
 <div align=center>
 <img src="/articles/developer/3-/images/3-25.png"/>
        </div>
 <p align="center">图3-25</p>
 
## 3.3 Prometheus安装

1）下载developercenter_monitor.tar.gz安装包，解压后运行目录下的main.sh  按提示安装即可，在提示选择需要部署的服务，输入序号时，初次安装选择0即可。
```
./main.sh
```
 <div align=center>
 <img src="/articles/developer/3-/images/3-26.png"/>
        </div>
 <p align="center">图3-26</p>
 
2）验证安装是否成功，可以在浏览器输入：<label>http://prometheus安装主机ip:9090</label>，正常打开一下页面即代表安装成功，Promethues监控的使用可参考其对应版本官方文档。
 <div align=center>
 <img src="/articles/developer/3-/images/3-27.png"/>
        </div>
 <p align="center">图3-27</p>
 
## 3.4 DNS（bind）安装
>注意：本dns需要依赖docker环境，因此要装在已经有docker环境的【开发者中心任意从节点机】或某【监控节点】上。

1）下载dns_install.tar.gz安装包，解压后运行目录下的install_dns.sh按提示安装即可
```
./install_dns.sh
```
 <div align=center>
 <img src="/articles/developer/3-/images/3-28.png"/>
        </div>
 <p align="center">图3-28</p>
 
2）验证DNS是否安装成功，如图所示执行以下两个命令，如ping命令输出的内容与安装提示输出的域名和ip对应信息一致（可自行调整ping命令的域名参数，使用安装提示输出的任意一个域名测试都可以），即可判断DNS服务正常。
```
sed -i '1inameserver 本机IP' /etc/resolv.conf
ping app.yyuap.com
```
 <div align=center>
 <img src="/articles/developer/3-/images/3-29.png"/>
        </div>
 <p align="center">图3-29</p>
 
3）	如果部署开发者中心的用户拥有自有的DNS服务，决定不使用开发者中心部署的DNS，则需要将以下DNS解析条目加入客户自己的DNS服务内。
```
app.yyuap.com=开发者中心主节点ip
yonyoucloud-k8s.com=ingress的IP地址
master.apiserver.k8s=k8s主节点ip
yonyoucloud.com=开发者中心主节点ip
private.registry.com=开发者中心主节点ip
private.kafka.com=开发者中心主节点ip
developer.yyuap.com=开发者中心主节点ip
mesos=开发者中心主节点ip
```
## 3.5 监控大盘服务安装

### 3.5.1 ycm-insight模块安装

1）在安装ycm-insight服务前确认，zookeeper，elasticsearch，kafka，Redis（以上服务默认在开发者中心主节点）和dns基础服务已经部署运行，可通过在相应主机终端执行命令确认，看输出结果是否包含四种中间件，检查命令：
```
docker ps | egrep -i \(kafka\|redis\|zookeeper\|elasticsearch\)
```
 <div align=center>
 <img src="/articles/developer/3-/images/3-30.png"/>
        </div>
 <p align="center">图3-30</p>
 
2）将ycm-insight服务安装包拷贝至规划安装节点，解压并进入解压目录内执行main.sh安装程序，按提示安装
```
./main.sh
```
 <div align=center>
 <img src="/articles/developer/3-/images/3-31.png"/>
        </div>
 <p align="center">图3-31</p>
 
3）配置开发者中心主节点Nginx

进入开发者中心主节点安装目录/script/tools下，如图3-27执行ycm_insight_config.sh脚本，按安装提示输入ycm-insight安装地址，脚本正常执行完毕即可。
 <div align=center>
 <img src="/articles/developer/3-/images/3-32.png"/>
        </div>
 <p align="center">图3-32</p>
 
### 3.5.2 ycm-agent安装

1）将ycm-agent服务安装包，下载至开发者中心主节点规划安装目录，确认主节点上Nginx容器日志映射到宿主机的目录（通常在：开发者中心安装目录/log_center/nginx/nginx下，也可以通过docker inspect容器ID 命令，查看日志文件在主机的映射位置）。 
 <div align=center>
 <img src="/articles/developer/3-/images/3-33.png"/>
        </div>
 <p align="center">图3-33</p>
 
 <div align=center>
 <img src="/articles/developer/3-/images/3-34.png"/>
        </div>
 <p align="center">图3-34</p>
 
 <div align=center>
 <img src="/articles/developer/3-/images/3-35.png"/>
        </div>
 <p align="center">图3-35</p>
 
2）解压ycm-agent服务安装包，在解压目录内执行main.sh安装程序，按提示安装，在提示输入Nginx服务app_acc.log输出路径时，如果第一步确认的日志路径与默认路径一致，可以直接回车选择默认路径。
```
./main.sh
```
 <div align=center>
 <img src="/articles/developer/3-/images/3-36.png"/>
        </div>
 <p align="center">图3-36</p>
 
### 3.5.3 验证监控大盘功能
>监控大盘监控的基本单元为部署在开发者中心的应用，验证步骤请在开发者中心成功部署应用后再执行。
	
1）确认数据收集链路是否正常

浏览器访问<label>http://elasticsearch主机ip(开发者中心主节点):9200/_plugin/head</label> 点击浏览数据，查看是否生成以iuap_年_月_日命名的数据文件，存在该数据文件即可认为数据收集链路正常
 <div align=center>
 <img src="/articles/developer/3-/images/3-37.png"/>
        </div>
 <p align="center">图3-37/p>
 
2）确认数据查询链路是否正常

待开发者中心正常启动后，登陆系统，在首页控制台页面下方和【Devops服务---监控大盘】功能下看到如下截图，即可认为数据查询链路功能正常，注意，首次查看大盘监控数据，可能由于所选应用没有访问数据，监控数据为0的状态，此时在坐标轴线应有蓝色值为0的标识点，否则查询数据应为异常状态。
 <div align=center>
 <img src="/articles/developer/3-/images/3-38.png"/>
        </div>
 <p align="center">图3-38/p>
 
## 附录1：云运维平台-开发者中心各组件启动批次及说明

 批次/序号 | 组件名称 | 独立安装模式 | 简介
 ---|---|---|---
 第一批次（默认已启动） |||
 1 | cas | 否 |单点登录器
 2 | tennantauth | 否 | 数据权限
 3 | user | 否 | 用户中心
 第二批次 |||
 4 | accesscenter | 否 | 签名认证
 5 | confcenter | 是 | 配置中心
 6 | fe | 是 | 前端展示页面
 7 | edna | 是 | 域名添加记录
 第三批次 |||
 8 | portal | 否 | 登录验证添加菜单以及应用中心
 9 | middleware | 否 | 中间件服务
 10 | app-upload | 是 | 应用持续集成
 11 | app-publish | 否 | 应用部署
 12 | app-manage | 是 | 应用管控后台
 第四批次 |||
 13 | app-manager-cron | 是 | 应用管理定时任务
 14 | ceryx_proxy | 否 | 统一接入服务
 15	| data-authority | 否 | 数据权限认证
 16 | md-cms | 是 | 公有镜像仓库管理/镜像文件信息同步
 17 | res-pool-api | 是 | 资源池API
 18 | res-pool-manager | 是  |资源池管理
 19 | runtime-log | 是 | 应用运行日志
 20 | web-terminal | 是 | 控制台
 21 | sms | 是 | 短信邮件服务
 22 | res-alarm-center | 是 | 资源报警中心
 23 | change-dog | 是 | 变更大盘
 24 | auth_server | 是 | 镜像仓库登陆认证服务
 25 | app-approve | 是 | 友互通适配接入
 26 | dbtask-app | 是 | 流水线定时部署
 27 | dbtask-exam | 是 | 流水线发布审批状态检查
 28 | app-apply | 是 | 计算资源审批管理
 29 | app-api | 是 | 开发者中心对外接口
 30 | eos-console | 是 | 异步调用控制台
 31 | eos-mq-auth | 是 | 异步调用认证服务
 32 | msconsole | 是 | 微服务控制台
 33 | registry | 是 | 微服务注册中心
 34 | servmeta | 否 | 微服务元数据服务
 35 | netpolicy | 是 | 资源池隔离
 36 | timer | 是 | 微服务日志计算任务
 37 | data-log | 是 | Kubernetes日志处理
 38 | zipkin-kafka | 是 | 微服务日志同步服务，未提供健康检查接口，<br/>在marathon上呈现为unknown（灰色）状态，属正常
 