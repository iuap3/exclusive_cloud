# 流程接入

 

## 1	业务背景
企业互联网化需要流程，其本质目标就是实现人、物、事全部互联的企业数字化业务。客户的数据和客户一样重要。数据的价值和在于连接和流动。流程连接你的客户和业务，驱动数据的流动。</br>
![](/articles/application/6-/images/04/image2.png)

  iUAP流程平台</br>
- iUAP 重塑流程平台在互联网时代的核心价值</br>
- 聚焦流程核心能力的组件化和微服务化，支持可伸缩的弹性技术架构，满足企业对互联网开发模式与性能的需求</br>
- 遵循BPMN2.0标准，可以支持的流程模型驱动的应用开发过程</br>
- 支持标准开放的运行时环境，基于SOA架构和REST风格思想，集成其他平台或中间件，可以帮助企业按需搭配组合平台，解锁平台锁定，增加平台的可控性</br>
- 丰富的多语言SDK和示例，帮助你快速开发</br>
- 内建企业级的审批应用扩展，支持企业级的审批应用</br>

 

## 2	环境准备
### 2.1	war包配置</br>
下面说明获取到流程中心iuap-eiap-bpm-service.war后，需要修改的配置文件。培训现场环境已提供，不需要修改。</br>
配置application.properties文件，流程平台提供的服务地址</br>
![](/articles/application/6-/images/04/image11.png) 

接下来说明获取到流程设计器ubpm-webserver-process-center2.war和流程服务ubpm-web-rest后，需要修改的配置文件。培训现场环境已提供，不需要修改。配置conf/db.properties文件，修改数据库配置

![](/articles/application/6-/images/04/image12.png)  

配置conf/cache.properties文件，修改缓存配置
 
![](/articles/application/6-/images/04/image13.png) 

### 2.2	开发环境配置</br>
配置pom.xml文件，添加eiap-plus-common依赖，目前最新版本3.3.0-SNAPSHOT

<dependency>
   <groupId>com.yonyou.iuap</groupId>
   <artifactId>eiap-plus-common</artifactId>
   <version>${module.version}</version>
</dependency>

<dependency>
	<groupId>com.yonyou.iuap.pap.bpm</groupId>
	<artifactId>ubpm-rest-sdk</artifactId>
	<version>3.3.0-SNAPSHOT</version>
</dependency>

配置application.properties文件，流程平台提供的服务地址

![](/articles/application/6-/images/04/image14.png) 

		
# 开发过程

### 2.3	流程图元介绍
 
### 2.4	流程管理</br>
#### 2.4.1	流程模型</br>
流程模型主要包括流程分组、流程设计、流程部署等</br>
- 新建、编辑、删除、设计流程模型，支持流程分组 </br>
- 设计调整好的流程模型可以部署运行 </br>
2.4.1.1	流程分组

![](/articles/application/6-/images/04/image45.png) 
 
新建流程时需要关联流程模型，流程模型的注册见下面章节

2.4.1.2	流程模型注册</br>
iuap_bpm_buzientity流程实体

![](/articles/application/6-/images/04/image46.png) 
 
iuap_bpm_buzimodel流程模型

![](/articles/application/6-/images/04/image47.png) 
 
2.4.1.3	流程设计</br>
基于H5的在线流程定义管理与设计

![](/articles/application/6-/images/04/image48.png) 
![](/articles/application/6-/images/04/image49.png) 
   
2.4.1.4	流程部署</br>
对设计好的流程发布，并进行多版本管理。

#### 2.4.2	流程监控</br>
流程监控主要流程查看，流程管理，如挂起、恢复、终止等</br>
- 快速查找、定位流程实例，以图形化方式展现流程实例执行情况及各流转节点详细信息</br>
- 支持流程实例的挂起、恢复、终止操作

### 2.5	流程对接

![](/articles/application/6-/images/04/image50.png) 
 
#### 2.5.1	业务开发</br>
业务单据开发与流程平台完全解耦，各业务系统独立设计。本案例以督办任务为例进行流程对接。</br>
列表界面增加提交、收回、查看详情功能。
 
