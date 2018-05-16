# 调度任务开发

**业务场景</br>**
应用程序中经常会用到跑定时任务的需求，比如定时垃圾回收。有关任务调度需求有时候很复杂，如每隔多长时间重复执行，一个任务在不同时间段执行等。</br>
应用支撑组件-调度任务组件提供对Quartz的封装，提供独立的任务调度服务。避免业务系统直接配置Quartz的带来的性能消耗和集群环境的并发问题。 业务系统通过Rest服务添加任务，任务调动执行时回调业务系统的URL启动任务。</br>
在本文档中提供了调度组件配置、开发的示例。</br>
业务需求：定时查询已封存客户信息。</br>
## 1、环境准备</br>
新建项目以及数据库配置过程，请参考《应用平台环境搭建》文档。</br>

### 1.1、war包配置</br>
下面说明获取到iuap-saas-dispatch-service.war后，需要修改的配置文件。培训现场环境已提供，不需要修改。直接跳转到1.2章节</br>
1.	修改dispatch-server.properties中调度服务的配置</br>
  ![](/articles/application/4-/images/17/image2.png)
  
2.	修改auth.properties中redis的配置</br>
  ![](/articles/application/4-/images/17/image3.png)
   
3.	修改jdbc.properties中的数据库配置</br>
   ![](/articles/application/4-/images/17/image4.png)
   
配置数据类型</br>
   ![](/articles/application/4-/images/17/image5.png)
   
4.	如果与消息中心对接，需要修改msg-sdk.properties中消息中心和认证文件的配置，认证文件路径配置为本机authfile.txt的地址</br>
   ![](/articles/application/4-/images/17/image6.png)
   
5.	修改sdk.properties中客户认证路径为本机路径</br>
客户端身份文件路径</br>
client.credential.path=d:/iuap_ieap/authfile.txt

6.	配置workbench-sdk.properties</br>
配置工作台服务地址、客户认证路径
   ![](/articles/application/4-/images/17/image7.png)
   
### 1.2、数据库脚本配置</br>
#### 1.2.1 基础脚本导入</br>
调度任务基础表在培训环境中已存在。</br>
如果表不存在，则需要执行安装盘中war包提供脚本。</br>
CREATE TABLE dispatch_group(
     id VARCHAR(40) NOT NULL,
	 sysid VARCHAR(40),/* 应用id*/
	 tenantid VARCHAR(40),/* 租户id*/
	 name VARCHAR(200) NOT NULL,/*分组名称*/
     PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

#### 1.2.2 注册任务分组</br>
设定任务模板分组dispatch_taskwayclass
   ![](/articles/application/4-/images/17/image8.png)
   
注意sysid要和部署各组件中的配置一致，tenantid要和登录系统的人的tenantid一致。</br>
#### 1.2.3 注册任务规则</br>
预置任务规则表dispatch_taskway</br>
   ![](/articles/application/4-/images/17/image9.png)
   
注意： 
a)	classname的设定必须配置为：com.yonyou.iuap.dispatch.server.recall.HttpRecall</br>
b)	taskwayclassid在dispatch_taskwayclass里必须存在。</br>
c)	模板分组dispatch_taskwayclass和dispatch_taskway中的任务是分组和任务的1：N关系。一个“客户定时任务”下面可以根据条件设定不同的定时任务。</br>
**d)	需要把此url路径从shiro的拦截中去掉。具体做法为：在shiro的配置文件中，针对shiroFilter的filterChainDefinitions属性添加如下设定：**</br>
/customer/queryNum = anon   如下图所示：
   ![](/articles/application/4-/images/17/image10.png)
   
#### 1.2.4 设定任务参数</br>
在表dispatch_taskparam里添加执行任务所需要的参数。如果任务不需要参数，此表无需设定。</br>
在当前案例中，传入参数isfrozen字段，如果为Y则查询“封存”客户，如果为N，则查询正常客户</br>
   ![](/articles/application/4-/images/17/image11.png)
   
至此，环境配置结束，可以写代码来实现业务功能了。</br>
## 2、代码开发示例</br>
### 2.1、Controller</br>
Controller的职责是：接收任务调度组件传过来的参数，根据参数执行业务功能并返回执行结果给任务调度组件。</br>
**要点1：接收和返回的结果都是Map<String, Object>类型的。并且，返回结果需要满足以下规范：
调用接口返回json的格式。**</br>
{"success":"true","resultValue":"logContent","sendMsgContent":"msgContent","asynchronized":"false"}
success指是否执行成功，如果成功，"success":"true"，不成功，就不要返回
logContent指日志的内容
sendMsgContent指配置了消息模板，给消息接收人的内容。
asynchronized指是否是异步调用。

