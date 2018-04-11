# UI后端框架 #

## 简介 ##
此组件是iUAP前端框架的后端适配组件，和spring MVC配合，接收并处理前端UI datatable发送的数据，拥有独立的上下文处理和序列化框架，对事件、多语、异常、参数传递等做了封装，方便了开发者对前后端数据的对接。

Datatable控制器是一个特殊的spring MVC 控制器，与普通的Spring MVC控制器的区别有两点：

- 可以通过参数来获取Datatable 相关的信息，如Event、EventRequest，EventResponse，Datatable 。

- 输出类型必须是JSON，返回对象必须是EventResponse.

## 配置和使用方式 ##

**1:maven工程的配置文件pom.xml中加入对iweb组件的依赖**

	<dependency>
            <groupId>com.yonyou.iuap</groupId>
            <artifactId>iuap-iweb</artifactId>
            <version>${iuap.modules.version}</version>
            <exclusions>
                <exclusion>
                    <artifactId>spring-webmvc</artifactId>
                    <groupId>org.springframework</groupId>
                </exclusion>
                <exclusion>
                    <artifactId>cglib-nodep</artifactId>
                    <groupId>cglib</groupId>
                </exclusion>
            </exclusions>
        </dependency>

${iuap.modules.version} 为平台在maven私服上发布的组件的version。

**2:在工程的web.xml中配置filter**

	<filter>
	    <filter-name>IWebPlatformFilter</filter-name>
	    <filter-class>com.yonyou.iuap.iweb.platform.IWebPlatformFilter</filter-class>
	</filter>
	<filter-mapping>
	    <filter-name>IWebPlatformFilter</filter-name>
	    <servlet-name>springServlet</servlet-name>
	</filter-mapping>

**3:在工程的spring MVC的配置文件中加入mvc:annotation-driven配置，示例如下：**

	<mvc:annotation-driven>
		<mvc:message-converters register-defaults="true">
			<!-- 将StringHttpMessageConverter的默认编码设为UTF-8 -->
			<bean class="org.springframework.http.converter.StringHttpMessageConverter">
		    	<constructor-arg value="UTF-8" />
			</bean>
			<!-- 将Jackson2HttpMessageConverter的默认格式化输出设为true -->
			<bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
                <property name="prettyPrint" value="true"/>
            </bean>			
  		</mvc:message-converters>
  		<mvc:argument-resolvers>
  			<bean class="com.yonyou.iuap.iweb.datatable.handler.IWebHandlerMethodArgumentResolver">
  		</mvc:argument-resolvers>
	</mvc:annotation-driven>

    <bean id="iwebExceptionHandler" class="com.yonyou.iuap.iweb.exception.WebExceptionHandler"/>

**4:spring MVC的controller类的方法中，对接前台发送的数据**

	@RequestMapping(value = "/page", method = RequestMethod.GET)
	public EventResponse loadData(@IWebParameter(id = "dataTable1") DataTable<GoodJdbcDemo> dataTable1, @IWebParameter() EventResponse response) {
		try {
			int pageNumber = 0;
			Map<String, Object> parameters = IWebViewContext.getEventParameter();
			if (parameters.get("pageIndex") != null) {
				pageNumber = (Integer) parameters.get("pageIndex");
			}
			int pageSize = dataTable1.getPageSize();
			pageNumber = dataTable1.getPageIndex();

			// 构造查询条件等
			Map<String, Object> searchParams = new HashMap<String, Object>();
			PageRequest pageRequest = ... ...


			Page<GoodJdbcDemo> categoryPage = goodService.getDemoPage(searchParams, pageRequest);
			
			//设置返回数据
			dataTable1.remove(dataTable1.getAllRow());
			dataTable1.set(categoryPage.getContent().toArray(new GoodJdbcDemo[0]));

			int totalPages = categoryPage.getTotalPages();
			dataTable1.setPageIndex(pageNumber);
			dataTable1.setTotalPages(totalPages);

			... ... ... ... 
		} catch (Exception e) {
			logger.error("查询数据失败!", e);
			throw new WebRuntimeException("查询数据失败!");
		}
		return response;
	}

