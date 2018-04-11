此组件是iUAP前端框架的后端适配组件，和spring MVC配合，接收并处理前端UI datatable发送的数据，拥有独立的上下文处理和序列化框架，对事件、多语、异常、参数传递等做了封装，方便了开发者对前后端数据的对接。

Datatable控制器是一个特殊的spring MVC 控制器，与普通的Spring MVC控制器的区别有两点：

1.	可以通过参数来获取Datatable 相关的信息，如Event、EventRequest，EventResponse，Datatable 。

2.	输出类型必须是JSON，返回对象必须是EventResponse.
