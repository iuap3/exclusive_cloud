# 服务端接口校验

## 适用场景
用于验证服务端的接口参数是否符合校验注解的校验规则; 
通过在服务的接口上标注[BeanValidator](https://beanvalidation.org/)校验注解, 服务消费方在发起RPC调用后, 如果未通过接口校验规则, 服务提供方将返回IllegalArgumentException, 在异常消息中包含未通过校验参数信息及未通过校验原因.

## maven配置
maven组件依赖对应的pom.xml中，依赖如下：

	<dependency>
		<groupId>com.yonyou.cloud.middleware</groupId>
		<artifactId>iris-validate-support</artifactId>
		<version>5.1.1-RELEASE</version>
	</dependency>


## 使用用法

- 在服务端接口API参数上增加javax.validation.constraints/org.hibernate.validator.constraints包下的注解, 如:
	
		import javax.validation.Valid;
		import javax.validation.constraints.NotEmpty;
		
		import com.yonyou.cloud.middleware.rpc.RemoteCall;
		import com.yonyou.cloud.ms.constants.AppInfoConstant;
		import com.yonyou.cloud.ms.dto.ApiParamDto;
		
		@RemoteCall(AppInfoConstant.APP_INF_PROVIDER)
		public interface IApiParamValidateService {
		
			public String apiWithValidateParams(@NotEmpty String[] arrs, @Valid ApiParamDto apiParam);
		
		}
- 接口参数是DTO的不仅要在接口字段上标注校验规则, 且DTO参数前要加上@Valid注解, 表明此DTO是要经过校验的:
	
		import javax.validation.constraints.Email;
		import javax.validation.constraints.Max;
		import javax.validation.constraints.NotBlank;
		
		import org.hibernate.validator.constraints.URL;
		
		public class ApiParamDto {
	
		@NotBlank
		private String hasText;
	
		@Max(10)
		private int maxTen;
	
		@Email
		private String email;
	
		@URL
		private String url;
	
		// 此处省略了gettters&setters

		}

	
- 进行API调用, 如果参数校验未通过, 则服务端会抛出IllegalArgumentException, 异常Message中包含不符合校验规则的参数信息,如:
		
		com.yonyou.cloud.exception.PluginRuntimeException: java.lang.IllegalArgumentException: [apiWithValidateParams.arg0的值为:([Ljava.lang.String;@7dacbbd9),验证失败原因:=>不能为空], [apiWithValidateParams.arg1.email的值为:(abc+qq.com),验证失败原因:=>不是一个合法的电子邮件地址], [apiWithValidateParams.arg1.hasText的值为:(),验证失败原因:=>不能为空], [apiWithValidateParams.arg1.maxTen的值为:(11),验证失败原因:=>最大不能超过10] 


## 部署/运行Servlet容器版本要求:

- TomcatV8.5及以上
- JettyV9.4及以上


## 校验规则说明:
### javax.validation.constraints:
<table>
    <thead>
        <tr>
            <th>校验注解</th>
            <th>作用</th>
            <th>示例</th>
        </tr>
    </thead>
    <tr>
      	 <td>@AssertFalse</td>
		 <td>校验参数值是否为false</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@AssertTrue</td>
		 <td>校验参数值是否为true</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@DecimalMax</td>
		 <td>校验小数的最大值(value="10.01")</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@DecimalMin</td>
		 <td>校验小数的最小值(value="10.01")</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@Digits</td>
		 <td>验证数字的整数位和小数位的位数是否超过指定的长度。</td>
		 <td>@Digits(integer = 2, fraction = 2)</td>
    </tr>
    <tr>
      	 <td>@Email</td>
		 <td>校验参数值是否符合邮箱地址格式</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@Future</td>
		 <td>参数值必须是java.time包下的日期类型的未来日期</td>
		 <td>@Future Date theFutureTime;</td>
    </tr>
    <tr>
      	 <td>@FutureOrPresent</td>
		 <td>参数值必须是java.time包下的日期类型的现在或未来日期</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@Max</td>
		 <td>参数值必须小于或等于指定值</td>
		 <td>@Max(10) int maxParam</td>
    </tr>
    <tr>
      	 <td>@Min</td>
		 <td>参数值必须大于或等于指定值</td>
		 <td>@Min(5)int minParam</td>
    </tr>
    <tr>
      	 <td>@Negative</td>
		 <td>参数值必须是小于0的负数</td>
		 <td>@Negative float negval</td>
    </tr>
    <tr>
      	 <td>@NegativeOrZero</td>
		 <td>参数值必须是小于/等于0的数</td>
		 <td>@Negative int negval</td>
    </tr>
    <tr>
      	 <td>@NotBlank</td>
		 <td>参数值必须包含一个非空格的字符</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@NotEmpty</td>
		 <td>参数值的length或size()值必须大于0, 支持CharSequence/Collection/Map/Array</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@NotNull</td>
		 <td>参数值不可以为null</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@Null</td>
		 <td>参数值必须为null</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@Past</td>
		 <td>参数值必须是java.time包下的日期类型的过去日期</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@PastOrPresent</td>
		 <td>参数值必须是java.time包下的日期类型的现在或过去日期</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@Pattern</td>
		 <td>参数值必须匹配制定的正则表达式</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@Positive</td>
		 <td>参数值必须是大于0的负数</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@PositiveOrZero</td>
		 <td>参数值必须是大于或等于0的值</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@Size</td>
		 <td>参数值的length或size()值在指定的范围内, 支持CharSequence/Collection/Map/Array</td>
		 <td>@Size(min=5,max=10)</td>
    </tr>
</table>

### org.hibernate.validator.constraints:
<table>
    <thead>
        <tr>
            <th>校验注解</th>
            <th>作用</th>
            <th>示例</th>
        </tr>
    </thead>
    <tr>
      	 <td>@CreditCardNumber</td>
		 <td>信用卡号码校验</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@EAN</td>
		 <td>国际商品编码校验</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@SafeHtml</td>
		 <td>校验是否是合法的HTML</td>
		 <td></td>
    </tr>
    <tr>
      	 <td>@URL</td>
		 <td>校验是否是合法的URL</td>
		 <td></td>
    </tr>
	<tr>
      	 <td>@ConstraintComposition</td>
		 <td>自定义组合验证注解</td>
		 <td></td>
    </tr>
</table>
