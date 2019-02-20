##  开发示例

其他模块借助自定义档案进行开发的时候，在引入自定义档案iuaprmodel.war的情况下，只需要如下配置即可使用。

$.ajax({

        type: GET,

        url: &quot; iuaprmodel/modeling/data/query &quot;,

        data: { tableName: &quot;mdm\_x&quot;, mdm\_code: &quot;XL0000000007&quot;},

        dataType: &quot;json&quot;,

        success: function(data){

        },
		error:function(){
        }
});

通过ajax请求，即可获取到指定的数据。其中请求参数说明，请参考API模块对应的接口规范，自定义档案数据模型的建立、数据的维护等，参考应用支撑组件模块对应的章节。
