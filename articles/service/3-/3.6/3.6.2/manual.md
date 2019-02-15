# 最终一致性框架CC

## 产品介绍

TccTransaction分布式事务框架是解决微服务开发环境中，事务一致性问题而提出的一种解决方案，与传统TCC分布式事务一致性方案不同，
TccTransaction是一个没有事务协调器、没有事务中心概念，事务由各个参与者共同完成整个事务的管理，是用友微服务治理平台提供的
一个核心功能组件。
## 与传统tcc的优缺点
* 优点
  1）使用简单，业务只需要使用注解，并提供共confirm及cancel方法即可
  2）没有事务协调器，各节点平等参与事务，各节点记录自己的事务信息，避免了中心服务器的瓶颈
  3）由业务节点自行confirm或cancel
  4）非常适合微服务调用链事务一致性

* 缺点
  1）由于没有事务协调器，事务信息分散在各业务节点，不利于全局事务监控
  2）需要各业务节点向全局事务控制平台上报各自异常的事务信息，有可能出现在控制台看不到完整的事务参与者信息

## 功能介绍

* 解决分布式事务一致性问题，业务使用分布式事务一致性功能只需两个注解，  @TccTransactional(confirm="confirmOrder", cancel="cancelOrder")标识业务方法需要使用分布式事务
  confirm属性指定了业务事务提交方法，cancel属性指定了业务异常时的取消方法，与业务方法在同一个接口中实现，提交或取消方法上加入  @Async注解
* 基于可靠消息的异步调用框架（EOS）的最终一致性
* 支持链式rpc调用中各个业务节点的事务一致性
* 基于用友微服务治理平台rpc框架Iris
* 事务信息数据与业务库一起，保证业务与消息发送一致性，分布式事务数据记录由框架自行操作，用户无需关心
* Tcc框架支持软隔离机制，可通过两种方式来实现隔离：1）将try阶段执行资源预留，confirm阶段执行提交，预留后的资源其它系统无法获取，如预扣，账户合法性检查，
  真正数据库操作在confirm阶段执行insert或update操作，2）在try阶段执行预插入及更新操作，并由业务对数据有效性状态进行标记，由confirm或cancel修改数据状态，confirm后的数据可用
* 支持事务上下文，在try阶段可将用户数据存储到当前事务上下文中，然后在confirm或cancel方法内可以根据获取try阶段设置的事务上下文信息
* confirm、cancel方法第一个参数为TccTransactionContext context，代表当前事务上下文信息
* 事务边界：1）第一个rpc 服务endpoint，事务开始于服务端，服务端同步调用其它服务时处于同一个全局事务 2）异步调用，异步调用是事务起点
  3）sagas与tcc事务混用时，两个服务之间调用如果调用端和服务端用了不一样的事务框架，则，被调用端开启新事务
*  confirm及cancel方法必须确保幂等性操作



## TccTransaction事务场景
* TccTransaction分布式事务框架解决rpc链式同步调用事务一致性
![](./images/cc-1.png)


## TccTransaction事务管理控制台
* TccTransaction控制台为分布式事务控制管理提供了可视化的界面, 当分布式事务节点异常，导致事务最终不一致的时候提供人工介入处理的途径，可以通过该界面，查看当前
全局不一致的事务，并可以发起补偿操作


## TccTransaction事务异步消息
* 在使用TccTransaction框架前, 需要配置应用的RabbitMQ地址:
	* 页面导航: 开发者中心左边菜单 &gt;&gt; 微服务 &gt;&gt; 服务管理 &gt;&gt; 找到对应的应用 &gt;&gt; 点进对应的环境 &gt;&gt; 微服务 &gt;&gt; 事务管理 &gt;&gt; 消息队列配置 &gt;&gt; 在输入框中填写RabbitMQ地址, 如: IP1:Port1,IP2:Port2; 参见下图:
![](./images/eos-console.png)
* 消息地址配置输入框:
![](./images/rabbitmq-config.png)