**5:前端对应的发送数据的方式如下:**

	app.serverEvent().addDataTable("dataTable1", "current").fire({
	
	    url : ctx+'/goods/page',
	
	    type:'GET',
	
	    success: function(){
	
	        ... ...
	
	    }
	);

**6：前后端完成示例如下：**

- java示例

		
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
			public EventResponse loadData(@IWebParameter(id="dataTable1") DataTable<GoodJdbcDemo> dataTable1, @IWebParameter()
			EventResponse response) {
				try {
					int pageNumber = 0;
					Map<String, Object> parameters = IWebViewContext.getEventParameter();
					if (parameters.get("pageIndex") != null) {
						pageNumber = (Integer) parameters.get("pageIndex");
					}
					int pageSize = dataTable1.getPageSize();
					pageNumber = dataTable1.getPageIndex();
		
					// 构造查询条件等
					Map<String, Object> searchParams = new HashMap<String, Object>();
					PageRequest pageRequest = ...
		
		
					Page<GoodJdbcDemo> categoryPage = goodService.getDemoPage(searchParams, pageRequest);
					
					//设置返回数据
					dataTable1.remove(dataTable1.getAllRow());
					dataTable1.set(categoryPage.getContent().toArray(new GoodJdbcDemo[0]));
		
					int totalPages = categoryPage.getTotalPages();
					dataTable1.setPageIndex(pageNumber);
					dataTable1.setTotalPages(totalPages);
		
					... ... ... ... 
				} catch (Exception e) {
					logger.error("查询数据失败!", e);
					throw new WebRuntimeException("查询数据失败!");
				}
				return response;
			}
			
					
		
			@RequestMapping(value="/update", method= RequestMethod.POST)
			public EventResponse update(@IWebParameter(id="dataTable1") DataTable<GoodJdbcDemo> dataTable1, @IWebParameter()
			EventResponse response) {
				dataTable1.setCls("com.yonyou.iuap.example.entity.GoodJdbcDemo");
				try {
					//查找出需要保存或者更新的行

					... ... 

					GoodJdbcDemo entity = goodService.saveEntity(demoEntity);
					
					... ...

					JSONObject json = new JSONObject();
					json.put("flag", "success");
					json.put("msg", "保存成功!");
					IWebViewContext.getResponse().write(json.toString());
				} catch (Exception e) {
					logger.error("保存失败!",e);
					throw new WebRuntimeException("保存失败!", e);
				}
				return response;
			}
			
			@RequestMapping(value="/del", method= RequestMethod.POST)
			public EventResponse del(@IWebParameter(id="dataTable1") DataTable<GoodJdbcDemo> dataTable1, @IWebParameter()
			EventResponse response) {
				try {
					Row[] rows = dataTable1.getSelectRow();
					Iterator<Row> it = Arrays.asList(rows).iterator();
					while (it.hasNext()) {
						Row r = it.next();
						
						String productid = (String) r.getField("productid").getValue();
						if (productid!=null) {
							//此处只为示例，应该考虑批量删除
							goodService.deleteById(productid);
						}
					}
					dataTable1.remove(rows);
					
					dataTable1.setSelect(new Integer[]{});
					dataTable1.setTotalRow(goodService.getAllCount());
				} catch (Exception e) {
					logger.error("删除报错失败!",e);
					throw new WebRuntimeException("删除失败!",e);
				}
				return response;
			}
			
			private Map<String, Object> createSearchParamsStartingWith(DataTable dataTable1,String prefix) {
				Map<String, Object> params = new HashMap<String, Object>();
				Map<String, Object> m = dataTable1.getParams();
				Set<Entry<String, Object>> set = m.entrySet();
				for (Entry<String, Object> entry : set) {
					String key = entry.getKey();
					if (key.startsWith(prefix)) {
						String unprefixed = key.substring(prefix.length());
						params.put(unprefixed, entry.getValue().toString());
					}
				}
				return params;
			}
			
			/**
			 * 创建分页请求.
			 */
			private PageRequest buildPageRequest(int pageNumber, int pagzSize, String sortType) {
				Sort sort = null;
				if ("auto".equals(sortType)) {
					sort = new Sort(Direction.DESC, "ts");
				} else if ("productname".equals(sortType)) {
					sort = new Sort(Direction.ASC, "productname");
				}
				return new PageRequest(pageNumber, pagzSize, sort);
			}
		}

