#CCTransaction分布式事务框架·用户指南

## 产品介绍

CCTransaction分布式事务框架是解决微服务开发环境中，事务一致性问题而提出的一种解决方案，与TCC分布式事务一致性方案不同，
CCTransaction是一个没有事务协调器、没有事务中心概念，事务由各个参与者共同完成整个事务的管理，是用友微服务治理平台提供的
一个核心功能组件。


## 功能介绍

* 解决分布式事务一致性问题，业务使用分布式事务一致性功能只需两个注解，  @CCTransactional(cancel="cancelOrder")标识业务方法需要使用分布式事务
  cancel属性指定了业务异常时的补偿方法，与业务方法在同一个接口中实现，补偿方法上加入  @Async注解
* 基于可靠消息的异步调用框架（EOS）的最终一致性
* 支持链式rpc调用中各个业务节点的事务一致性
* 基于用友微服务治理平台rpc框架Iris
* 事务信息数据与业务库一起，保证业务与消息发送一致性，分布式事务数据记录由框架自行操作，用户无需关心



## CCTransaction事务场景
* CCTransaction分布式事务框架解决rpc链式同步调用事务一致性
![](./images/cc-1.png)


## CCTransaction事务管理控制台
* CCTransaction控制台为分布式事务控制管理提供了可视化的界面, 当分布式事务节点异常，导致事务最终不一致的时候提供人工介入处理的途径，可以通过该界面，查看当前
全局不一致的事务，并可以发起补偿操作


## CCTransaction事务异步消息
* 在使用CCTransaction框架前, 需要配置应用的RabbitMQ地址:
	* 页面导航: 微服务治理平台左边菜单 &gt;&gt;  应用管理 &gt;&gt; 找到对应的应用 &gt;&gt; 点进对应的环境 &gt;&gt; 微服务 &gt;&gt; 事务管理 &gt;&gt; 消息队列配置 &gt;&gt; 在输入框中填写RabbitMQ地址, 如: IP1:Port1,IP2:Port2; 参见下图:
![](./images/eos-console.png)
* 消息地址配置输入框:
![](./images/rabbitmq-config.png)

* 配置完消息地址后, 按照异步框架的开发指南进行工程配置, 若有异常会上报到EOS控制台; 按照以下步骤查询异常消息: <p>微服务治理平台左边菜单 &gt;&gt; 应用管理 &gt;&gt;找到对应的应用 &gt;&gt; 点进对应的环境 &gt;&gt; 微服务 &gt;&gt; **事务管理** &gt;&gt;</p>
* 点击重试后会下发命令到客户端进行重试, 客户端重试完成后会上报重试结果, 重试成功后会在状态列中显示"重试成功", 否则显示"重试失败".
* 点击忽略后会下发命令到客户端进行忽略, 被忽略的消息应交由人工处理, 进行数据的校对和检查.
![](./images/mq-process.png)


## CCTransaction事务使用

* 引入相关jar包

		<dependency>
			<groupId>com.yonyou.cloud.middleware</groupId>
			<artifactId>eos-spring-support</artifactId>
			<version>${mw.version}</version>
		</dependency>
		<dependency>
			<groupId>com.yonyou.cloud.middleware</groupId>
			<artifactId>cctrans-spring-support</artifactId>
			<version>${cctrans-spring-support.version}</version>
		</dependency>
		<dependency>
			<groupId>com.yonyou.cloud.middleware</groupId>
			<artifactId>mwclient</artifactId>
			<version>${mw.version}</version>
			<type>pom</type>
		</dependency>


* 编写业务接口，包括业务方法和补偿方法，通过分布式事务注解来实现

		/**
		* 订单服务
		* @author Administrator
		*
		*/
		@RemoteCall(AppConstant.APP_INFO_ORDERSERVICE)
		public interface IOrderService {
			@ApiOperation(value="下旅游订单", response=TourOrder.class)
			@CCTransactional(cancel="cancelOrder")
			public abstract TourOrder order(TourOrder paramTourOrder);
			  
			@ApiOperation(value="取消旅游订单", response=TourOrder.class)
			@Async
			public abstract TourOrder cancelOrder(TourOrder paramTourOrder);
		}


* 服务提供方实现业务接口

		public class OrderService implements IOrderService {
			private static final Logger logger = LoggerFactory
					.getLogger(OrderService.class);
			@Autowired
			private JdbcTemplate jdbcTemplate;
			@Autowired
			private IMsPlaneService msPlaneService;
			@Autowired
			private IMsHotelService msHotelService;
		
			@Transactional
			public TourOrder order(TourOrder dto) {
				this.jdbcTemplate
						.update("insert into biz_tourorder (orderName,userId,userName,dest,status,tourOrderId)values (?,?,?,?,?,?)",
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
				planeOrder.setStatus("NORMAL");
				this.msPlaneService.orderPlane(planeOrder);
				HotelOrder hotelOrder = new HotelOrder();
				hotelOrder.setStart(new DateTime().toString());
				hotelOrder.setEnd(new DateTime().toString());
				hotelOrder.setHotelName("锦江之星宾馆");
				hotelOrder.setHotelOrderId(UUID.randomUUID().toString());
				hotelOrder.setRoomNo("401");
				hotelOrder.setStatus("NORMAL");
				hotelOrder.setUserId(dto.getUserId());
				hotelOrder.setUserName(dto.getUserName());
				this.msHotelService.orderHotel(hotelOrder);
		
				return dto;
			}
		
			@Transactional
			public TourOrder cancelOrder(TourOrder dto) {
				this.jdbcTemplate.update(
						"update biz_tourorder set status=?  where tourOrderId=?",
						new Object[] { AppConstant.DELETED, dto.getTourOrderId() });
				return dto;
			}


* 服务型消费方调用业务业务接口

		@Service("webApiService")
		public class WebApiService {
			@Autowired
			private IOrderService orderService;
			@Autowired
			private JdbcTemplate jdbcTemplate;
		
			@Transactional
			@CCTransactional(cancel = "cancel")
			public void placeOrder(TourOrder dto) {
				this.orderService.order(dto);
				this.jdbcTemplate.update("insert into biz_send(content) values(?)",
						new Object[] { dto.toString() });
			}
		
			public void cancel(TourOrder dto) {
			}
		}
