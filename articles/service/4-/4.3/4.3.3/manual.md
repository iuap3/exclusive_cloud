# 单元测试(junit)

## spring boot

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class XxxTest {
    
    public void xxx() {
        // do remote call
    }
}
```

## spring

由于spring的单元测试无法触发sdk的启动，所以单元测试需要在构造方法中启动sdk

```java

@RunWith(SpringJUnit4ClassRunner.class)
@WebAppConfiguration
@ContextConfiguration(locations = "classpath:xxxx.xml")
public class XxxTest {
    
    public XxxTest() {
        MwClientStartUp.starup();
    }
    
    public void xxx() {
        // do remote call
    }
}
```

***注意：使用和spring-test相对应的junit版本***