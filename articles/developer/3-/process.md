# 安装流程

## 安装清单
> 安装开发者中心企业版时所需的安装包和文件如下表所示：

文件|说明
---|---
developercenter_enterprise.tar.gz|软件安装包
docker-registry-data.tar.gz|镜像仓库文件包
iuap 云运维平台 V3.5 安装指南.pdf|软件安装指南
iuap 云运维平台 V3.5 使用手册.pdf|软件使用手册

## 安装流程

### 集群模式安装

1、按照要求准备运行环境，使用 XShell 等工具连接至需要安装云运维平台的机器。

2、将软件安装包和镜像仓库文件包上传至该机器的某一目录下，如 `/data` 目录，并确保该目录所在磁盘有充足的空间，至少100G。
```
/data/developercenter_enterprise.tar.gz
/data/docker-registry.tar.gz
```

3、进入 `/data` 目录下，解压软件安装包，解压缩的命令为：
```
cd /data
tar -xf developercenter_enterprise.tar.gz
```
<div align=center>
<img src="/articles/developer/3-/images/1.png"/>
</div>
<p align="center">图 1</p>

4、进入解压后的目录，并执行安装命令：
```
cd developercenter_enterprise
./install.sh
```
<div align=center>
<img src="/articles/developer/3-/images/2.png"/>
</div>
<p align="center">图 2</p>

5、选择安装模式，根据配置情况选择相应的安装模式，集群安装模式（cluster）的序号为“1”。
<div align=center>
<img src="/articles/developer/3-/images/3.png"/>
</div>
<p align="center">图 3</p>

6、选择本机IP地址，如果本机仅有一个ip，则安装程序自动选择该ip，并直接进入之后的安装过程；如果本机有多个ip，则需要输入序号选择ip地址（请选择可以和其他主机或虚拟机相区分的内网IP地址）。
<div align=center>
<img src="/articles/developer/3-/images/4.png"/>
</div>
<p align="center">图 4</p>

7、等待系统自动完成基础组件的安装。安装程序自动进行的安装步骤包括：系统配置，如关闭SeLinux、配置nexus源等；安装docker环境，并启动镜像仓库、拉取所需镜像、启动并配置nginx、zookeeper、redis、数据库MySQL等。当出现如图3-5所示的“Congratulations! Developer Center Enterprise Edition has been installed successfully!”提示时，则表示基础组件安装完成。
<div align=center>
<img src="/articles/developer/3-/images/5.png"/>
</div>
<p align="center">图 5</p>

### 完整模式安装

完整安装模式下的安装过程与集群安装模式基本相同，区别是在初始选择安装模式时，选择第二项即可，如下图所示。
<div align=center>
<img src="/articles/developer/3-/images/29.png"/>
</div>
<p align="center">图 29</p>

当云运维平台基础组件安装完毕后，会启动云运维平台-开发者中心的所有组件。

注意：在完整安装模式下，所有运维平台及开发者中心组件均运行在同一主机或虚拟机下。若该主机或虚拟机出现故障，则可能导致云运维平台完全不可用！故该模式仅为快速体验云运维平台-开发者中心使用，请勿应用于生产环境。


## 启动云运维平台后端管理系统

1、访问并登陆云运维平台后台管理系统，使用浏览器访问 http://管控机ip地址:880，用户名：admin 密码：admin。登陆后如图6所示。
<div align=center>
<img src="/articles/developer/3-/images/6.png"/>
</div>
<p align="center">图 6</p>

2、增加云运维平台启动组件用主机或虚拟机。注意，此时添加的主机或虚拟机仅用来启动云运维平台系统，不用来运行业务应用。点击菜单中的【增加节点】按钮。
<div align=center>
<img src="/articles/developer/3-/images/7.png"/>
</div>
<p align="center">图 7</p>

3、输入节点机IP地址等信息，添加节点机。输入待添加节点机的ip地址、root用户密码以及节点前缀。节点前缀即节点名称，其内容可自定义。输入完成后，点击右边的加号添加主机。云运维平台支持批量添加节点机，只需按照页面内提示按照格式填入文本框中即可。
<div align=center>
<img src="/articles/developer/3-/images/8.png"/>
</div>
<p align="center">图 8</p>
下拉页面，点击安装按钮开始节点安装。
<div align=center>
<img src="/articles/developer/3-/images/9.png"/>
</div>
<p align="center">图 9</p>

