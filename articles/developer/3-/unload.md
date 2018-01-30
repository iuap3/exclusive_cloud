# 卸载及常见问题

## 卸载
如果要卸载 iuap 云运维平台，最简单的方式是直接销毁虚拟机。
如果想在保留虚拟机的情况下，卸载 iuap 云运维平台，可按照如下命令进行操作：
1、停止docker
```
systemctl stop docker
```

2、删除安装目录
```
rm -rf /data/developercenter_enterprise
```

3、卸载docker
```
yum remove -y docker
```

## 常见问题

#### 卸载时报错 “Device or resource basy”
删除 docker 安装包时，提示如下：
<div align=center>
<img src="/articles/developer/3-/images/30.png"/>
</div>
<p align="center">图 1</p>

停止docker进程，再次执行删除操作即可。