![](/articles/application/6-/images/04/image51.png) 

1.	js中需要引入辅助类
”/eiap-plus/pages/flow/bpmapproveref/bpmopenbill.js”

2.	js中初始化参数需要传递cookie以及bpm相应信息如下图所示
3.	
 ![](/articles/application/6-/images/04/image52.png) 

3.	html中增加流程信息
4.	
 ![](/articles/application/6-/images/04/image53.png) 
![](/articles/application/6-/images/04/image54.png) 
         
2.5.1.1	提交功能</br>
2.5.1.1.1	html</br>
列表界面增加按钮定义</br>
<button id="unsubmitBtn" class="u-button u-button-primary" data-bind="click: event.submit">
					提交
</button>

2.5.1.1.2	js</br>
除了常规处理外，核心代码为调用运行平台增强包内的流程服务</br>
1)	调用URL，根据当前节点功能号、资源编码查询对应的流程定义</br>
               //审批流添加功能----提交审批
                submit: function () {
                    var selectArray = viewModel.ygdemo_yw_infoDa.selectedIndices();

……
                    $.ajax({
                        type: 'GET',
                        url: '/eiap-plus/appResAllocate/queryBpmTemplateAllocate?funccode=' + getAppCode() + '&nodekey=003',
                        datatype: 'json',
                        contentType: 'application/json;charset=utf-8',
                        success: function (result) {
	if(result){
		if(result.success=='success'){
                                    var data = result.detailMsg.data;
                                    if(data==null){
                                        u.messageDialog({ msg: "此数据在“资源分配”中未分配流程！", title: '操作有误', btnText: '确定' });
                                    }
                                    else{
                                        var processDefineCode = data.res_code;
viewModel.event.submitBPMByProcessDefineCode(selectedData,processDefineCode);
                                    }
			
		}else{
			u.messageDialog({
										msg : data.detailMsg.msg,
										title : "提示",
										btnText : "OK"
									});
		}

	}else {
                                u.messageDialog({msg: '无返回数据', title: '操作提示', btnText: '确定'});
                            }
                        }
	})
                },
2)	提交审批流</br>
调用后台服务，提交审批流</br>
提交的URL地址如下图：</br>
 ![](/articles/application/6-/images/04/image55.png) 

需要传入的参数为查询的资源号</br>
                submitBPMByProcessDefineCode:function(selectedData,processDefineCode){
	$.ajax({
                        type: "post",
                        url: viewModel.submiturl + "?processDefineCode=" + processDefineCode,
                        contentType: 'application/json;charset=utf-8',
                        data: JSON.stringify(selectedData),
                        success: function (res) {
if (res) {
if (res.success == 'success') {
	message("流程启动成功");
                                    viewModel.event.initUerList();
                                } else {
                                    u.messageDialog({msg: res.message, title: '操作提示', btnText: '确定'});
                                }
                            } else {
                                u.messageDialog({msg: '无返回数据', title: '操作提示', btnText: '确定'});
                            }
                        }

                    });
                },

2.5.1.1.3	Controller</br>
调用后台服务</br>
 ![](/articles/application/6-/images/04/image56.png) 

2.5.1.1.4	Service</br>
引入通用处理服务com.yonyou.iuap.bpm.service.BPMSubmitBasicService</br>
![](/articles/application/6-/images/04/image57.png) 
![](/articles/application/6-/images/04/image58.png) 
 ![](/articles/application/6-/images/04/image59.png) 
 
构造提交的流程数据BPMFormJSON</br>
 ![](/articles/application/6-/images/04/image98.png) 


如下代码中督办任务需要传入的参数</br>
 ![](/articles/application/6-/images/04/image60.png)
 ![](/articles/application/6-/images/04/image61.png)

 
其他变量的传入方式如下图所示，督办任务将主表所有信息作为其他变量传入

 ![](/articles/application/6-/images/04/image62.png)
 
