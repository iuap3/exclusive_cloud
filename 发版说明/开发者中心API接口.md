# API接口文档规范

## 接口名称

```
部署接口
```

## 接口说明

```
通过一些参数实现部署任务。
调用此接口需要Access Key验证方式。
```
```
注意: 调用此接口需要Access Key验证方式。
```

## 接口使用说明

* maven引入:
<pre>
&lt;groupId&gt;com.yonyou.cloud&lt;/groupId&gt;
&lt;artifactId&gt;auth-sdk-client&lt;/artifactId&gt;
&lt;version&gt;1.0.13-SNAPSHOT&lt;/version&gt;
</pre>

<pre>
&lt;groupId&gt;com.yonyou&lt;/groupId&gt;
&lt;artifactId&gt;app.publish&lt;/artifactId&gt;
&lt;version&gt;0.0.1-SNAPSHOT&lt;/version&gt;
</pre>

* **调用方式:**

	1. 请求地址（请求方式POST）：developer.yonyoucloud.com/app-publish/api/v1/publish/deploy
	2. 参数说明
	<pre>
	Cookie
	主要信息：userId，u_providerid，userName
	例如:u_providerid=xxxxx; userName=xxxxx;userId=xxxx; </pre>

  
	

	 <pre>
	请求BODY
	{
    "app_name": "fgsdfg",应用名称                            
    "app_id": "b2e4ad18-217f-4a3b-a95c-d25c081a5a0e",应用id
    "app_type": 1, 应用部署环境
    "app_code": "sdfgsdfg", 应用编码
    "cpus": 0.1, 设置cpu
    "mem": 256,设置内存
    "disk": 1024,设置磁盘
    "instances": 1,设置实例
    "res_pool_ids": [ 资源池id
        1
    ],
    "publish_type": 0,
    "cmd": null, 启动命令
    "container": {  容器相关
        "type": "DOCKER",
        "volumes": [],
        "docker": {
            "image": "dockerhub.yonyoucloud.com/35568e76-1ef1-4d77-b5cf-8fb66d2c8002/sdfgsdfg:2017128101639", 镜像地址
            "network": "BRIDGE",
            "portMappings": [
                {
                    "containerPort": 8080, 端口号
                    "hostPort": 0,           
                    "servicePort": null,
                    "protocol": "tcp",      协议
                    "access_mode": "TCP", 访问方式
                    "access_range": "OUTER" 访问范围（内部INNER、外部、不可访问ALL）
                }
            ],
            "privileged": true,
            "parameters": [],
            "forcePullImage": true
        }
    },
    "healthChecks": [  健康检查
        {
            "path": "/",
            "protocol": "MESOS_TCP",
            "portIndex": 0,
            "gracePeriodSeconds": 50,
            "intervalSeconds": 10,
            "timeoutSeconds": 10,
            "maxConsecutiveFailures": 10,
            "ignoreHttp1xx": false
        }
    ],
    "labels": {
        "HAPROXY_GROUP": "isv-apps-group1"
    },
    "group_name": "", 分组名称
    "portDefinitions": [
        {
            "port": 35000,
            "protocol": "tcp",
            "labels": {}
        }
    ],
    "confProperties": null, 配置文件内容
    "fake_id": 0, 对应空壳应用0,1,2,3
    "parent_id": 0, 通过group_name获取
    "is_fake": false  是否是微服务
	}</pre>

	<pre>
	客户端调用
	AuthSDKClient authSDKClient = new AuthSDKClient("accessKey", "accessSecret");
	HttpPost request = HttpUtils.genPostRequest("URL", null, httpentity, null, null);
	HttpResult result = authSDKClient.execute(request);</pre>
		

## 接口名称

```
持续集成创建接口
```

## 接口说明

```
实现创建持续集成任务，参数不变情况下，在次请求为创建新版本。
```

```
注意: 调用此接口需要使用Access Key方式。
```

## 接口使用说明

* maven引入:
<pre>
&lt;groupId&gt;com.yonyou.cloud&lt;/groupId&gt;
&lt;artifactId&gt;auth-sdk-client&lt;/artifactId&gt;
&lt;version&gt;1.0.13-SNAPSHOT&lt;/version&gt;
</pre>

* **调用方式:**

	1. 请求地址（请求方式POST）：developer.yonyoucloud.com/api/v1/JenkinsUpload/appJenkinsUpload
	2. 参数说明
	3. <pre>{
	      "warurl": "oss_url.war",//OSS地址
	      "signflag": false,
		  "iconDefault": true,//图片是否默认
		  "ak": "accessKey",
		  "as": "accessSecret",
		  "username": "jenkinsname",
		  "usertoken": "jenkinstoken",
		  "publishApp": false,
		  "descobj": {
			 "appCode": "appcode",
			 "appName": "appname",
			 "appType": "j2ee",
			 "appUploadId": "",
			 "baseImage": "tomcat:9.0.0.M9-jdk8-alpine",//基础镜像
			 "description": "",
			 "emails": "test@yonyou.com",
			 "env": 1,
			 "imageName": "imagename",
			 "is_root_path_access": true,//是否根目录访问
			 "version": "v1.0"
		  }
		}
</pre>
	3. 客户端调用
		<pre>AuthSDKClient authSDKClient = new AuthSDKClient("accessKey", "accessSecret");
		HttpPost request = HttpUtils.genPostRequest("URL", null, httpentity, null, null);
	    HttpResult result = authSDKClient.execute(request);</pre>