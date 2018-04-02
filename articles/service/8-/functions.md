
# 常见问题1：服务验证失败.请确认accessKey 和application.name的正确性! #




> - 输入的access.key和access.secret不一致或者错误
> 
> 
> 
> - 此AccessKey已被停用或者删除
> 
> 
> - application.name即应用编码输入错误
> 
> 
> - 此用户下没有应用编码为application.name的应用

# 常见问题2：对应的应用不存在! #

> 
> - 此用户下没有相应的应用
> 
> 
> 
> - value的值有误即应用编码或者租户id输入错误

# 常见问题3：can not find active server from eureka! server url is ... #


> - 服务提供方未启动或down掉
> 
> 
> 
> - 服务提供方因网络原因未注册到服务注册中心
> 
> 
> - 服务提供方的部署时的对外服务端口和application.properties属性文件中配置的server.port属性值不一致.
> 
> 
> - 服务提供方访问路径中的ContextPath与application.properties属性文件中配置的spring.application.name属性值不一致.
> 
> 
> - 一般情况下, 项目名称、项目maven配置的artifactId、application.properties属性文件中配置的spring.application.name属性值 及 部署时的ContextPath这四项要一致; 项目部署时的对外服务端口和application.properties属性文件中配置的server.port属性值要一致, 如果使用内置Jetty还需要和pom.xml中jetty-maven-plugin插件的port值要一致.

# 常见问题4：项目往服务注册中心注册或注销操作有延迟#
## 几个重要的时间段 ##



> - 注册到服务注册中心:
> 
> 
> - 实例心跳上报时间间隔: 30秒; 第一次心跳在30秒后(参见:HeartbeatThread "HeartbeatThread") 和 TimedSupervisorTask)
> 
> 
> - 实例状态刷新
> 
> 
> - 45秒如果服务注册中心未接收到心跳, 将会计划移除此实例.
> 
> 
> - 拉取注册列表
> 
> 
> - 每30秒从服务注册中心拉取下最新的实例增量信息, 但服务增量信息将会在服务注册中心中保持3分钟.
> 
> 
> - 注销
> 
> 
> - 在正常关闭容器(有执行shutdown操作而不是直接kill进程)的情况下, 客户端示例会向服务注册中心发送注销请求.
> 
> 
> - 服务器移除实例参见:EvictionTask
> 
> 
> - 客户端注册/注销的时间延迟
> 
> 
> - 客户端注册/注销反应到所有的实例一般需要两分钟.
> 
> 
> - 注册:30秒注册+30标识为健康状态+30从服务注册中心拉取=90秒内.
> 
> 
> - 注销:直接kill进程情况下,43秒(优化前为60秒)定时移除过期实例和45秒(优化前为90秒)实例过期时间 +30秒被所有实例拉取=约75秒(优化前为120秒)内.

## 常见问题5：限流不生效 ##


> - 唯一标识冲突
> 
> 
> - 此时可能是启用了自定义限流，和系统自动获取的RPC唯一标识冲突，此时我们可以查看对应的限流配置文件，查看时候冲突。
> 配置中心延迟
> 
> 
> - 由于限流配置的下发依赖与配置中心，由于配置文件更新的延迟，我们设置的限流策略，可能会有一定的生效延时，此时只需要等待一会儿即可生效。