如果不成功，返回格式如下，
{"resultValue":"logContent","sendMsgContent":"msgContent","asynchronized":"true"}

**要点2：controller的url路径要和1.2.3中在表dispatch_taskway中设定的路径一致。**

示例代码如下</br>
CustomerController</br>
@RestController</br>
@RequestMapping(value = "/customer")</br>
public class CustomerController extends BaseController {</br>

    @Autowired
    private CustomerDispatchService dispatch;

    @RequestMapping(value = "/queryNum", method = RequestMethod.POST)
    public @ResponseBody Object queryNumforDispatch(@RequestBody Map<String, Object> params) {
    	logger.debug("execute queryNum operate.");
        
		Map<String,String> paramMap=new HashMap<String,String>();		
		for(String key :params.keySet()){
			if("tasklogid".equals(key)||"".equals(key))
			{
				continue;
			}
			Object value=params.get(key);
				
			if(value instanceof String&&value!=null)
			{
				String strParam = (String) value;
				paramMap.put(key, strParam);
			}
		}
		
		Map<String,Object> map=new HashMap<String, Object>();
		try {			
			int num=dispatch.queryNum(paramMap);			
	        map.put("success","true");
			String strnum=Integer.toString(num);
	        map.put("resultValue", "查询到的订单总数为"+strnum);
	        map.put("sendMsgContent", "订单总数为："+strnum);
	        map.put("asynchronized","false");	       
		} catch (BusinessException e) {
			logger.error("订单量查询失败");
			map.put("success","false");
			map.put("resultValue", "error");
	        map.put("sendMsgContent", "error");
	        map.put("asynchronized","false");
		}
		 return map;
    }

### 2.2、Service
package com.yonyou.train.customer.service;

import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;</br>
import org.springframework.stereotype.Service;

import com.yonyou.train.customer.dao.CustomerDao;

@Service</br>
public class CustomerDispatchService {

	@Autowired
	CustomerDao dao;

	public int queryNum(Map<String, String> paramMap) {
		Object value = paramMap.get("isfrozen");
		String sql = " select * from train_customer ";
		if(value!=null){
			sql = sql +" where ";
			String isfrozen = (String) value;
			if(isfrozen.equals("Y")){
				sql = sql +" enablestatus=0 ";
			}else{
				sql = sql + " enablestatus=1 ";
			}
		}
		return dao.queryNum(sql);
	}
	
}

### 2.3、dao
	public int queryNum(String sql) {
		List<Customer> list = dao.queryByClause(Customer.class, sql);
		if(list!=null&&list.size()>0){
			return list.size();
		}
		return 0;
	}

## 3、任务配置</br>
使用系统管理员admin登录应用平台环境，打开“调度任务管理“节点
   ![](/articles/application/4-/images/17/image12.png)
   
### 3.1 新建任务分组</br>
	双击打开“调度任务管理”界面，如果缺少脚本，会无法打开界面，从后台提示解决问题。</br>
   ![](/articles/application/4-/images/17/image13.png)
   
这步操作做出来的任务分组保存在表dispatch_group里</br>
   ![](/articles/application/4-/images/17/image14.png)
   
点击确定后，点击新建任务，创建调度任务</br>
如果之前创建过任务分组的话直接跳转到下面页面。</br>
### 3.2 创建任务</br>
   ![](/articles/application/4-/images/17/image15.png)
   
新建任务
   ![](/articles/application/4-/images/17/image16.png)
   
调用规则选择我们在库里配置的条件URL。</br>
参数：可修改“值”</br>
消息接收：如果配置了消息中心，则可配置消息接收人。</br>
定时规则：设定条件执行规则</br>
   ![](/articles/application/4-/images/17/image17.png)

参数修改后，会保存在下面表中dispatch_task_recallparam</br>
点击确定，开始按照配置的定时规则执行，执行结果显示在界面。</br>
   ![](/articles/application/4-/images/17/image18.png)
   
## 4、应用效果
执行后结果会显示在界面上</br>
   ![](/articles/application/4-/images/17/image18.png)
   
点击日志，会展示定时日志信息</br>
   ![](/articles/application/4-/images/17/image19.png)
   
点击“查看详情”，可以查看代码输出的返回结果</br>
   ![](/articles/application/4-/images/17/image20.png)
