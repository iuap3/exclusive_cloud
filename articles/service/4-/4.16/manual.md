# Feign迁移

---

## 配置文件

maven依赖新增

```xml
        <dependency>
            <groupId>com.yonyou.cloud.middleware</groupId>
            <artifactId>middleware</artifactId>
            <version>5.1.1-RELEASE</version>
        </dependency>
        <dependency>
            <groupId>com.yonyou.cloud.middleware</groupId>
            <artifactId>iris-springboot-support</artifactId>
            <version>5.1.1-RELEASE</version>
        </dependency>
        
```

配置文件新增

```yml
#公有云可不用此项
registry: http://xxxxxxx

access:
  key: xxxxx
  secret: xxxxxxxxxxx

#删除eureka、Feign相关配置
#eureka:
#  client:
#    serviceUrl:
#      defaultZone: http://127.0.0.1:8761/eureka/
```



## 代码
远程接口

```java
public interface CreditApi {
}

//修改为

@RemoteCall("服务提供方名字@providerid")
public interface CreditApi {
}
```

删除eureka和Feign相关注解和pom依赖

```java
@SpringBootApplication
//@EnableEurekaClient
public class App {

    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}

@SpringBootApplication
//@EnableEurekaClient
//@EnableFeignClients
public class App {

    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

删除被@FeignClient注解的class


## 建议
1. 远程接口的具体实现放在service层
2. 删除RequestMapping、GetMapping、PostMapping、RequestBody、RequestParam等注解



