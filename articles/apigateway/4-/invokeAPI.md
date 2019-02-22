# 二、	快速入门 调用API #
1. 概述  
本文将快速引导您创建并开放API。  
您需要依次完成以下步骤才能成功开放API服务：  
（1）	获取API文档  
（2）	创建应用  
（3）	获取授权  
（4）	调用API  
2. 	获取API文档  
（1）	从首页中跳入“API中心”页签  
（2）	选择所需的API，点击“立即使用”    
（3）	进入接口详情页面后，即可看到详细的接口定义    
（4）	选择“申请”按钮，对接口进行权限申请  
3. 创建应用  
应用（APP）是您调用 API 服务时的身份。每个 APP 有一组 Key 和 Secret，您可以理解为账号密码，您调用 API 的时候需要将 AppKey 做参数传入，AppSecret 用于签名计算，网关会校验这对密钥对您进行身份认证。调用 API 需要这个 APP 具备调用该API的权限，这个授权也是面向 APP 的。您可以在 API 网关控制台 应用管理 页面创建应用。创建成功后，系统会为应用分配一对 AppKey 和 AppSecret。
4. 	获取授权
API网关在调用API时提供两种全可靠的认证方式，分别通过apicode和appkey进行用户认证。  
**apicode授权**  
apicode授权方式是指，使用者进行API申请完成后，API网关会为申请完成的API提供一个apicode值，使用者可以根据apicode值进行调用API。    
**appkey + appsecret授权**   
您可以使用 AppKey 和 AppSecret 对您的 API 进行认证管理。AppKey和 AppSecret 成对出现。授权方式是依靠一组相关的API来实现。通过创建应用将一组特定功能的API关联起来，帮助用户高效、便捷的管理API。  Appkey是API网关为应用提供的授权方式，使用者可以根据Appkey调用此应用中的全部API。
6. 调用API
您可以直接用 API 网关控制台为您提供的多语言调用示例来测试调用，可以自行编辑 HTTP(s) 请求来调用 API。关于签名方式，您可以参照API网关提供的SDK及样例。