* 配置完消息地址后, 按照异步框架的开发指南进行工程配置, 若有异常会上报到EOS控制台; 按照以下步骤查询异常消息: <p>开发者中心左边菜单 &gt;&gt; 微服务 &gt;&gt; 服务管理 &gt;&gt; 找到对应的应用 &gt;&gt; 点进对应的环境 &gt;&gt; 微服务 &gt;&gt; **事务管理** &gt;&gt;</p>
* 点击重试后会下发命令到客户端进行重试, 客户端重试完成后会上报重试结果, 重试成功后会在状态列中显示"重试成功", 否则显示"重试失败".
* 点击忽略后会下发命令到客户端进行忽略, 被忽略的消息应交由人工处理, 进行数据的校对和检查.
![](./images/mq-process.png)


## TccTransaction事务使用  
* 业务场景，见图，实箭头表示同步调用，虚箭头为异步调用  
  ![](./images/tcc.png)

* 引入相关jar包
```
		<dependency>
			<groupId>com.yonyou.cloud.middleware</groupId>
			<artifactId>eos-spring-support</artifactId>
			<!--需要引入eos-spring-support 5.1.0以后的包-->
			<version>${mw.version}</version>
		</dependency>

		<!--tcc事务框架包-->
		<dependency>
			<groupId>com.yonyou.cloud.middleware</groupId>
			<artifactId>Tcc</artifactId>
			<version>${Tcc.version}</version>
		</dependency>
		<!--引入middleware sdk-->
		<dependency>
			<groupId>com.yonyou.cloud.middleware</groupId>
			<artifactId>mwclient</artifactId>
			<version>${mw.version}</version>
			<type>pom</type>
		</dependency>
```

