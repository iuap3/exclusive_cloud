# 控制层介绍 #
控制层是用来接受不同的Ajax请求的，平台采用了SpringMVC实现，控制器Controller 负责处理由DispatcherServlet 分发的请求，它把用户请求的数据经过业务处理层处理之后封装成一个Model ，然后再把该Model 返回给对应的View 进行展示。在SpringMVC 中提供了一个非常简便的定义Controller 的方法，你无需继承特定的类或实现特定的接口，只需使用@Controller 标记一个类是Controller ，然后使用@RequestMapping 和@RequestParam 等一些注解用以定义URL 请求和Controller 方法之间的映射，这样的Controller 就能被外界访问到。


## 使用 URL访问 #

这种情况是在控制器上加了@RequestMapping 注解，所以当需要访问到里面使用了@RequestMapping 标记的方法page() 的时候就需要使用page 方法上@RequestMapping 相对于控制器GoodJdbcDemoController 上@RequestMapping 的地址，即/goods/page.html 。
   
```
/**
 * spring MVC控制类示例，开发者需要在此基础上进行修改，适应项目的需求
 */
@RestController
@RequestMapping(value = "/goods")
public class GoodJdbcDemoController {
	private final Logger logger = LoggerFactory.getLogger(getClass());
	
	@Autowired
	private GoodJdbcDemoService goodService;
	
	@RequestMapping(value="/page", method= RequestMethod.GET)
	public @ResponseBody Object page(@RequestParam(value = "pageIndex", defaultValue = "0") int pageNumber, @RequestParam(value = "pageSize", defaultValue = "10") int pageSize, @RequestParam(value = "sortType", defaultValue = "auto") String sortType, Model model, ServletRequest request) {
		Map<String,Object> result = new HashMap<String,Object>();
		Map<String, Object> searchParams = new HashMap<String, Object>();
		searchParams = Servlets.getParametersStartingWith(request, "search_");
		PageRequest pageRequest = buildPageRequest(pageNumber, pageSize, sortType);
		try {
			Page<GoodJdbcDemo> accountPage = goodService.getDemoPage(searchParams, pageRequest);
			result.put("data", accountPage);
			result.put("flag", "success");
			result.put("msg", "查询数据成功!");
		} catch (Exception e) {
			String errMsg = "查询数据详情失败!";
			result.put("flag", "fail");
			result.put("msg", errMsg);
			logger.error(errMsg, e);
		}
		return result;
	}
    ......
  }
```
