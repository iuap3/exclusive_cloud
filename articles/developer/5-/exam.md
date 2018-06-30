## 概述
用友云开发者中心的```持续集成流水线```和```生产环境资源池添加主机```需要加入审批流程，在执行到需要审批的环节，会向指定的审批人发送邮件，审批人收到邮件执行审批，如果审批通过，则继续往下执行。

## 流水线审批执行点
执行流水线之前，在填写流水线配置信息的时候，部分流水线点可以选择执行。如图1所示。
<div align="center">
<img src="/articles/cloud/3-/images/exam/exam-select.jpg">
</div>
<p align="center"> 图 1</p>

## 流水线审批人指定
执行流水线之前，在填写流水线配置信息的时候，需要指定审批人，在执行流水线的时候，会自动将审批邮件发送给添加的审批人（审批人可以添加多个）。如图2，3，4所示，添加审批人步骤。
<div align="center">
<img src="/articles/cloud/3-/images/exam/examrole.jpg">
</div>
<p align="center"> 图 2</p>

<div align="center">
<img src="/articles/cloud/3-/images/exam/examaddrole.jpg">
</div>
<p align="center"> 图 3</p>

<div align="center">
<img src="/articles/cloud/3-/images/exam/examaddsch.jpg">
</div>
<p align="center"> 图 4</p>


## 流水线审批执行
如果选择了审批点，则在配置完成之后，执行流水线期间，进行到审批点的时候，流水线会停止执行。如图5所示。
<div align="center">
<img src="/articles/cloud/3-/images/exam/exam-stop.jpg">
</div>
<p align="center"> 图 5</p>

## 流水线审批邮件的接收
执行流水线之前，在填写流水线配置信息的时候，如果选择了流水线审批执行点之后，需要在下面指定是```审批```或者```告知```，如果是审批，会执行审批流程，如果是告知，则不会执行审批流程，流水线将直接执行到底，但是中间会向审批人发送告知邮件，审批告知邮件格式，如图6、7所示。
<div align="center">
<img src="/articles/cloud/3-/images/exam/exam-email.jpg">
</div>
<p align="center"> 图 6</p>

<div align="center">
<img src="/articles/cloud/3-/images/exam/exam-email2.jpg">
</div>
<p align="center"> 图 7</p>

## 审批人收到邮件执行审批
如图6所示，当审批人收到邮件之后，需要点击```请点击这里进入```进入审批界面，执行审批。如图8所示。
<div align="center">
<img src="/articles/cloud/3-/images/exam/exam-ui.jpg">
</div>
<p align="center"> 图 8</p>

之后，点击如图8所示的```审批```按钮，进入审批框进行审批。如图9所示。
<div align="center">
<img src="/articles/cloud/3-/images/exam/exam-ui2.jpg">
</div>
<p align="center"> 图 9</p>

如果申请通过，会显示绿色图标```申请通过```，不通过会显示橙色图标```申请不通过```如图10所示。
<div align="center">
<img src="/articles/cloud/3-/images/exam/exam-ui3.jpg">
</div>
<p align="center"> 图 10</p>

## 审批结束之后，流水线的执行状态
当审批结束之后，如果审批通过，流水线会继续往下执行，直到执行结束，如果不通过，则流水线审批点会显示```×```如图11所示。
<div align="center">
<img src="/articles/cloud/3-/images/exam/exam-X.jpg">
</div>
<p align="center"> 图 11</p>


##自有资源审批执行入口
自有资源审批是在资源池添加主机的时候要进行一次添加主机的审批拦截，前提是只在生产环境进行拦截，点击添加主机执行拦截。拦截的时候，将要向指定的审批人发送邮件。如图12所示。
<div align="center">
<img src="/articles/cloud/3-/images/exam/rexam-sch.jpg">
</div>
<p align="center"> 图 12</p>

##自有资源审批审批人
自有资源审批的审批人是当前用户的租户，还有一些自定义的审批人，自定已审批人可以在后台数据库中持续更新。

##自有资源审批邮件模板
当审批人收到邮件之后，会看到如图13所示的信息。
<div align="center">
<img src="/articles/cloud/3-/images/exam/rexam-email.jpg">
</div>
<p align="center"> 图 13</p>

点击邮件中相应的超链接```进入```，会进入到审批页面进行审批。如图14所示。
<div align="center">
<img src="/articles/cloud/3-/images/exam/rexam-ui.jpg">
</div>
<p align="center"> 图 14</p>

审批页面ui以及审批流程基本和流水线审批完全相同，此处不再赘述，当审批通过之后，即可进行相应的主机添加。

  