- js示例,请根据具体业务需求修改

		define(['jquery', 'knockout', 'text!pages/goods/page.html', 'uui'], function($, ko, template) {
		
			var app, viewModel, datas;
			viewModel = {
				md: document.querySelector('#demo-mdlayout'),
				editoradd: '',
				dataTable1: new u.DataTable({
					meta: {
						'productid':{
							type:'string'
						},
						'productName': {
							type: 'string'
						},
						'productNum': {
							type: 'integer'
						},
						'price': {
							type: 'float'
						},
						'supplier': {
							type: 'string'
						}, //供应商
						'proDate': {
							type: 'date'
						}, //生成日期
						'orgin': {
							type: 'string'
						} //原产地
					},
					pageSize: 10
				}),
				infodata: new u.DataTable({
					meta: {
						'productid':{
							type:'string'
						},
						'productName': {
							type: 'string'
						},
						'productNum': {
							type: 'integer',
							precision: 0,
							min: 0
						},
						'price': {
							type: 'float',
							precision: 2,
							min: 0
						},
						'supplier': {
							//供应商
							type: 'string'
						},
						'proDate': {
							//生成日期
							type: 'date'
						}, 
						'orgin': {
							//原产地
							type: 'string'
						} 
					}
				}),
				
			
				menuEvents: {
					afterAdd:function(element, index, data){
			            if (element.nodeType === 1) {
			                u.compMgr.updateComp(element);
			            }
			        },
					goBack: function() {
						viewModel.md['u.MDLayout'].dBack();
					},
					goPage: function(pathStr) {
						viewModel.md['u.MDLayout'].dGo(pathStr);
					},
					beforeAdd: function() {
						viewModel.editoradd = 'add';
						viewModel.infodata.clear();
						viewModel.infodata.createEmptyRow();
						viewModel.infodata.setRowSelect(0);
						viewModel.md['u.MDLayout'].dGo('addPage');
					},
					addRow: function() {
						var self = this;
						var _meta = this.dataTable1.meta;
						var addInfo = this.infodata.getAllRows();
						if (this.editoradd === 'add') {
							this.dataTable1.addSimpleData( this.infodata.getSimpleData(),'new');
						} else {
							var curRow = viewModel.dataTable1.getCurrentRow();
							curRow.setSimpleData(viewModel.infodata.getCurrentRow().getSimpleData(), 'upd');
						}
						
						app.serverEvent().addDataTable("dataTable1", "current").fire({
							url : ctx+'/goods/update'
							})
						viewModel.md['u.MDLayout'].dBack();
						u.compMgr.updateComp();
					},
					
					beforeEdit: function(id) {
						viewModel.dataTable1.setRowSelect(id);
						viewModel.infodata.setSimpleData(viewModel.dataTable1.getSimpleData({
							type: 'select'
						}));
						viewModel.editoradd = 'edit';
						viewModel.md['u.MDLayout'].dGo('addPage');
					},
					
					delRow: function() {
						var selectArray = viewModel.dataTable1.selectedIndices();
						if (selectArray.length < 1) {
							u.messageDialog({
								msg: "请选择要删除的行!",
								title: "提示",
								btnText: "OK"
							});
							return;
						}
						
						app.serverEvent().addDataTable("dataTable1", "current").fire({
							url : ctx+'/goods/del',
							success:function(){
								viewModel.comp.update({totalPages: viewModel.dataTable1.totalPages(),pageSize:viewModel.dataTable1.pageSize(),currentPage:viewModel.dataTable1.pageIndex()+1,totalCount:viewModel.dataTable1.totalRow()});
							}
						});
					},
					viewRow: function(id) {
						//如果样式列表不含有checkbox说明不是第一列
						if (event.target.classList.toString().indexOf('checkbox') < 0) { 
							viewModel.dataTable1.setRowFocus(id);
							viewModel.md['u.MDLayout'].dGo('showPage');
						}
					}
				}
		
			}
		
			var init = function(params) {
				app = u.createApp({
					el: '#content',
					model: viewModel
				});
				viewModel.md = document.querySelector('#demo-mdlayout');
				app.serverEvent().addDataTable("dataTable1", "current").fire({
					url : ctx+'/goods/page',
					type:'GET',
					success: function(){
						viewModel.comp.update({totalPages: viewModel.dataTable1.totalPages(),pageSize:viewModel.dataTable1.pageSize(),currentPage:viewModel.dataTable1.pageIndex()+1,totalCount:viewModel.dataTable1.totalRow()});
					}
						
				});
				
				var r = viewModel.infodata.createEmptyRow();
				viewModel.infodata.setRowSelect(0);
				//分页
				var element = document.getElementById('pagination');
			    var comp = new u.pagination({el:element,jumppage:true});
			    viewModel.comp=comp;
			    //更新分页的配置
			    comp.update({totalPages: 5,pageSize:2,currentPage:1,totalCount:35,});
			
			    comp.on('pageChange', function (pageIndex) {
			        //console.log('新的页号为' + pageIndex);
			        viewModel.dataTable1.pageIndex(pageIndex)
			        app.serverEvent().addDataTable("dataTable1", "current").fire({
						url : ctx+'/goods/page',
						type:'GET',
						success: function(){
							viewModel.comp.update({totalPages: viewModel.dataTable1.totalPages(),pageSize:viewModel.dataTable1.pageSize(),currentPage:pageIndex+1,totalCount:viewModel.dataTable1.totalRow()});
						}
							
					});
			    });
			
			    comp.on('sizeChange', function (arg) {
			        //console.log('每页显示条数为' + arg[0]);
			        viewModel.dataTable1.pageSize(arg[0]);
			        app.serverEvent().addDataTable("dataTable1", "current").fire({
						url : ctx+'/goods/page',
						type:'GET',
						success: function(){
							viewModel.comp.update({totalPages: viewModel.dataTable1.totalPages(),pageSize:arg[0],currentPage:viewModel.dataTable1.pageIndex()+1,totalCount:viewModel.dataTable1.totalRow()});
						}
					});
			    });
				
				window.viewModel = viewModel;
				u.compMgr.updateComp();
		
			}
		
			return {
				'model': viewModel,
				'template': template,
				'init': init
			};
		});

- 注意：上述示例代码只作为参考，项目上需要按照自身要求修改。

**7：更加详细的使用方法，请参考前端技术相关的使用手册**