# 微服务监控

## 简介

    微服务监控主要针对微服务启动失败、微服务配置出错、rpc调用异常等问题的排查，同时提供了准实时rpc调用qps、次数、失败次数等信息
    

## 配置项
<table border="1px" align="left" bordercolor="black" width="80%" height="100px">
<tr align="left"><td>配置项（前缀app.metainfo.monitor.）</td><td>默认值</td><td>说明</td></tr>
<tr align="left">  <td>reportConnectTimeout</td><td>300</td>  <td>okhttp connectTimeout</td>   </tr>
<tr align="left">  <td>reportReadTimeout</td><td>300</td>  <td>okhttp readTimeout</td>   </tr>
<tr align="left">  <td>maxBufferSize</td><td>1000</td>  <td>上报队列最大缓存数量</td>   </tr>
<tr align="left">  <td>maxReportSize</td><td>10</td>  <td>一次最大上报数量</td>   </tr>
<tr align="left">  <td>minReportDelay</td><td>60000</td>  <td>两次上报时间最大间隔</td>   </tr>
<tr align="left">  <td>maxExpire</td><td>120000</td>  <td>队列中存在的最大时间</td>   </tr>
<tr align="left">  <td>rateTimeInterval</td><td>10000</td>  <td>收集数据的时间间隔</td>   </tr>
<tr align="left">  <td>logPath.</td><td>/monitor</td>  <td>本地持久化位置</td>   </tr>
<tr align="left">  <td>logSize</td><td>10485760</td>  <td>每个文件的大小</td>   </tr>
<tr align="left">  <td>persistenceMonitor</td><td>true</td>  <td>是否开启监控</td>   </tr>
<tr align="left">  <td>reportUrl</td><td></td>  <td>上报url</td>   </tr>
<tr align="left">  <td>threadExpectTime</td><td>5000</td>  <td>rpc调用耗时超过这个值会上报</td>   </tr>
</table>