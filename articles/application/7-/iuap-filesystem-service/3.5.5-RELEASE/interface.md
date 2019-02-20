## 开放接口规范

1.附件上传

请求体

| URl | /iuap-saas-filesystem-service/file/upload |
| --- | --- |
| Method | POST |

常用请求参数：

参数以form格式提交

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| filepath | String | 是 | 单据相关的唯一标示，一般包含单据ID，如果有多个附件的时候由业务自己制定规则 |
| groupname | String | 是 | 分组名 |
| permission | String | 否 | Oss权限(read，private，full),read是可读=公有，private=私有，当这个参数不传的时候会默认private |
| url | String | 否 | 里传true或false。为true，则返回附件的连接地址存到数据库中；如果isencrypt设置为true，url不能设置为true否则不能上传，提示：对于加密文件不能返回url，返回了也无法访问！ |
| thumbnail | String | 否 | 缩略图大小，非图片不传；&quot;500w&quot;,//缩略图--可调节大小，和url参数配合使用，不会存储到数据库 |
| isreplace | boolean | 否 | 当filepath groupname filename 都一致的时候是否覆盖上传附件，默认false，不覆盖 |
| isencrypt | boolean | 否 | 是否加密，默认false不加密 |

正确返回值：

{

&quot;data&quot;:[{

&quot;filename&quot;:&quot;attachTest.txt&quot;,

&quot;filepath&quot;:&quot;2dec1b12-e590-456c-8f48-2aef9bd75699&quot;,

&quot;id&quot;:&quot;A74ha7G812VedRgzzbQ&quot;,

&quot;filesize&quot;:&quot;12Byte(s)&quot;

,&quot;groupname&quot;:&quot;ygdemo&quot;,

&quot;url&quot;:&quot;http://172.20.53.144:8080/wbalone/images/tenant/4823b949-fe3c-4        b2e-b8c4-151bd9867f93.txt&quot;,

&quot;uploadtime&quot;:&quot;2018-10-29 15:29:01&quot;

}],

&quot;message&quot;:&quot;上传成功&quot;,

&quot;status&quot;:1

}

异常返回：

{

data:&quot;&quot;,

message: &quot;上传失败{jca.bat}附件已经存在不能重复上传&quot;

status: 0

}

参数说明：

 data：返回的数据

filename：附件名

filepath:附件路径

id：附件id

filesize：附件大小

groupname:附件分组名

url：附件的存储地址

uploadtime：附件上传时间

message：返回的信息

status：状态码，1表示成功，0表示失败

2.附件删除

请求体

| URl | /iuap-saas-filesystem-service/file/delete |
| --- | --- |
| Method | GET |

请求参数

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| id | String | 是 | 附件id |

正确返回值：

{&quot;message&quot;:&quot;删除成功&quot;,&quot;status&quot;:1}

异常返回值：

{&quot;message&quot;:&quot;没有找到附件信息&quot;,&quot;status&quot;:0}

参数说明：

message：返回的信息

status：状态码，1表示成功，0表示失败

3.根据IDS批量删除

请求体

| URl | /iuap-saas-filesystem-service/file/batchDeleteByIds |
| --- | --- |
| Method | GET |

请求参数

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| ids | String[] | 是 | 附件id数组 |

正确返回值：

{&quot;message&quot;:&quot;批量删除成功&quot;,&quot;status&quot;:1}

异常返回值：

{&quot;message&quot;:&quot;附件批量删除出现异常！&quot;,&quot;status&quot;:0}

参数说明：

message：返回的信息

status：状态码，1表示成功，0表示失败



4.附件更新

请求体

| URl | /iuap-saas-filesystem-service/file/update |
| --- | --- |
| Method | POST |