调用处理服务，如果结果为成功则更新当前业务数据状态并返回结果</br>
2.5.1.2	收回功能</br>
2.5.1.2.1	html开发</br>
<button id="unsubmitBtn" class="u-button u-button-primary" data-bind="click: event.unsubmit"></br>
					收回
</button>

2.5.1.2.2	js开发

 ![](/articles/application/6-/images/04/image63.png)
 
2.5.1.2.3	Controller</br>
调用服务的收回功能</br>
  ![](/articles/application/6-/images/04/image64.png)

2.5.1.2.4	Service</br>
1)更新当前单据状态</br>
2)调用eiap加强包提供的收回服务</br>
3)参数为当前数据ID</br>
4)返回值：如果结果为成功则返回success，如果为失败，则返回message失败信息</br>
  ![](/articles/application/6-/images/04/image65.png)

2.5.1.3	查看详情</br>
增加查看详情功能</br>
1)	点击查看详情时，需要处理按钮卡片页面的编辑类按钮不显示。比如：保存按钮不显示【仅在编辑、新增时显示】、子表的编辑按钮不显示；</br>
2)	加载通用panel区域<div id=bpmhead></br>
3)	加入BPM相关按钮，调用通用处理bpmhandler.js的initBPMFromBill的方法，传入的参数为当前数据的ID</br>
viewModel.initBPMFromBill(viewModel.ygdemo_yw_infoDa.getValue("id"),viewModel);

 ![](/articles/application/6-/images/04/image66.png) 

2.5.1.3.1	html</br>

> <button id="viewPage" class="u-button u-button-primary" data-bind="click:
>  
> event.viewClick">

>查看详情				
>     </button>


2.5.1.3.2	js示例</br>
viewClick:function(viewModel,app){
                    viewModel.formStatus = _CONST.FORM_STATUS_EDIT;
                    var selectArray = viewModel.ygdemo_yw_infoDa.selectedIndices();

                    if (selectArray.length < 1) {
                        u.messageDialog({
                            msg: "请选择要编辑的记录!",
                            title: "提示",
                            btnText: "OK"
                        });
                        return;
                    }

                    if (selectArray.length > 1) {
                        u.messageDialog({
                            msg: "一次只能编辑一条记录，请选择要编辑的记录!",
                            title: "提示",
                            btnText: "OK"
                        });
                        return;
                    }

    //只显示返回和保存按钮
                    viewModel.event.userCardBtn();

    $("#save").hide();

    $(".ygdemo_yw_sub-actions").hide();   //子任务的增加和删除按钮隐藏

                    //获取选取行数据
                    viewModel.ygdemo_yw_infoDa.setRowSelect(selectArray);
                    viewModel.ygdemo_yw_infoFormDa.clear();
                    viewModel.ygdemo_yw_subFormDa.clear();
                    viewModel.ygdemo_yw_infoFormDa.setSimpleData(viewModel.ygdemo_yw_infoDa.getSimpleData({type: 'select'}));
                    viewModel.ygdemo_yw_subFormDa.setSimpleData(viewModel.ygdemo_yw_subDa.getSimpleData(), {unSelect: true});

                    viewModel.event.fileQuery();

                    //显示操作卡片
                    viewModel.md.dGo('addPage');

                    $('#addPage').each(function(index,element){
	$(element).find('input[type!="radio"]').attr('disabled',true);
                    });

    //加入bpm按钮</br>
    viewModel.initBPMFromBill(viewModel.ygdemo_yw_infoDa.getValue("id"),viewModel);
                },

2.5.1.4	流程图</br>
通用功能已包含，不需要开发，下面章节仅做开发介绍

 ![](/articles/application/6-/images/04/image67.png) 

2.5.1.5	审批功能</br>
参考bpmhandler.js中的bpmApproveHandler方法</br>
2.5.1.6	改派</br>
参考bpmhandler.js中的bpmDelegateOkHandler方法</br>
2.5.1.7	驳回或指派</br>
参考bpmhandler.js中的bpmRejectOkHandler方法</br>
2.5.1.8	查看流程图</br>
参考bpmhandler.js中的bpmApproveBrowseHandler方法</br>
2.5.1.9	其他编辑控制初始化</br>
相关编辑控制功能</br>
 ![](/articles/application/6-/images/04/image68.png) 