4、等待所有节点添加完成。点击节点管理，可查看安装进度。节点机会自动进行安装docker、配置网络环境等操作。请等待所有节点安装完成。
<div align=center>
<img src="/articles/developer/3-/images/10.png"/>
</div>
<p align="center">图 10</p>

当所有节点安装完成后，点击菜单中的发布管理。如果基础组件均正常运行，即可开始启动开发者中心各模块。

在云运维平台中，各基础组件的状态已用颜色标识，如绿色表示该模块正常运行。点击该模块名称，可对其进行启动、关闭、重启等操作。
<div align=center>
<img src="/articles/developer/3-/images/11.png"/>
</div>
<p align="center">图 11</p>

5、发布开发者中心各组件。按照下图中【应用发布】按钮下方所示的优先级，分批次批量发布开发者中心后台应用。开发者中心可分为4批应用发布。请确保每一批次的应用部署成功后，再发布下一批次应用。
<div align=center>
<img src="/articles/developer/3-/images/12.png"/>
</div>
<p align="center">图 12</p>

开发者中心的各模块的部署优先级及其作用，请参见《附录1：云运维平台-开发者中心各组件启动批次及说明》。

6、在启动portal和middleware容器前，需要登录配置中心进行相应配置。使用火狐浏览器，在浏览	器地址栏中输入http://管控机ip地址/confcenter/api/login，使用用户名/密码：admin/huaweidev，登	录进去。
<div align=center>
<img src="/articles/developer/3-/images/13.png"/>
</div>
<p align="center">图 13</p>

点击左侧菜单“配置管理”。
<div align=center>
<img src="/articles/developer/3-/images/14.png"/>
</div>
<p align="center">图 14</p>

输入“portal”，选择“开发环境”，版本选择“1_0_0_0”。
<div align=center>
<img src="/articles/developer/3-/images/15.png"/>
</div>
<p align="center">图 15</p>

可以看到portal应用的配置文件。
<div align=center>
<img src="/articles/developer/3-/images/16.png"/>
</div>
<p align="center">图 16</p>

此处需要修改application.properties和sdk.properties，点击application.properties所在行的操作按钮。
<div align=center>
<img src="/articles/developer/3-/images/17.png"/>
</div>
<p align="center">图 17</p>

点击“请选择上传方式”所在输入框。
<div align=center>
<img src="/articles/developer/3-/images/18.png"/>
</div>
<p align="center">图 18</p>

选择“输入文本”。
<div align=center>
<img src="/articles/developer/3-/images/19.png"/>
</div>
<p align="center">图 19</p>

进入管控机上开发者中心安装目录中如下路径。
<div align=center>
<img src="/articles/developer/3-/images/20.png"/>
</div>
<p align="center">图 20</p>

将application.properties文件的内容复制到如下文本框。
<div align=center>
<img src="/articles/developer/3-/images/21.png"/>
</div>
<p align="center">图 21</p>

点击右上角的“保存”按钮，保存修改后的文件。
<div align=center>
<img src="/articles/developer/3-/images/22.png"/>
</div>
<p align="center">图 22</p>

用同样的方法修改portal的sdk.properties。

用同样的方法修改middleware的配置文件，环境选择“开发环境”，版本为：1_0_0_0，要修改的配置	文件为：application.properties，disconf.properties，sdk.properties。
<div align=center>
<img src="/articles/developer/3-/images/23.png"/>
</div>
<p align="center">图 23</p>

middleware的配置文件在管控机上的路径为：
<div align=center>
<img src="/articles/developer/3-/images/24.png"/>
</div>
<p align="center">图 24</p>

修改、保存好上述文件文件后，启动portal和middleware容器。

7、查看各组件的运行状态。点击菜单【容器管理】，然后点击“所有应用”，查看组件的发布情况。
<div align=center>
<img src="/articles/developer/3-/images/25.png"/>
</div>
<p align="center">图 25</p>

8、点击选择dcee文件夹，即可查看所有开发者中心组件。
<div align=center>
<img src="/articles/developer/3-/images/26.png"/>
</div>
<p align="center">图 26</p>
如果各组件都成功运行，则云运维平台启动完毕；如果有应用没有启动成功，则需要对启动失败原因进行排查。
<div align=center>
<img src="/articles/developer/3-/images/27.png"/>
</div>
<p align="center">图 27</p>

9、当应用全部启动成功时，在浏览器中输入管控机ip地址即可访问刚刚安装好的云运维平台-开发者中心。
<div align=center>
<img src="/articles/developer/3-/images/28.png"/>
</div>
<p align="center">图 28</p>