请求参数

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| id | String | 是 | 附件id |
| pkfile | String | 否 | 附件在附件服务器上的id |
| filename | String | 否 | 附件名 |
| filepath | String | 否 | 单据相关的唯一标示，一般包含单据ID，如果有多个附件的时候由业务自己制定规则 |
| groupname | String | 否 | 分组名 |
| permission | String | 否 | Oss权限(read，private，full),read是可读=公有，private=私有，当这个参数不传的时候会默认private |
| uploader | String | 否 | 附件上传者 |
| uploadTime | String | 否 | 上传时间 |
| sysid | String | 否 | 系统id |
| tenant | String | 否 | 租户id |
| modular | String | 否 | 模块 |
| url | String | 否 | 完整路径 |
| sourcetenant | String | 否 | 源租户 |
| secretkey | String | 否 | 加密秘钥 |
| filesize | String | 否 | 文件大小 |

正确返回值：

{&quot;message&quot;:&quot;更新成功&quot;,&quot;status&quot;:1,&quot;data&quot;:{}}

错误返回值：

{&quot;message&quot;:&quot;更新失败&quot;,&quot;status&quot;:0,&quot;data&quot;:{}}

参数说明：

message：返回的信息

status：状态码，1表示成功，0表示失败

data：更新返回之后数据

5.批量更新附件路径

请求体

| URl | /iuap-saas-filesystem-service/file/updateBatch |
| --- | --- |
| Method | POST |

请求参数

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| fileIDs | String[] | 是 | 附件id数组 |
| filepath | String | 是 | 单据相关的唯一标示，一般包含单据ID，如果有多个附件的时候由业务自己制定规则 |
| tenant | String | 否 | 租户id，不写会自动从系统获取 |

正确返回值：

{&quot;message&quot;:&quot;更新成功&quot;,&quot;status&quot;:1,data:1}

错误返回值：

{&quot;message&quot;:&quot;更新失败&quot;,&quot;status&quot;:0,data:0}

参数说明：

message：返回的信息

status：状态码，1表示成功，0表示失败

data返回批量更新成功记录数

6.附件查询

请求体

| URl | /iuap-saas-filesystem-service/file/query |
| --- | --- |
| Method | GET |

请求参数

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| filepath | String | 是 | 单据相关的唯一标示，一般包含单据ID，如果有多个附件的时候由业务自己制定规则 |
| groupname | String | 否 | 分组名 |
| tenant | String | 否 | 租户id，不写会自动从系统获取 |

正确返回值

{

&quot;data&quot;:[{

&quot;filename&quot;:&quot;attachTest.txt&quot;,

&quot;filepath&quot;:&quot;a58cd83c-8a41-4dd2-8b20-761ace347e1d&quot;,

&quot;id&quot;:&quot;qh0jxuMZQZShnl5BxaC&quot;,

&quot;filesize&quot;:&quot;12Byte(s)&quot;,

&quot;groupname&quot;:&quot;ygdemo&quot;,

&quot;url&quot;:&quot;http://172.20.53.144:8080/wbalone/images/tenant/df735b45-e3d1-4403-878a-9f41fa490d9a.txt&quot;,

&quot;uploadtime&quot;:&quot;2018-10-29 15:43:31&quot;

}],

&quot;message&quot;:&quot;查询成功&quot;,

&quot;status&quot;:1

}

错误返回值

{&quot;message&quot;:&quot;没有查询到相关数据&quot;,&quot;status&quot;:0,data:&quot;&quot;}

参数说明：

data：返回的数据

filename：附件名

filepath:附件路径

id：附件id

filesize：附件大小

groupname:附件分组名

url：附件的存储地址

uploadtime：附件上传时间

message：返回的信息

status：状态码，1表示成功，0表示失败

7.替换附件

请求体

| URl | /iuap-saas-filesystem-service/file/replace |
| --- | --- |
| Method | POST |

请求参数

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| id | String | 是 | 附件id |
| filepath | String | 否 | 单据相关的唯一标示，一般包含单据ID，如果有多个附件的时候由业务自己制定规则 |
| groupname | String | 否 | 分组名 |
| permission | String | 否 | Oss权限(read，private，full),read是可读=公有，private=私有，当这个参数不传的时候会默认private |
| url | String | 否 | 这里传true或false。为true，则返回附件的连接地址存到数据库中；如果isencrypt设置为true，url不能设置为true否则不能上传，提示：对于加密文件不能返回url，返回了也无法访问！ |
| thumbnail | String | 否 | 缩略图大小，非图片不传；&quot;500w&quot;,//缩略图--可调节大小，和url参数配合使用，不会存储到数据库 |
| isencrypt | boolean | 否 | 是否加密，默认false不加密 |

