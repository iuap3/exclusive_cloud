## TccTransaction分布式事务框架·用户指南

## 产品介绍

TccTransaction分布式事务框架是解决微服务开发环境中，事务一致性问题而提出的一种解决方案，与传统TCC分布式事务一致性方案不同，
TccTransaction是一个没有事务协调器、没有事务中心概念，事务由各个参与者共同完成整个事务的管理，是用友微服务治理平台提供的
一个核心功能组件。
## 与传统tcc的优缺点
* 优点
	1. 使用简单，业务只需要使用注解，并提供共confirm及cancel方法即可
	2. 没有事务协调器，各节点平等参与事务，各节点记录自己的事务信息，避免了中心服务器的瓶颈
	3. 由业务节点自行confirm或cancel
	4. 非常适合微服务调用链事务一致性

* 缺点
	1. 由于没有事务协调器，事务信息分散在各业务节点，不利于全局事务监控
	2. 需要各业务节点向全局事务控制平台上报各自异常的事务信息，有可能出现在控制台看不到完整的事务参与者信息


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
* 事务边界：
	1. 第一个rpc 服务endpoint，事务开始于服务端，服务端同步调用其它服务时处于同一个全局事务 
	2. 异步调用，异步调用是事务起点
	3. sagas与tcc事务混用时，两个服务之间调用如果调用端和服务端用了不一样的事务框架，则，被调用端开启新事务
*  confirm及cancel方法必须确保幂等性操作
*  rpc重试机制，如果出现超时及网络问题，导致业务重试，每次重试都会向后继续调用，后续调用可能成功或失败，每次调用后都会根据当前全局事务状态来确定重试返回时是提交还是取消事务状态

## 数据库模型:
Tcc事务框架要求每个事务节点各自记录事务信息，通过以下两个表记录事务及事务调用关系
<br>**1.tcctransation表：记录当前业务参与的事务上下文及状态信息**
<table border="1px" align="left" bordercolor="black" width="80%">
<tr align="left"><td>字段名字</td><td>字段类型</td><td>备注</td></tr>
<tr align="left"><td>gt_id</td> <td>varchar</td> <td>全局事务id，参与事务的各节点通过gt_id来标识一次全局事务</td> </tr>
<tr align="left"><td>tx_id</td> <td>varchar</td> <td>当前节点事务id</td> </tr>
<tr align="left"><td>ptx_id</td> <td>varchar</td> <td>上级事务id</td> </tr>
<tr align="left"><td>status</td> <td>varchar</td> <td>事务状态，记录当前节点在全局事务中所处的状态，取值为以下四种
CONFIRMING（进入事务方法时的状态）
FAILED（业务方法异常时的状态）
CANCEL(成功cancel后的状态)
CONFIRMED(成功确认后的状态)
</td> </tr>
<tr align="left"><td>type</td> <td>varchar</td> <td>事务类型：ROOT，起点， BRANCH参与节点</td> </tr>
<tr align="left"><td>context</td> <td>varchar</td> <td>存储事务开始时用户存储当前事务的信息，在confirm或cancel方法中可以取出来</td> </tr>
<tr align="left"><td>invocation</td> <td>blob</td> <td>业务方法的Rpc调用上下文信息</td> </tr>  
 
<tr align="left"><td>service_name</td> <td>varchar</td> <td>发起调用的服务名</td> </tr>  
<tr align="left"><td>interface_name</td> <td>varchar</td> <td>发起调用的接口名</td> </tr>  
<tr align="left"><td>method_name</td> <td>varchar</td> <td>发起调用的方法</td> </tr>  
<tr align="left"><td>pk</td> <td>bigint</td> <td>自增主键</td> </tr>  
</table>

<br>**2.tcctransation_rel表：记录当前事务中调用的下级事务关系上下文及状态**
<table border="1px" align="left" bordercolor="black" width="80%">
<tr align="left"><td>字段名字</td><td>字段类型</td><td>备注</td></tr>
<tr align="left"><td>gt_id</td> <td>varchar</td> <td>全局事务id，参与事务的各节点通过gt_id来标识一次全局事务</td> </tr>
<tr align="left"><td>tx_id</td> <td>varchar</td> <td>当前节点事务id</td> </tr>
<tr align="left"><td>ptx_id</td> <td>varchar</td> <td>上级事务id</td> </tr>
<tr align="left"><td>call_service_name</td> <td>varchar</td> <td>调用方服务名</td> </tr>
<tr align="left"><td>call_interface_name</td> <td>varchar</td> <td>调用方接口名</td> </tr>
<tr align="left"><td>call_method_name</td> <td>varchar</td> <td>调用方服务名</td> </tr>
<tr align="left"><td>service_name</td> <td>varchar</td> <td>被调用方服务名</td> </tr>
<tr align="left"><td>interface_name</td> <td>varchar</td> <td>被调用接口</td> </tr>  
<tr align="left"><td>method_name</td> <td>varchar</td> <td>被调用方法名</td> </tr>  
<tr align="left"><td>cancel</td> <td>int</td> <td>值为1时，已经发起cancel，避免重复调用</td> </tr>  
<tr align="left"><td>confirm</td> <td>int</td> <td>值为1时，已经发起confirm，避免重复调用</td> </tr>  
<tr align="left"><td>confirm_invocation</td> <td>varchar</td> <td>Confirm的rpc上下文</td> </tr>  
<tr align="left"><td>confirm_method</td> <td>varchar</td> <td>Confirm方法</td> </tr>  
<tr align="left"><td>cancel_invocation</td> <td>varchar</td> <td>Cancel的rpc上下文</td> </tr>  
<tr align="left"><td>cancel_method</td> <td>varchar</td> <td>Cancel方法</td> </tr> 
<tr align="left"><td>pk</td> <td>bigint</td> <td>自增主键</td> </tr> 
<tr align="left"><td>parent_pk</td> <td>bigint</td> <td>上级事务pk</td> </tr> 
<tr align="left"><td>invocation</td> <td>blob</td> <td>rpc上下文</td> </tr> 
</table>
<br><br><br>
*  要使用tcc事务，需要引入sdk、eos及tcc框架，且需要在业务数据库中创建eos和tcc相关的数据库表
   引入eos框架后，应用启动后悔自动创建以下几个数据库表：<br/>
   tm_locks<br/>
   tm_mqerror<br/>
   tm_mqrecv_error<br/>
   tm_mqrecv_success<br/>
   tm_mqsend_error<br/>
   tm_mqsend_success_20181216<br/>
   .....<br/>
   其中tm_mqsend_success_*用了分表策略，由eos框架自动提前创建，默认提前创建十天的表<br/>

   tcc框架会自动在业务数据库中创建数据库表<br/>
   tcc_transaction_20181216<br/>
   .........<br/>
   tcc_transaction_20181226<br/>
   .........<br/>
   其中tcc_transaction_*与tcc_transaction_rel*用了分表策略，由tcc框架自动提前创建，默认提前创建十天的表

## TccTransaction事务场景
* TccTransaction分布式事务框架解决rpc链式同步调用事务一致性
![](./images/cc-1.png)  