需要继承BPM的viewmodel</br>
初始化按钮权限</br>
![](/articles/application/6-/images/04/image69.png)  
![](/articles/application/6-/images/04/image70.png)  

 
2.5.1.10	流程动作回调</br>
流程中的一些动作结束后，需要回写业务单据。比如审批成功后回写业务单据状态为已完成等。</br>
在提交功能3.3.1.4章节中在提交审批流，传入的参数中包含serviceClass，即为回调的代码。</br>
回调类可以实现接口com.yonyou.iuap.bpm.web.IBPMBusinessProcessController，也可以直接继承通用类实现com.yonyou.iuap.bpm.web.BPMBussinessProcessController<T></br>
 ![](/articles/application/6-/images/04/image71.png)  

#### 2.5.2	功能管理</br>
业务单据开发完成需进行功能注册，开发订单管理节点的时候已经完成注册，这里略。</br>
2.5.3	资源分配</br>
资源分配主要完成业务功能和流程模型的关系映射，实现流程的动态配置。并进行BPM的启用、停用控制。</br>
 ![](/articles/application/6-/images/04/image72.png)  
## 3	应用效果

### 3.1	流程定义

 ![](/articles/application/6-/images/04/image73.png) 
 
点击设计功能</br>
 ![](/articles/application/6-/images/04/image74.png) 
 
在演示案例中，设置使用了抢占功能、条件分支、会签功能</br>

部门人员信息</br>
财务部人员</br>
 ![](/articles/application/6-/images/04/image75.png)  

销售部人员</br>
 ![](/articles/application/6-/images/04/image76.png)  

抢占功能配置</br>
 ![](/articles/application/6-/images/04/image77.png)  

条件分支</br>
 ![](/articles/application/6-/images/04/image78.png)  

条件2</br>
 ![](/articles/application/6-/images/04/image79.png)  

按照用户会签</br>
 ![](/articles/application/6-/images/04/image80.png)  

录入完成后，需要点击发布功能，发布流程。
 ![](/articles/application/6-/images/04/image81.png)  

### 3.2	提交
![](/articles/application/6-/images/04/image82.png)  
 
提交后提示触发流程，单据状态为执行中</br>
此时点击“查看详情”，切换到卡片页面</br>
![](/articles/application/6-/images/04/image83.png)  
 
点击流程图，可以查看审批流，因为当前人员没有审批权限，所以并没有审批相关按钮
 
![](/articles/application/6-/images/04/image84.png) 
 
### 3.3	单据审批
使用审批人登录，切到卡片页面后，出现如下图所示界面</br>
![](/articles/application/6-/images/04/image85.png)  
 
可以选择同意/不同意，输入审批意见，点击提交。同时可以选择查看流程图</br>
![](/articles/application/6-/images/04/image86.png)   

除了在业务单据上做审批动作，也可以选择在“任务中心”节点上做审批动作，参加下面章节。</br>
### 3.4	任务中心</br>
1.	任务中心页面</br>
“待办”：当前人员待处理的审批任务</br>
“已办”：当前人员已办理的审批任务</br>
如以上示例中，审批人员huahua01已经做过审批，流程继续，下一步审批人为“huahua01”或者”马素娟”。

![](/articles/application/6-/images/04/image87.png)   
  ![](/articles/application/6-/images/04/image88.png)  
2.	待办任务</br>
所以点击“待办”时，huahua01用户仍然能看到审批单据</br>
 ![](/articles/application/6-/images/04/image89.png)  

此时可以选择“审批”或者“打开单据”</br>
 
点击审批，则为简单审批界面；</br>
 ![](/articles/application/6-/images/04/image90.png)  

如果选择“打开单据”，则出现如下审批界面</br>
 ![](/articles/application/6-/images/04/image91.png) 
 
