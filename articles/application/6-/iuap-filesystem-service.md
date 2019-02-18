# 文件服务

##开发示例

使用文件服务进行开发，只需要根据实际情况修改文件服务器配置信息文件，前端直接调用相关接口即可。

1.文件服务器信息配置

![](/articles/iuap-develop/10-/iuap-filesystem-service/3.5.5-RELEASE/img/4.png)

storeType有AliOss，Local和FastDfs三种值，分别对应阿里云，本地文件存储和分布式文件系统。

1.1 本地文件存储配置

![](/articles/iuap-develop/10-/iuap-filesystem-service/3.5.5-RELEASE/img/5.png)


storeDir用来配置文件在服务器上的存储路径，local\_server配置外部访问文件服务器的url。

1.2 AliOss阿里云

![](/articles/iuap-develop/10-/iuap-filesystem-service/3.5.5-RELEASE/img/6.png)


endpoint用来指定接收消息的终端地址，accessKeyId是阿里云的账号，accessKeySecret是阿里云账号的密码。callbackTarget是接受阿里云上传回调的主机ip和端口，该主机需要外网能直接访问（直传功能）。callbackUrl处理上传回调的servlet截取的地址，直传成功后阿里云oss将会对上面配置的主机地址发送url为改配置url的post请求（直传功能）。callbackBody需要上传回调所带的参数，上面配置的的请求将会携带callbackBody配置的参数（直传功能）。

1.3 FastDfs

![](/articles/iuap-develop/10-/iuap-filesystem-service/3.5.5-RELEASE/img/7.png)


connect\_timeout用来设置连接超时时间，network\_timeout用来设置网络超时时间。charset是字符编码，tracker\_server用来设置文件服务器的地址，fdfsread\_server是文件系统的nginx服务器ip。

2. 数据库表PUB\_FILESYSTEM

| 字段名 | 注释 |
| --- | --- |
| ID | 主键 |
| PKFILE | 文件在文件服务器上的ID |
| FILENAME | 文件名 |
| FILESIZE | 文件大小 |
| GROUPNAME | 分组名 |
| PERMISSION | oss权限(read，private，full) |
| UPLOADER | 上传用户的id |
| UPLOADTIME | 上传时间 |
| SYSID | 系统id |
| TENANT | 租户id |
| MODULAR | 模块 |
| URL | 文件访问的url |
| SOURCETENANT | 源租户 |
| SECRETKEY | 加密密钥 |
| FILEPATH | 单据相关的唯一标示，一般包含单据ID，如果有多个附件的时候由业务自己制定规则 |

分组策略：文件一般根据所属的功能模块进行分组，groupname就是分组名，比如一个文件属于督办任务模块，它的groupname就是ygdemo。在督办任务下又有很多单据，那么再根据filepath确定文件属于哪个单据。同一个单据下的文件的filepath是一样的。
