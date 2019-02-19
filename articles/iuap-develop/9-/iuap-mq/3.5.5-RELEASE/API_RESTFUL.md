Restful调用
-----------

### 发送文本消息

**（1）请求体：**

| URl    | /iuap-saas-message-center/message/ pushTextMessage |
|--------|----------------------------------------------------|
| Method | POST                                               |

**（2）请求参数：**

参数以json格式提交 

{  
    **"data"**:{  
        **"sendman"**:**"admin"**,  
        **"recevier"**:[  
            **"1001A1100000000014Y4"**  
        ],  
        **"subject"**:**"消息主题"**,  
        **"tenantid"**:**"tenant"**,  
        **"channel"**:[  
            **"sys"**  
        ],  
        **"msgtype"**:**"notice"**,  
        **"content"**:**"消息内容"**  
    }  
}

**（3）请求参数说明：**

| **参数** | **参数类型** | **是否必须** | **说明**                                                     |
|----------|--------------|--------------|--------------------------------------------------------------|
| sendman  | String       | 是           | 消息发送人ID                                                 |
| recevier | String数组   | 是           | 消息接收人ID                                                 |
| subject  | String       | 是           | 消息主题                                                     |
| tenantid | String       | 是           | 租户                                                         |
| channel  | String       | 是           | 消息通道（邮件：email；短信：note；站内信：sys）             |
| msgtype  | String       | 是           | 类型（通知：notice；调度任务：dispatchtask；流程任务：task） |
| content  | String       | 是           | 消息内容                                                     |

**（4）返回值：**

{  
    **"msg"**:**"返回信息"**,  
    **"status"**:**"0"**  
}

**（5）返回值参数说明：**

| **参数** | **参数类型** | **说明**                     |
|----------|--------------|------------------------------|
| msg      | String       | 返回信息，一般为错误信息内容 |
| status   | String       | 类型（0代表发送消息失败）    |

### 发送模板消息

**（1）请求体：**

| URl    | /iuap-saas-message-center/message/pushTemplateMessage |
|--------|-------------------------------------------------------|
| Method | POST                                                  |

**（2）请求参数：**

参数以json格式提交 

{  
    **"data"**:{  
        **"sendman"**:**"U001"**,  
        **"busiData"**:{  
            **"dbrw_info.name"**:**"督办名称"**,  
            **"dbrw_info.code"**:**"督001"**  
        },  
        **"recevier"**:[  
            **"U001"**  
        ],  
        **"subject"**:**"站内消息标题"**,  
        **"billid"**:**"ss0002"**,  
        **"channel"**:[  
            **"sys"**  
        ],  
        **"tenantid"**:**"tenant"**,  
        **"templateCode"**:**"ygdemo"**,  
        **"msgtype"**:**"notice"**,  
        **"content"**:**"模板消息内容"**  
    }  
}

**（3）参数说明：**

| **参数**     | **参数类型** | **是否必须** | **说明**                                                     |
|--------------|--------------|--------------|--------------------------------------------------------------|
| sendman      | String       | 是           | 消息发送人ID                                                 |
| recevier     | String数组   | 是           | 消息接收人ID                                                 |
| subject      | String       | 是           | 消息主题                                                     |
| tenantid     | String       | 是           | 租户                                                         |
| channel      | String数组   | 是           | 消息通道（邮件：email；短信：note；站内信：sys）             |
| msgtype      | String       | 是           | 类型（通知：notice；调度任务：dispatchtask；流程任务：task） |
| content      | String       | 是           | 消息内容                                                     |
| busiData     | Json         | 是           | 业务单据内容                                                 |
| billid       | String       | 是           | 业务单据编码                                                 |
| templateCode | String       | 是           | 消息模板编码                                                 |

**（4）返回值：**

{  
    **"msg"**:**"返回信息"**,  
    **"status"**:**"0"**  
}

**（5）返回值参数说明：**

| **参数** | **参数类型** | **说明**                     |
|----------|--------------|------------------------------|
| msg      | String       | 返回信息，一般为错误信息内容 |
| status   | String       | 类型（0代表发送消息失败）    |

### 邮箱支持发送附件

**（1）请求体：**

| URl    | iuap-saas-message-center/message/pushTextMessage |
|--------|--------------------------------------------------|
| Method | POST                                             |

**（2）请求参数：**

参数以json格式提交 

{  
    **"data"**:{  
        **"sendman"**:**"邮箱附件测试"**,  
        **"recevier"**:[  
            **"U001"**  
        ],  
        **"subject"**:**"邮箱附件"**,  
        **"tenantid"**:**"tenant"**,  
        **"channel"**:[  
            **"email"**  
        ],  
        **"msgtype"**:**"email"**,  
        **"attachFileUrls"**:[  
            **"**<http://10.10.24.84:8080/wbalone/images/401.png>**"**  
        ],  
        **"content"**:**"测试邮箱添加附件发送功能，附件以URL形式提供！"**  
    }  
}

**（3）参数说明：**

| **参数**       | **参数类型** | **是否必须** | **说明**                |
|----------------|--------------|--------------|-------------------------|
| sendman        | String       | 是           | 消息发送人ID            |
| recevier       | String数组   | 是           | 消息接收人ID            |
| subject        | String       | 是           | 消息主题                |
| tenantid       | String       | 是           | 租户                    |
| channel        | String数组   | 是           | 消息通道（邮件：email） |
| msgtype        | String       | 是           | 类型（邮件：email）     |
| content        | String       | 是           | 消息内容                |
| attachFileUrls | String数组   | 是           | 附件的网络地址          |

**（4）返回值：**

{  
    **"msg"**:**"返回信息"**,  
    **"status"**:**"0"**  
}

**（5）返回值参数说明：**

| **参数** | **参数类型** | **说明**                     |
|----------|--------------|------------------------------|
| msg      | String       | 返回信息，一般为错误信息内容 |
| status   | String       | 类型（0代表发送消息失败）    |