* 编写业务接口，包括业务方法和补偿方法，通过分布式事务注解来实现
```
     /**
     * 订单服务
     * @author Administrator
     *
     */
     @RemoteCall(AppConstant.APP_INFO_ORDERSERVICE)
     public interface IOrderService {

	  @ApiOperation(value="下旅游订单", response=TourOrder.class)
	  @TccTransactional(confirm="confirmOrder", cancel="cancelOrder")
	  public abstract TourOrder order(TourOrder paramTourOrder);

	  @ApiOperation(value="确定旅游订单", response=TourOrder.class)
	  @Async
	  public abstract TourOrder confirmOrder(TccTransactionContext context, TourOrder paramTourOrder);
	  
	  @ApiOperation(value="取消旅游订单", response=TourOrder.class)
	  @Async
	  public abstract TourOrder cancelOrder(TccTransactionContext context, TourOrder paramTourOrder);
     }


    /**
     * 机票预订服务
     * @author Administrator
     *
     */
     @RemoteCall(AppConstant.APP_INFO_PLANESERVICE)
     public interface IMsPlaneService {
	  @ApiOperation(value="预订机票", response=Void.class)
	  @TccTransactional(confirm="confirmPlane", cancel="cancelPlane")
	  public abstract void orderPlane(PlaneOrder paramPlaneOrder);

	  
	  @ApiOperation(value="确定机票预定", response=Void.class)
	  @Async
	  public abstract void confirmPlane(TccTransactionContext context, PlaneOrder paramPlaneOrder);
	  
	  @ApiOperation(value="取消机票预定", response=Void.class)
	  @Async
	  public abstract void cancelPlane(TccTransactionContext context, PlaneOrder paramPlaneOrder);
	  
	  
	  @ApiOperation(value="测试", response=String.class)
	  @Async
	  public abstract String testAsync(String hello);
	  
     }

    /**
     * 积分服务
     * @author Administrator
     *
     */
     @RemoteCall(AppConstant.APP_INFO_MEMBERSERVICE)
     public interface IMsMemberService {
	 @ApiOperation(value="会员积分", response=Void.class)
	  @TccTransactional(confirm="confirmPoints", cancel="cancelPoints")
	  public abstract void addPoints(String paramString1, String paramString2, Double paramDouble);
	 
	 
	  @ApiOperation(value="确认会员积分", response=Void.class)
	  @Async
	  public abstract void confirmPoints(TccTransactionContext context, String paramString1, String paramString2, Double paramDouble);
	  
	  @ApiOperation(value="取消会员积分", response=Void.class)
	  @Async
	  public abstract void cancelPoints(TccTransactionContext context, String paramString1, String paramString2, Double paramDouble);
     }

    /**
     * 酒店预订服务
     * @author Administrator
     *
     */

     @RemoteCall(AppConstant.APP_INFO_HOTELSERVICE)
     public interface IMsHotelService {
	  @ApiOperation(value="预订酒店", response=Void.class)
	  @TccTransactional(confirm="confirmHotel", cancel="cancelHotel")
	  public abstract void orderHotel(HotelOrder paramHotelOrder);

	  @ApiOperation(value="确认酒店预订", response=Void.class)
	  @Async
	  public abstract void confirmHotel(TccTransactionContext context, HotelOrder paramHotelOrder);
	  
	  @ApiOperation(value="取消酒店预订", response=Void.class)
	  @Async
	  public abstract void cancelHotel(TccTransactionContext context, HotelOrder paramHotelOrder);
     }


    /**
     * 车辆预订服务
     * @author Administrator
     *
     */
    @RemoteCall(AppConstant.APP_INFO_CARSERVICE)
    public interface IMsCarService {
	  @ApiOperation(value="车辆预订", response=Void.class)
	  @TccTransactional(confirm="confirmCar", cancel="cancelCar")
	  public abstract void orderCar(CarOrder paramCarOrder);
	
	  @ApiOperation(value="确认车辆预订", response=Void.class)
	  @Async
	  public abstract void confirmCar(TccTransactionContext context, CarOrder paramCarOrder);
	  
	  
	  @ApiOperation(value="取消车辆预订", response=Void.class)
	  @Async
	  public abstract void cancelCar(TccTransactionContext context, CarOrder paramCarOrder);
	  
     }

    /**
     * 餐饮预订服务
     * @author Administrator
     *
     */
     @RemoteCall(AppConstant.APP_INFO_DINNERSERVICE)
     public interface IMsDinnerService {
	
	  @ApiOperation(value="餐饮预定", response=Void.class)
	  @TccTransactional(confirm="confirmDinner", cancel="cancelDinner")
	  public abstract void orderDinner(DinnerOrder paramDinnerOrder);
	
	  
	  @ApiOperation(value="确认餐饮预定", response=Void.class)
	  @Async
	  public abstract void confirmDinner(TccTransactionContext context, DinnerOrder paramDinnerOrder);
	  
	  @ApiOperation(value="取消餐饮预定", response=Void.class)
	  @Async
	  public abstract void cancelDinner(TccTransactionContext context, DinnerOrder paramDinnerOrder);
     }

    /**
     * 异步服务，测试事务边界
     * @author Administrator
     *
     */
     @RemoteCall(AppConstant.APP_INFO_MSGSERVICE)
     public interface IMsMsgService {
	 @ApiOperation(value="短信服务", response=Void.class)
	  @TccTransactional(confirm="confirmMsg", cancel="cancelMsg")
	  @Async
	  public abstract void sendMsg(String phone, String msg, String bizId);

	  @ApiOperation(value="确认短信服务", response=Void.class)
	  @Async
	  public abstract void confirmMsg(TccTransactionContext context, String phone, String msg, String bizId);
	 
	  @ApiOperation(value="取消短信服务", response=Void.class)
	  @Async
	  public abstract void cancelMsg(TccTransactionContext context, String phone, String msg, String bizId);
}


    /**
     * 异步服务
     * @author Administrator
     *
     */
   @RemoteCall(AppConstant.APP_INFO_MSGfEESERVICE)
   public interface IMsMsgFeeService {
	 @ApiOperation(value="短信计费服务", response=Void.class)
	  @TccTransactional(confirm="confirmMsgFee", cancel="cancelMsgFee")
	  public abstract void MsgFee(String bizId, Double fee);

	  @ApiOperation(value="确认短信计费服务", response=Void.class)
	  @Async
	  public abstract void confirmMsgFee(TccTransactionContext context, String bizId, Double fee);
	 
	  @ApiOperation(value="取消短信计费服务", response=Void.class)
	  @Async
	  public abstract void cancelMsgFee(TccTransactionContext context, String bizId, Double fee);
}
```



