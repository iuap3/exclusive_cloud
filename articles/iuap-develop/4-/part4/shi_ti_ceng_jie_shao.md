# 实体层介绍

实体层是与数据库表进行映射的vo类，采用iuap-jdbc方式进行持久化的vo类的示例如下，如果是JPA方式采用简单POJO类配合相应的注解即可。  
注意：iuap-jdbc方式的vo类，注解为iuap-jdbc组件指定的注解。  

```
/**
 * 商品示例实体,iuap-jdbc方式
 */
@Entity
@Table(name="good_demo")
public class GoodJdbcDemo extends BaseEntity{

	private static final long serialVersionUID = -3773469861533388458L;

	@Id
    @Column(name = "productid")
    @GeneratedValue(strategy=Stragegy.UUID,moudle="example_demo")
    private String productid;
    
    @Column(name = "productName")
    private String productName;
    
    @Column(name = "productNum")
    private BigInteger productNum;
    
    @Column(name = "price")
    private Double price;
   
	public String getProductid() {
		return productid;
	}

	public void setProductid(String productid) {
		this.productid = productid;
	}

	public String getProductName() {
		return productName;
	}

	public void setProductName(String productName) {
		this.productName = productName;
	}
   ............
```

其中iuap-jdbc要求用户实现的预留元数据方式访问的方法如下，请注意：  

```
@Override
	public String getMetaDefinedName() {
		return "GoodJdbcDemo";
	}

	@Override
	public String getNamespace() {
		//应该工具的元数据设计器上指定
		return "com.yonyou.iuap.example";
	}
```

具体的注解的使用请参考技术组件中的JDBC持久化组件对应的手册。  