正确返回值：

{

&quot;data&quot;:[{

&quot;filename&quot;:&quot;attachTest.txt&quot;,

&quot;filepath&quot;:&quot;2dec1b12-e590-456c-8f48-2aef9bd75699&quot;,

&quot;id&quot;:&quot;A74ha7G812VedRgzzbQ&quot;,

&quot;filesize&quot;:&quot;12Byte(s)&quot;

,&quot;groupname&quot;:&quot;ygdemo&quot;,

&quot;url&quot;:&quot;http://172.20.53.144:8080/wbalone/images/tenant/4823b949-fe3c-4        b2e-b8c4-151bd9867f93.txt&quot;,

&quot;uploadtime&quot;:&quot;2018-10-29 15:29:01&quot;

}],

&quot;message&quot;:&quot;上传成功&quot;,

&quot;status&quot;:1

}

异常返回：

{

data:&quot;&quot;,

message: &quot;上传失败{jca.bat}附件已经存在不能重复上传&quot;

status: 0

}

参数说明：

 data：返回的数据

filename：附件名

filepath:附件路径

id：附件id

filesize：附件大小

groupname:附件分组名

url：附件的存储地址

uploadtime：附件上传时间

message：返回的信息

status：状态码，1表示成功，0表示失败

8.附件复制

请求体

| URl | /iuap-saas-filesystem-service/file/copy |
| --- | --- |
| Method | POST |

常用请求参数：

参数以form格式提交

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| filepath | String | 是 | 单据相关的唯一标示，一般包含单据ID，如果有多个附件的时候由业务自己制定规则 |
| groupname | String | 是 | 分组名 |
| id | String | 是 | 附件id |
| tenant | String | 否 | 租户id |

正确返回值

{

&quot;data&quot;:[{

&quot;filename&quot;:&quot;attachTest.txt&quot;,

&quot;filepath&quot;:&quot;a58cd83c-8a41-4dd2-8b20-761ace347e1d&quot;,

&quot;id&quot;:&quot;qh0jxuMZQZShnl5BxaC&quot;,

&quot;filesize&quot;:&quot;12Byte(s)&quot;,

&quot;groupname&quot;:&quot;ygdemo&quot;,

&quot;url&quot;:&quot;http://172.20.53.144:8080/wbalone/images/tenant/df735b45-e3d1-4403-878a-9f41fa490d9a.txt&quot;,

&quot;uploadtime&quot;:&quot;2018-10-29 15:43:31&quot;,

&quot;sourcetenant&quot;:&quot;tenant&quot;,

&quot;secretkey&quot;:&quot;&quot;

}],

&quot;message&quot;:&quot;附件复制成功&quot;,

&quot;status&quot;:1

}

异常返回值：

{&quot;message&quot;:&quot;附件复制出错&quot;,&quot;status&quot;:0}

参数说明：

data：返回的数据

filename：附件名

filepath:附件路径

id：附件id

filesize：附件大小

groupname:附件分组名

url：附件的存储地址

uploadtime：附件上传时间

sourcetenant:源租户

secretkey：加密秘钥

message：返回的信息

status：状态码，1表示成功，0表示失败

9.附件下载

请求体

| URl | /iuap-saas-filesystem-service/file/download |
| --- | --- |
| Method | POST |

请求参数

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| id | String | 是 | 附件id |
| permission | String | 否 | Oss权限(read，private，full),read是可读=公有，private=私有，当这个参数不传的时候会默认private |
| stream | String | 否 | 是否已流形式下载，true表示是，false表示否 |

正确返回值：

9.1 以流形式下载附件

{&quot;data&quot;:{&quot;headers&quot;:{&quot;Content-Type&quot;:[&quot;application/octet-stream&quot;]},&quot;body&quot;:&quot;aGVsbG8gd29ybGQh&quot;,&quot;statusCode&quot;:&quot;OK&quot;},&quot;message&quot;:&quot;下载成功&quot;,&quot;status&quot;:1}