选择并处理即可。</br>
3.	已办任务（处理弃审动作）</br>
点击“已办”，能看到刚审批的单据。</br>
 ![](/articles/application/6-/images/04/image92.png) 

 点击打开单据，可以查看单据，同时弃审按钮可用，可以做回退操作。</br>
  ![](/articles/application/6-/images/04/image93.png) 

## 4	案例demo

培训案例中提供了支持审批流程主子表示例</br>
前台代码</br>
  ![](/articles/application/6-/images/04/image94.png) 

后台代码</br>
实体：</br>
com.yonyou.iuap.appdemo.entity.Busiflow 
com.yonyou.iuap.appdemo.entity.Busiflow_sub 
持久层Mapper
com.yonyou.iuap.appdemo.mapper.BusiflowMapper
com.yonyou.iuap.appdemo.mapper.Busiflow_subMapper
服务层Service
com.yonyou.iuap.appdemo.service.BusiflowService
com.yonyou.iuap.appdemo.service.Busiflow_subService
控制层Web
com.yonyou.iuap.appdemo.web.BusiflowController
com.yonyou.iuap.appdemo.web.Busiflow_subController  
com.yonyou.iuap.appdemo.web.Busiflow_BPMProcessController 流程回写服务

### 4.1	html</br>
添加bpmhead的div</br>
  ![](/articles/application/6-/images/04/image95.png)  

添加bpmbody的div</br>
  ![](/articles/application/6-/images/04/image96.png)  

### 4.2	js</br>
引入通用js</br>
  ![](/articles/application/6-/images/04/image97.png) 
 
#### 4.2.1	提交</br>

               /**
                 * BPM-start
                 */
                beforeSubmit:function(){
                	$.ajax({
                        type : 'get',
                        url : '/eiap-plus/appResAllocateRelate/getEnableBpm?funcCode=' + node.funcCode,
                        dataType : 'json',
                        async : true,
                        contentType : "application/json ; charset=utf-8",
                        success : function(result) {
                            var enableBpm = result.detailMsg.data;
                            if(enableBpm=='Y'){
                            	return;
                            }else{
                            	//不启动审批流程，业务组直接设置单据上的状态
                            	viewModel.busiflowDa.setValue("state",'1');
                            	//应该要提交吧?
                            	debugger;
                            }
                        }
                    })
                },

                //审批流添加功能----提交审批
                submit: function () {
                    var selectArray = viewModel.busiflowDa.selectedIndices();

                    if (selectArray.length < 1) {
                        u.messageDialog({
                            msg: "请选择要提交的记录!",
                            title: "提示",
                            btnText: "OK"
                        });
                        return;
                    }

                    if (selectArray.length > 1) {
                        u.messageDialog({
                            msg: "一次只能提交一条记录，请选择要提交的记录!",
                            title: "提示",
                            btnText: "OK"
                        });
                        return;
                    }

                    var selectedData = viewModel.busiflowDa.getSimpleData({type: 'select'});

                    if(selectedData[0].state &&	selectedData[0].state !='0'){ //状态不为待确认
                    	message("该单据已经使用关联流程，不能启动","error");
                    	return ;
                    }

                    $.ajax({
                        type: 'GET',
                        url: '/eiap-plus/appResAllocate/queryBpmTemplateAllocate?funccode=' + getAppCode() + '&nodekey=001',
                        datatype: 'json',
                        contentType: 'application/json;charset=utf-8',
                        success: function (result) {
                        	if(result){
                        		if(result.success=='success'){
                                    var data = result.detailMsg.data;
                                    if(data==null){
                                        u.messageDialog({ msg: "此数据在“资源分配”中未分配流程！", title: '操作有误', btnText: '确定' });
                                    }
                                    else{
                                        var processDefineCode = data.res_code;
                                        viewModel.event.submitBPMByProcessDefineCode(selectedData,processDefineCode);
                                    }
                        			
                        		}else{
                        			u.messageDialog({
										msg : data.detailMsg.msg,
										title : "提示",
										btnText : "OK"
									});
                        		}

                        	}else {
                                u.messageDialog({msg: '无返回数据', title: '操作提示', btnText: '确定'});
                            }
                        }
                	})
                },

                submitBPMByProcessDefineCode:function(selectedData,processDefineCode){
                	$.ajax({
                        type: "post",
                        url: viewModel.submiturl + "?processDefineCode=" + processDefineCode,
                        contentType: 'application/json;charset=utf-8',
                        data: JSON.stringify(selectedData),
                        success: function (res) {
                            if (res) {
                                if (res.success == 'success') {
                                	message("流程启动成功");
                                    viewModel.event.initUerList();
                                } else {
                                    u.messageDialog({msg: res.message, title: '操作提示', btnText: '确定'});
                                }
                            } else {
                                u.messageDialog({msg: '无返回数据', title: '操作提示', btnText: '确定'});
                            }
                        }

                    });
                },



