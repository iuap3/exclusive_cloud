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





2. 配置目的（功能点目标）