9.2 以文件形式下载附件将直接下载，无返回值。

错误返回值：

{&quot;message&quot;:&quot;没有找到相关数据&quot;,&quot;status&quot;:0,data:&quot;&quot;}

或{&quot;message&quot;:&quot;SAAS返回字符流为空&quot;,&quot;status&quot;:0,data:&quot;&quot;}

参数说明：

message：返回的信息

status：状态码，1表示成功，0表示失败

data返回批量更新成功记录数



10.获取附件url地址

请求体

| URl | /iuap-saas-filesystem-service/file/url |
| --- | --- |
| Method | POST |



请求参数

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| ids | String[] | 是 | 附件id数组 |
| thumbnail | String | 否 | 缩略图大小;&quot;500w&quot;,缩略图--可调节大小，和url参数配合使用，不会存储到数据库 |

返回值：

{

&quot;data&quot;:[{

&quot;附件id1&quot;:&quot;附件url1&quot;,

&quot;附件id2&quot;:&quot;附件url2&quot;,

&quot;附件id3&quot;:&quot;附件url3&quot;,

...

}],

&quot;message&quot;:&quot;成功获得URL&quot;,

&quot;status&quot;:1

}

参数说明：

data里的数据是附件id和url的键值对

message：返回的信息

status：状态码，1表示成功，0表示失败

11. oss直传

请求体

| URl | /iuap-saas-filesystem-service/file/oss |
| --- | --- |
| Method | POST |

常用请求参数：

参数以form格式提交

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| filepath | String | 是 | 单据相关的唯一标示，一般包含单据ID，如果有多个附件的时候由业务自己制定规则 |
| groupname | String | 是 | 分组名 |
| filename | String | 是 | 附件名 |
| isreplace | boolean | 否 | 当filepath groupname filename 都一致的时候是否覆盖上传附件，默认false，不覆盖 |

上传成功无返回值

上传失败返回值：

{

&quot;data&quot;:&quot;&quot;,

&quot;message&quot;:&quot;xxx已经存在不能重复上传&quot;,

&quot;status&quot;:0

}

参数说明：

data：返回的数据

message：返回的信息

status：状态码，1表示成功，0表示失败


12. oss直传（加签）

请求体

| URl | /iuap-saas-filesystem-service/file/osssign |
| --- | --- |
| Method | POST |

常用请求参数：

参数以form格式提交

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| filepath | String | 是 | 单据相关的唯一标示，一般包含单据ID，如果有多个附件的时候由业务自己制定规则 |
| groupname | String | 是 | 分组名 |
| filename | String | 是 | 附件名 |
| isreplace | boolean | 否 | 当filepath groupname filename 都一致的时候是否覆盖上传附件，默认false，不覆盖 |
| permission | String | 否 | Oss权限(read，private，full),read是可读=公有，private=私有，当这个参数不传的时候会默认private |
| bucketname | String | 是 | Oss里bucket的名称 |

无返回值

13. oss直传（回写到数据库表）

请求体

| URl | /iuap-saas-filesystem-service/file/rewrite |
| --- | --- |
| Method | POST |

常用请求参数：

参数以form格式提交

| 参数 | 参数类型 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| filepath | String | 是 | 单据相关的唯一标示，一般包含单据ID，如果有多个附件的时候由业务自己制定规则 |
| groupname | String | 是 | 分组名 |
| filename | String | 是 | 附件名 |
| permission | String | 否 | Oss权限(read，private，full),read是可读=公有，private=私有，当这个参数不传的时候会默认private |
| sysid | String | 否 | 系统id |
| tenantid | String | 否 | 租户 |
| url | String | 是 | 这里传true或false。为true，则返回连接地址存到数据库中 |
| size | String | 是 | 附件大小 |
| thumbnail | String | 否 | 缩略图大小，非图片不传；&quot;500w&quot;,//缩略图--可调节大小，和url参数配合使用，不会存储到数据库 |
| userid | String | 否 | 用户id |

无返回值
