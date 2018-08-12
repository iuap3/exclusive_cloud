# 安装前准备

## 2.1 安装盘清单

文件 | 说明
---|---
developercenter_enterprise.tar.gz | 开发者中心软件安装包
docker-registry-data.tar.gz | 开发者中心镜像仓库文件包
developercenter_kubernetes.tar.gz | kubernetes软件安装包
developercenter_monitor.tar.gz | Prometheus监控软件安装包
dns_install.tar.gz | DNS服务安装包
developercenter_ycm_insight.tar.gz | 监控大盘功能服务组件安装包
developercenter_ycm_agent.tar.gz | 监控大盘功能Nginx日志数据收集探针安装包
iuap云运维平台V3.5.1安装指南.pdf | 软件安装指南
iuap云运维平台V3.5.1使用手册.pdf | 软件使用手册

## 2.2 主机配置要求
> iuap 云运维平台 V3.5.1（开发者中心企业版）的安装方式分为集群模式与完整安装模式，推荐配置如下:

配置项 | 集群模式推荐配置 | 完整安装模式推荐配置
---|---|---
机器数量 | 6+2 | 1
内存 | 16G 及以上 | 80G 及以上
CPU  | 8核，2.4G Hz|40核，2.4G Hz
硬盘 | 主节点，500GB及以上<br/>节点机，200GB及以上|1TB及以上
网卡 | 1000Mb 及以上 | 1000Mb 及以上
操作系统 | CentOS 7 及以上 | CentOS 7 及以上

## 2.3 集群安装主机规划（推荐）

主机（示例IP） | 部署内容
---|---
172.20.23.236 | 开发者中心主节点 监控大盘数据收集探针
172.20.23.237 | 开发者中心从节点	
172.20.23.238 | 开发者中心从节点	DNS
172.20.23.239 | Kubernetes 主节点
172.20.23.240 | Prometheus监控
172.20.23.241 | 监控大盘服务端组件
172.20.23.242 | 普通资源池接入主机资源
172.20.23.243 | VIP资源池接入主机资源

## 2.4 待装主机检查
1.检查操作系统版本
> 安装开发者中心的主机，建议操作系统版本为：CentOS 7.4.1708，可通过如下命令对待装主机进行发行版本与Linux内核版本检查。

发行版本检查命令：
```
cat /etc/redhat-release
```
 <div align=center>
 <img src="/articles/developer/3-/images/2-1.png"/>
 </div>
 <p align="center">图2-1</p>
								
Linux内核检查命令：
```
uname -sr
```
 <div align=center>
 <img src="/articles/developer/3-/images/2-2.png"/>
 </div>
 <p align="center">图2-2</p>
 					
2.检查磁盘空间
查看磁盘的挂载情况，需要将存储盘挂到/data目录下，并根据实际挂载情况检查挂载磁盘的格式，需为xfs格式。

磁盘挂载情况检查命令：
```
df –h
```
 <div align=center>
 <img src="/articles/developer/3-/images/2-3.png"/>
 </div>
 <p align="center">图2-3</p>

3.检查CPU
执行如下命令，检查CPU，重点关注CPU（s）核心数，是否符合待装主机配置需求
```
lscpu
```
 <div align=center>
 <img src="/articles/developer/3-/images/2-4.png"/>
 </div>
 <p align="center">图2-4</p>