* 服务提供方实现业务接口
```
       /**
        * 订单服务
        * @author Administrator
        *
        */
      @Service("orderService")
      public  class OrderService implements IOrderService {
	  private static final Logger logger = LoggerFactory
			.getLogger(OrderService.class);
	  @Autowired
	  private JdbcTemplate jdbcTemplate;
	
	  @Autowired
	  private IMsHotelService msHotelService;
	
	  @Autowired
	  private IMsPlaneService msPlaneService;

	  public static final ObjectMapper OBJECT_MAPPER = new ObjectMapper();
	
	  @Transactional
	  public TourOrder order(TourOrder dto) {
		dto.setStatus(AppConstant.CONFIRMING);
		this.jdbcTemplate
				.update("insert into biz_tourorder (orderName,userId,userName,dest,status,tourOrderId) values (?,?,?,?,?,?)",
						new Object[] { dto.getOrderName(), dto.getUserId(),
								dto.getUserName(), dto.getDest(),
								dto.getStatus(), dto.getTourOrderId() });
		PlaneOrder planeOrder = new PlaneOrder();
		planeOrder.setAirport("首都国际机场");
		planeOrder.setArrive("上海虹桥机场");
		planeOrder.setPlaneNo("CCAC5982");
		planeOrder.setPlaneOrderId(UUID.randomUUID().toString());
		planeOrder.setPrice(Double.valueOf(2903.0D));
		planeOrder.setStart(new DateTime().toString());
		planeOrder.setUserId(dto.getUserId());
		planeOrder.setStatus(AppConstant.CONFIRMING);
		try {
		// 将当前业务数据保存到当前事务中，便于在confirm或cancel时能够获取到该业务数据做提交或回退操作
			System.out.println(OBJECT_MAPPER.writeValueAsString(dto));
			TccTransactionUtils.setContext(OBJECT_MAPPER.writeValueAsString(dto));
		} catch (Throwable e) {
			e.printStackTrace();
			throw new RuntimeException("参数序列化异常");
		}
	    this.msPlaneService.orderPlane(planeOrder);
		HotelOrder hotelOrder = new HotelOrder();
		hotelOrder.setStart(new DateTime().toString());
		hotelOrder.setEnd(new DateTime().toString());
		hotelOrder.setHotelName("锦江之星宾馆");
		hotelOrder.setHotelOrderId(UUID.randomUUID().toString());
		hotelOrder.setRoomNo("401");
		hotelOrder.setStatus(AppConstant.CONFIRMING);
		hotelOrder.setUserId(dto.getUserId());
		hotelOrder.setUserName(dto.getUserName());
		this.msHotelService.orderHotel(hotelOrder);
		return dto;
	  }

	  @Transactional
	  public TourOrder confirmOrder(TccTransactionContext context,TourOrder dto) {
	        //获取当前事务上下文数据
		String restore = TccTransactionUtils.getContext(context);
		logger.info("===========================" + restore);
		this.jdbcTemplate.update(
				"update biz_tourorder set status=?  where tourOrderId=?",
				new Object[] { AppConstant.CONFIRMED, dto.getTourOrderId() });
		return dto;
	  }
	
	  @Transactional
	  public TourOrder cancelOrder(TccTransactionContext context,TourOrder dto) {
	        //获取当前事务上下文数据
		String restore = TccTransactionUtils.getContext(context);
		logger.info("===========================" + restore);
		this.jdbcTemplate.update(
				"update biz_tourorder set status=?  where tourOrderId=?",
				new Object[] { AppConstant.DELETED, dto.getTourOrderId() });
		return dto;
	  }
      }


       /**
        * 机票预订服务
        * @author Administrator
        *
        */
      @Service("msPlaneService")
      public class MsPlaneService implements IMsPlaneService {
	  private static final Logger logger = LoggerFactory
	 		.getLogger(MsPlaneService.class);
	
	  @Autowired
	  private JdbcTemplate jdbcTemplate;
	  @Autowired
	  IMsMemberService msMemberService;
	  public static final ObjectMapper OBJECT_MAPPER = new ObjectMapper();
	
	  @Transactional
	  public void orderPlane(PlaneOrder dto) {
			this.jdbcTemplate
					.update("insert into biz_planeorder(start,arrive,airport,price,userId,planeNo,planeOrderId,status) values (?,?,?,?,?,?,?,?)",
							new Object[] { dto

							.getStart(), dto.getArrive(), dto.getAirport(),
									dto.getPrice(), dto.getUserId(),
									dto.getPlaneNo(), dto.getPlaneOrderId(),
									dto.getStatus() });
	
		Double points = Double.valueOf(0.1D);
		String bizId = UUID.randomUUID().toString();
		try {
			TccTransactionUtils.setContext(OBJECT_MAPPER.writeValueAsString(dto));
		} catch (JsonProcessingException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		this.msMemberService.addPoints(dto.getUserId(), bizId, points);
	  }

	
	  @Transactional
	  public void confirmPlane(TccTransactionContext context, PlaneOrder dto) {
		String restore = TccTransactionUtils.getContext(context);
		logger.info("===========================" + restore);
		this.jdbcTemplate.update(
				"update biz_planeorder set status=? where planeOrderId=?",
				new Object[] { AppConstant.CONFIRMED, dto.getPlaneOrderId() });
	  }
	
	  @Transactional
	  public void cancelPlane(TccTransactionContext context, PlaneOrder dto) {
		String restore = TccTransactionUtils.getContext(context);
		logger.info("===========================" + restore);
		this.jdbcTemplate.update(
				"update biz_planeorder set status=? where planeOrderId=?",
				new Object[] { AppConstant.DELETED, dto.getPlaneOrderId() });
	  }
	
	  @Override
	  public String testAsync(String hello) {
		// TODO Auto-generated method stub
		return "hello world";
	  }
	
     }


       /**
        * 积分服务
        * @author Administrator
        *
        */

     @Service("msMemberService")
     public class MsMemberService implements IMsMemberService {
	  private static final Logger logger = LoggerFactory
			.getLogger(MsMemberService.class);
	  @Autowired
	  private JdbcTemplate jdbcTemplate;
	
	  @Autowired
	  private IMsMsgService msMsgService;
	
	  @Transactional
	  public void addPoints( String userid, String bizId, Double points) {
		this.jdbcTemplate
				.update("insert into biz_membership (userId,bizId,points, status) values(?,?,?,?)",
						new Object[] { userid, bizId, points, AppConstant.CONFIRMING });
		//短信通知
		msMsgService.sendMsg("18210260858", "获取积分2.5", UUID.randomUUID().toString());
	  }
	
	  @Transactional
	  public void confirmPoints(TccTransactionContext context, String userid, String bizId, Double points) {
		this.jdbcTemplate.update(
				"update  biz_membership set status=?  where bizId=?",
				new Object[] { AppConstant.CONFIRMED, bizId });
	  }
	
	  @Transactional
	  public void cancelPoints(TccTransactionContext context, String userid, String bizId, Double points) {
		this.jdbcTemplate.update(
				"update  biz_membership set status=?  where bizId=?",
				new Object[] { AppConstant.DELETED, bizId });
	  }
	  
     }




        /**
        * 酒店预订服务
        * @author Administrator
        *
        */
      @Service("msHotelService")
      public class MsHotelServicele implements IMsHotelService {
	  private static final Logger logger = LoggerFactory
			.getLogger(MsHotelServicele.class);
	  @Autowired
	  private JdbcTemplate jdbcTemplate;
	  @Autowired
	  IMsCarService msCarService;
	  @Autowired
	  IMsDinnerService msDinnerService;

	@Transactional
	public void orderHotel(HotelOrder dto) {
			this.jdbcTemplate
					.update("insert into biz_hotelorder(hotelName,roomNo,start,end,userId,userName,status,hotelOrderId) values(?,?,?,?,?,?,?,?)",
							new Object[] { dto.getHotelName(), dto.getRoomNo(),
									dto.getStart(), dto.getEnd(),
									dto.getUserId(), dto.getUserName(),
									dto.getStatus(), dto.getHotelOrderId() });
		CarOrder car = new CarOrder();
		car.setCarOrderId(UUID.randomUUID().toString());
		car.setCarType("Benz");
		car.setEnd(new Date().toString());
		car.setStart(new Date().toString());
		car.setStatus(AppConstant.CONFIRMING);
		car.setUserId(dto.getUserId());
		car.setUserName(dto.getUserName());
		this.msCarService.orderCar(car);
		DinnerOrder dinner = new DinnerOrder();
		dinner.setDinnerOrderId(UUID.randomUUID().toString());
		dinner.setDinnerPlace("朝阳区慧忠北里老北京");
		dinner.setDinnerRoom("宴会厅");
		dinner.setDinnerTime(new Date().toString());
		dinner.setStatus(AppConstant.CONFIRMING);
		dinner.setUserId(dto.getUserId());
		dinner.setUserName(dto.getUserName());
		this.msDinnerService.orderDinner(dinner);
	}

	  @Transactional
	  public void confirmHotel(TccTransactionContext context, HotelOrder dto) {
		this.jdbcTemplate.update(
				"update biz_hotelorder set status=? where hotelOrderId=?",
				new Object[] { AppConstant.CONFIRMED, dto.getHotelOrderId() });
	  }
	
	  @Transactional
	  public void cancelHotel(TccTransactionContext context, HotelOrder dto) {
		this.jdbcTemplate.update(
				"update biz_hotelorder set status=? where hotelOrderId=?",
				new Object[] { AppConstant.DELETED, dto.getHotelOrderId() });
	  }
     }
   


       /**
        * 车辆预订服务
        * @author Administrator
        *
        */
      @Service("msCarService")
      public class MsCarService implements IMsCarService {
	  private static final Logger logger = LoggerFactory
			.getLogger(MsCarService.class);
	  @Autowired
	  private JdbcTemplate jdbcTemplate;
	  @Transactional
	  public void orderCar(CarOrder dto) {
			this.jdbcTemplate
					.update("insert into biz_carorder(carType,start,end,userId,userName,carOrderId,status) values (?,?,?,?,?,?,?)",
							new Object[] { dto
							.getCarType(), dto.getStart(), dto.getEnd(),
									dto.getUserId(), dto.getUserName(),
									dto.getCarOrderId(), dto.getStatus() });
	  }
	
	  @Transactional
	  public void confirmCar(TccTransactionContext context, CarOrder dto) {
		this.jdbcTemplate.update(
				"update biz_carorder set status=? where carOrderId=?",
				new Object[] { AppConstant.CONFIRMED, dto.getCarOrderId() });
	  }
	
	  @Transactional
	  public void cancelCar(TccTransactionContext context, CarOrder dto) {
		this.jdbcTemplate.update(
				"update biz_carorder set status=? where carOrderId=?",
				new Object[] { AppConstant.DELETED, dto.getCarOrderId() });
	  }

     }




       /**
        * 餐饮预订服务
        * @author Administrator
        *
        */
      @Service("msDinnerService")
      public class MsDinnerService implements IMsDinnerService {
	  private static final Logger logger = LoggerFactory
			.getLogger(MsDinnerService.class);
	  @Autowired
	  private JdbcTemplate jdbcTemplate;
	
	  @Transactional
	  public void orderDinner(DinnerOrder dto) {
		this.jdbcTemplate
				.update("insert into biz_dinnerorder (userId,userName,dinnerPlace,dinnerTime,dinnerRoom,status,dinnerOrderId) values(?,?,?,?,?,?,?)",
						new Object[] { dto

						.getUserId(), dto.getUserName(), dto.getDinnerPlace(),
								dto.getDinnerTime(), dto.getDinnerRoom(),
								dto.getStatus(), dto.getDinnerOrderId() });
	   }
	
	
	   @Transactional
	   public void confirmDinner(TccTransactionContext context, DinnerOrder dto) {
		this.jdbcTemplate.update(
				"update biz_dinnerorder set status=? where dinnerOrderId=?",
	 			new Object[] { AppConstant.CONFIRMED, dto.getDinnerOrderId() });
	   }
	
	   @Transactional
	   public void cancelDinner(TccTransactionContext context, DinnerOrder dto) {
		this.jdbcTemplate.update(
				"update biz_dinnerorder set status=? where dinnerOrderId=?",
				new Object[] { AppConstant.DELETED, dto.getDinnerOrderId() });
	   }
       }



       /**
       * 异步服务，测试事务边界
       * @author Administrator
       *
       */
       @Service("msMsgService")
       public class MsMsgService implements IMsMsgService {
	   private static final Logger logger = LoggerFactory
			.getLogger(MsMsgService.class);
	   @Autowired
	   private JdbcTemplate jdbcTemplate;
	
	   @Autowired
	   private IMsMsgFeeService msMsgFeeService;
	   @Transactional
	   public void sendMsg(String phone, String msg, String bizId) {
		this.jdbcTemplate
				.update("insert into biz_msg (phone,msg, status,bizId) values(?,?,?,?)",
						new Object[] { phone, msg, AppConstant.CONFIRMING, bizId });
		//短信发送成功后，需要通知计费服务记录通道短信费用
		
		msMsgFeeService.MsgFee(UUID.randomUUID().toString(), 3.6d);
		
	    }
	
	   @Override
	   @Transactional
	   public void confirmMsg(TccTransactionContext context, String phone, String msg, String bizId) {
		this.jdbcTemplate.update(
				"update  biz_msg set status=?  where bizId=?",
				new Object[] { AppConstant.CONFIRMED, bizId });
	    }
	  
	    @Override
	    @Transactional
	    public void cancelMsg(TccTransactionContext context, String phone, String msg, String bizId) {
		this.jdbcTemplate.update(
				"update  biz_msg set status=?  where bizId=?",
				new Object[] { AppConstant.DELETED, bizId });
	   }

      }



     /**
        * 短信计费服务
        * @author Administrator
        *
        */

      @Service("msMsgFeeService")
      public class MsMsgFeeService implements IMsMsgFeeService {
	  private static final Logger logger = LoggerFactory
			.getLogger(MsMsgFeeService.class);
	  @Autowired
	  private JdbcTemplate jdbcTemplate;
	  @Transactional
	  public void MsgFee(String bizId, Double fee) {
		this.jdbcTemplate
				.update("insert into biz_msgfee (bizId,fee, status) values(?,?,?)",
						new Object[] { bizId, fee, AppConstant.CONFIRMING });
	}
	
	@Transactional
	public void  confirmMsgFee(TccTransactionContext context, String bizId, Double fee) {
		this.jdbcTemplate.update(
				"update  biz_msgfee set status=?  where bizId=?",
				new Object[] { AppConstant.CONFIRMED, bizId });
	}
	
	
	@Transactional
	public void  cancelMsgFee(TccTransactionContext context, String bizId, Double fee) {
		this.jdbcTemplate.update(
				"update  biz_msgfee set status=?  where bizId=?",
				new Object[] { AppConstant.DELETED, bizId });
	}
      }
```

* 服务型消费方调用业务业务接口
```
      @Service("webApiService")
      public class WebApiService {
	  @Autowired
	  private IOrderService orderService;
	  @Autowired
	  private JdbcTemplate jdbcTemplate;

	  @Transactional
	  @CCTransactional(cancel = "cancel")
	  public void placeOrder(TourOrder dto) {
			String restore = CCTransactionUtils.getContext(context);
		this.orderService.order(dto);
		this.jdbcTemplate.update("insert into biz_send(content) values(?)",
				new Object[] { dto.toString() });
	  }
 
	  public void cancel(TourOrder dto) {
	  }
      }
```