#### 4.2.2	收回</br>
//审批流添加功能----取消提交
                unsubmit: function () {
                    var selectedData = viewModel.busiflowDa.getSimpleData({type: 'select'});
                    $.ajax({
                        type: "post",
                        url: viewModel.unsubmiturl,
                        contentType: 'application/json;charset=utf-8',
                        data: JSON.stringify(selectedData),
                        success: function (res) {
                            if (res) {
                                if (res.detailMsg.data.success == 'success') {
                                	message("流程收回成功");
                                	viewModel.event.initUerList();
                                } else {
                                    u.messageDialog({msg: res.detailMsg.data.message, title: '操作提示', btnText: '确定'});
                                }
                            } else {
                                u.messageDialog({msg: '无返回数据', title: '操作提示', btnText: '确定'});
                            }
                        }

                    });
                },

#### 4.2.3	查看单据</br>
/**
                 * 查看单据详情
                 */
                viewClick:function(viewModel,app){
                    viewModel.formStatus = _CONST.FORM_STATUS_EDIT;
                    var selectArray = viewModel.busiflowDa.selectedIndices();

                    if (selectArray.length < 1) {
                        u.messageDialog({
                            msg: "请选择要编辑的记录!",
                            title: "提示",
                            btnText: "OK"
                        });
                        return;
                    }

                    if (selectArray.length > 1) {
                        u.messageDialog({
                            msg: "一次只能编辑一条记录，请选择要编辑的记录!",
                            title: "提示",
                            btnText: "OK"
                        });
                        return;
                    }

                    //只显示返回和保存按钮
                    viewModel.event.userCardBtn();

                    $("#save").hide();

                    $(".busiflow_sub-actions").hide();   //子任务的增加和删除按钮隐藏

                    //获取选取行数据
                    viewModel.busiflowDa.setRowSelect(selectArray);
                    viewModel.busiflowFormDa.clear();
                    viewModel.busiflow_subFormDa.clear();
                    viewModel.busiflowFormDa.setSimpleData(viewModel.busiflowDa.getSimpleData({type: 'select'}));
                    viewModel.busiflow_subFormDa.setSimpleData(viewModel.busiflow_subDa.getSimpleData(), {unSelect: true});


                    //显示操作卡片
                    viewModel.md.dGo('addPage');

                    $('#addPage').each(function(index,element){
                    	$(element).find('input[type!="radio"]').attr('disabled',true);
                    });
                    
                  //加入bpm按钮
                    viewModel.initBPMFromBill(viewModel.busiflowDa.getValue("id"),viewModel);
                },

### 4.3	提交服务</br>
com.yonyou.iuap.appdemo.web.BusiflowController
	/** 提交审批 */
	@RequestMapping(value = "/submit", method = RequestMethod.POST)
	@ResponseBody
	public Object submit(@RequestBody List<Busiflow> objs, HttpServletRequest request) {
		logger.debug("execute submit operate.");
		try {
			String processDefineCode = request.getParameter("processDefineCode");
			service.batchSubmitEntity(objs, processDefineCode);
			return super.buildSuccess(objs);
		} catch (BusinessException e) {
			logger.error(e.getMessage(), e);
			return super.buildGlobalError(e.getMessage());
		}
	}

