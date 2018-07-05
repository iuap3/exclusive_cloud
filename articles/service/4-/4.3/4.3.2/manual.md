# 文档生成

**对外发布的API接口应写上文档描述, 以便接口使用者更加容易理解和使用.**

以下是各个注解的使用说明:

- 作用于API的作用描述, 标注在API接口方法的文档注释.
	- 注解: @com.wordnik.swagger.annotations.ApiOperation(value)
	- 注解属性详解:
		- value: API描述字符串.
- 作用于API接口参数的描述, 标注在入参的文档注释.
	- 注解: @com.yonyou.cloud.mwclient.servmeta.annotation.ApiParam
	- 注解属性详解:
		- name: 参数名称
		- description: 参数描述
		- required: 是否必填
		- exampleValue: 示例值
		- moreRestrictions: 限制说明
- 作用于API接口返回值的描述, 标注在返回值的文档注释.
	- 注解: @com.yonyou.cloud.mwclient.servmeta.annotation.ApiReturnValue
	- 注解属性详解:
		- name: 返回值名称
		- exampleValue: 返回值示例值
		- description: 返回值描述.