com.yonyou.iuap.appdemo.service.BusiflowService
	public void batchSubmitEntity(List<Busiflow> objs, String processDefineCode) {
		Busiflow entity = objs.get(0);
		BPMFormJSON bpmform = buildBPMFormJSON(processDefineCode, entity);

		JSONObject resultJsonObject = bpmSubmitBasicService.submit(bpmform);

		if (isSuccess(resultJsonObject)) {
			entity.setBillstate(DemoEnmu.BILLSTATUS_PROCESS);// 从未提交状态改为已提交状态;
			
			//修改DB表数据
			saveEntity(entity);
		} else if (isFail(resultJsonObject)) {
			String msg = resultJsonObject.get("msg").toString();
			throw new BusinessException("提交启动流程实例发生错误，请联系管理员！错误原因：" + msg);
		}
	}

	private boolean isSuccess(JSONObject resultJsonObject){
		return resultJsonObject.get("flag").equals("success");
	}
	private boolean isFail(JSONObject resultJsonObject){
		return resultJsonObject.get("flag").equals("fail");
	}
	
	/**
	 * 设置BPMFormJSON，为条件参数传值
	 * 
	 * @param processDefineCode
	 * @param entity
	 * @return
	 * @throws  
	 */
	public BPMFormJSON buildBPMFormJSON(String processDefineCode, Busiflow entity){
		try
		{
			BPMFormJSON bpmform = new BPMFormJSON();
			bpmform.setProcessDefinitionKey(processDefineCode);
			
			String userName = InvocationInfoProxy.getUsername();
			
			userName = java.net.URLDecoder.decode(userName,"utf-8");
			
			userName = java.net.URLDecoder.decode(userName,"utf-8");
			
			String title = userName + "提交的【单据】,单号 是" + entity.getCode() + ", 请审批";
			
			bpmform.setTitle(title);
			// 单据id
			bpmform.setFormId(entity.getId());
			// 单据号
			bpmform.setBillNo(entity.getCode());
			// 制单人
			bpmform.setBillMarker(InvocationInfoProxy.getUserid());
			// 组织
			String orgId = "";// usercxt.getSysUser().getOrgId() ;
			bpmform.setOrgId(orgId);
			
			
			// 其他变量
			bpmform.setOtherVariables(assemblingOtherVariables(entity));
			// 单据url
			bpmform.setFormUrl("/appdemo/pages/busiflow/busiflow.js");
			// 流程实例名称
			bpmform.setProcessInstanceName(title);
			// 流程审批后，执行的业务处理类(controller对应URI前缀)
			String url = "/appdemo/busiflow";
			bpmform.setServiceClass(url);
			return bpmform;
		}
		catch(Exception ex)
		{
			logger.error(ex.getMessage(), ex);
			
			throw new RuntimeException(ex.getMessage());
		}
	}

	/**
	 * 拼装其他变量
	 * 
	 * @param fields
	 * @return
	 */
	public List<RestVariable> assemblingOtherVariables(Busiflow entity) {
		List<RestVariable> otherVariables = new ArrayList<RestVariable>();
		Field[] f2=entity.getClass().getDeclaredFields();
		for (Field field : f2) {
			RestVariable value = new RestVariable();
			field.setAccessible(true);
			try {
				value.setName(field.getName());
				value.setValue(field.get(entity));
				System.out.println(field.getName()+field.get(entity));
				otherVariables.add(value);
			} catch (IllegalArgumentException e) {
				logger.error(e.getMessage());
			} catch (IllegalAccessException e) {
				logger.error(e.getMessage());
			}
		}
		return otherVariables;
	}


### 4.4	收回服务</br>
com.yonyou.iuap.appdemo.web.BusiflowController
	/** 收回 */
	@RequestMapping(value = "/unsubmit", method = RequestMethod.POST)
	@ResponseBody
	public Object unsubmit(@RequestBody List<Busiflow> objs,
			HttpServletRequest request) {
		logger.debug("execute unsubmit operate.");
		Object unsubmitJson = service.batchUnsubmitEntity(objs);
		return super.buildSuccess(unsubmitJson);
	}


com.yonyou.iuap.appdemo.service.BusiflowService
	public JSONObject batchUnsubmitEntity(List<Busiflow> objs) {
		Busiflow entity = objs.get(0);
		entity.setBillstate(DemoEnmu.BILLSTATUS_INIT);// 从已提交状态改为未提交状态;
	
		//修改DB表数据
		saveEntity(entity);
		
		return bpmSubmitBasicService.unsubmit(entity.getId());
	}

### 4.5	流程回写</br>
package com.yonyou.iuap.appdemo.web;

import java.util.Date;
import java.util.Map;
import javax.servlet.http.HttpServletRequest;
import net.sf.json.JSONObject;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import yonyou.bpm.rest.response.historic.HistoricProcessInstanceResponse;
import com.yonyou.iuap.appdemo.entity.DemoEnmu;
import com.yonyou.iuap.appdemo.service.BusiflowService;
import com.yonyou.iuap.bpm.service.JsonResultService;
import com.yonyou.iuap.bpm.web.IBPMBusinessProcessController;
import com.yonyou.iuap.mvc.type.JsonResponse;

@Controller
@RequestMapping({ "/busiflow" })
public class Busiflow_BPMProcessController implements
		IBPMBusinessProcessController {
	@Autowired
	private JsonResultService jsonResultService;
	@Autowired
	private BusiflowService service;

	@Override
	public Object doApproveAction(@RequestBody Map<String, Object> paramMap,
			HttpServletRequest paramHttpServletRequest) throws Exception {
		String json = this.jsonResultService.toJson(paramMap);
		JSONObject requestBody = (JSONObject) this.jsonResultService.toObject(
				json, JSONObject.class);
		Object Node = requestBody.get("historicProcessInstanceNode");
		HistoricProcessInstanceResponse historicProcessInstance = (HistoricProcessInstanceResponse) this.jsonResultService
				.toObject(Node.toString(),
						HistoricProcessInstanceResponse.class);
		Date endTime = historicProcessInstance.getEndTime();
		if (endTime != null) {
			String billId = historicProcessInstance.getBusinessKey();
			service.updateBillStatus(billId, DemoEnmu.BILLSTATUS_FINISHED);
		}

		JsonResponse response = new JsonResponse();
		return response;
	}

	@Override
	public JsonResponse doTerminationAction(
			@RequestBody Map<String, Object> paramMap) throws Exception {
		String json = this.jsonResultService.toJson(paramMap);
		JSONObject requestBody = (JSONObject) this.jsonResultService.toObject(
				json, JSONObject.class);
		Object Node = requestBody.get("historicProcessInstanceNode");
		HistoricProcessInstanceResponse historicProcessInstance = (HistoricProcessInstanceResponse) this.jsonResultService
				.toObject(Node.toString(),
						HistoricProcessInstanceResponse.class);
		Date endTime = historicProcessInstance.getEndTime();
		if (endTime != null) {
			String billId = historicProcessInstance.getBusinessKey();
			service.updateBillStatus(billId, DemoEnmu.BILLSTATUS_STOP);
		}

		JsonResponse response = new JsonResponse();
		return response;
	}

	@Override
	public JsonResponse doRejectMarkerBillAction(
			@RequestBody Map<String, Object> paramMap) throws Exception {
		String billId = paramMap.get("billId").toString();
		service.updateBillStatus(billId, DemoEnmu.BILLSTATUS_PROCESS);
		JsonResponse response = new JsonResponse();
		return response;
	}

}

com.yonyou.iuap.appdemo.service.BusiflowService
	public void updateBillStatus(String billid,int billstatus){
		Timestamp ts = new Timestamp((new Date()).getTime());
		BusiflowMapper.updatebillstatusbyPk(billid,billstatus,ts);
	}

BusiflowMapper.xml
	<update id="updatebillstatusbyPk">
		update busiflow
		set  billstate = #{status,jdbcType=NUMERIC}, ts = #{ts,jdbcType=TIMESTAMP} 
		where id = #{0}
	</update>

