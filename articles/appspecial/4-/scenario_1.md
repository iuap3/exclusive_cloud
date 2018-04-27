# 币种_单表单据向导开发

## 业务场景

iuap平台是面向企业互联网应用的企业互联网运营平台。本指南以一个后台管理中的-列表类型页面为例，演示如何开发标准节点的操作步骤。</br>
1、需要安装iuap-STUDIO开发工具（可以参考iuap后台环境搭建、iuap前端环境搭建视频）</br>
2、创建数据库表，向导生成节点</br>
3、功能开发</br>

![](https://i.imgur.com/lbkY11j.jpg) 
 
## 一、step by step做节点开发

### 1、环境准备</br>

#### 1.1、开发环境准备</br>

新建项目以及创建数据库等过程，请参考《环境搭建》文档。默认已创建train项目。</br>

#### 1.2、Mybatis持久化配置</br>

找到持久化配置文件</br>
![](https://i.imgur.com/H56uATj.jpg)

 因为使用了Mybatis的持久化方式，所以需要修改对应的配置信息，增加Mybatis扫描的entity包。同时因为使用了mysql数据库，所以下面配置文件为mysql文件夹下的xml文件
 ![](https://i.imgur.com/qbMxW9E.jpg)
mapper接口类位置，增加我们目前生成的mapper包位置
 ![](https://i.imgur.com/poVaVtD.jpg)

### 2、建表</br>

CREATE TABLE `train_currtype` (</br>
  `creator` varchar(36) DEFAULT NULL,</br>
  `name` varchar(50) NOT NULL,</br>
  `currdigit` varchar(50) NOT NULL,</br>
  `code` varchar(50) NOT NULL,</br>
  `creationtime` datetime DEFAULT NULL,</br>
  `pk_currtype` char(36) NOT NULL,</br>
  `ts` datetime DEFAULT NULL,</br>
  `dr` smallint(6) DEFAULT '0',</br>
  PRIMARY KEY (`pk_currtype`)</br>
)

### 3、向导生成节点</br>

此处文档介绍的是使用iuap向导生成mybatis持久化的代码，也可以选择mybatis第三方插件的方式生成。</br>
1、项目点击右键“iuap tools->快速代码生成”
![](https://i.imgur.com/bxuYXEX.jpg)
 选择，从数据库导入，点击“下一步”</br>
![](https://i.imgur.com/BeTMkhS.jpg)
 左侧选择数据库表train_currtype，右侧勾选需要在节点上使用的字段，并点击下一步。</br>
![](https://i.imgur.com/xyYwawg.jpg)

![](https://i.imgur.com/ejFmw53.jpg)
在此处填写前后端代码包名
实体类包名、Mapper接口包名、Service包名、Controller包名
前端页面html,css,js位置
此处改写中文显示名称，同时ts字段前台不显示，日期显示等，查询字段为编码名称</br>
![](https://i.imgur.com/CcQNba5.jpg) 
点击完成，生成后的代码如下图所示</br>
后台代码</br>
![](https://i.imgur.com/sE6ACMd.jpg) 

前台代码</br>
![](https://i.imgur.com/YEfnclS.jpg)

mybatis xml文件</br>
与接口类同名的xml文件train_currtype.xml</br>
 ![](https://i.imgur.com/8guolWL.jpg)

## 二、代码解析

![](https://i.imgur.com/UbEJy5T.jpg)
 
### 1、代码结构</br>

![](https://i.imgur.com/X9vHNeU.jpg) 

Train_currtype.java:  实体</br>
Train_currtypeMapper.java 持久层dao</br>
Train_currtypeService.java  服务层Service</br>
Train_currtypeController.java  web层Controller</br>
 ![](https://i.imgur.com/0GxN1Pn.jpg)

train_currtype.xml映射文件</br>

### 2、Mybatis介绍</br>

MyBatis是支持普通SQL查询，存储过程和高级映射的优秀持久层框架。
MyBatis消除了几乎所有的JDBC代码和参数的手工设置以及结果集的检索。MyBatis使用简单的XML或注解用于配置和原始映射，将接口和Java的POJOs（Plan Old Java Objects，普通的Java对象）映射成数据库中的记录.

Mybatis运行过程
![](https://i.imgur.com/BMYQfhZ.jpg) 

### 3、后台代码解析</br>

后端代码分为几层：实体、DAO、Service、Controller，下面的中描述了手工编写后台代码时的开发步骤，此处因为向导开发，已经生成完毕。

![](https://i.imgur.com/Gn54zv6.jpg)
 

#### 3.1、配置

iuap平台使用iuap-mybatis作为MyBatis持久化的支持并提供了统一的Spring扫描注解和分页插件来支持分页的实现。
iuap-persistence同时依赖了基础的Spring Data JPA和Mybatis，如果项目上明确只使用Mybatis，则不需要引入iuap-persistence组件，直接依赖iuap-mybatis组件即可，注意要和iuap其他组件的版本保持一致。


- 功能特性</br>
iuap-mybatis提供了统一的Spring扫描注解和分页插件。通过@MyBatisRepository确定mybatis提供服务的接口，再使用mybatis的xml文件管理实现mybatis服务的sql语句。
分页插件支持MyBatis3.2.0~3.3.0(包含)

- 配置</br>
在maven中配置
 

#### 3.2、实体</br>

Train_currtype.java单表实体
package com.yonyou.iuap.appdemo.entity;

import java.util.Date;

/**
 * mybatis方式
 */
public class Train_currtype {
	
	private String creator;
	private String modifier;
	private String name;
	private String currdigit;
	private String code;
	private Date modifiedtime;
	private Date creationtime;
	private String pk_currtype;
	private Date ts;
	public Train_currtype() {
	}
    
	public String getCreator() {
		return this.creator;
	}

	public void setCreator(String creator) {
		this.creator = creator;
	}

	public String getModifier() {
		return this.modifier;
	}

	public void setModifier(String modifier) {
		this.modifier = modifier;
	}

	public String getName() {
		return this.name;
	}

	public void setName(String name) {
		this.name = name;
	}


	public String getCurrdigit() {
		return this.currdigit;
	}

	public void setCurrdigit(String currdigit) {
		this.currdigit = currdigit;
	}


	public String getCode() {
		return this.code;
	}

	public void setCode(String code) {
		this.code = code;
	}


	public Date getModifiedtime() {
		return this.modifiedtime;
	}

	public void setModifiedtime(Date modifiedtime) {
		this.modifiedtime = modifiedtime;
	}


	public Date getCreationtime() {
		return this.creationtime;
	}

	public void setCreationtime(Date creationtime) {
		this.creationtime = creationtime;
	}


	public String getPk_currtype() {
		return this.pk_currtype;
	}

	public void setPk_currtype(String pk_currtype) {
		this.pk_currtype = pk_currtype;
	}


	public Date getTs() {
		return this.ts;
	}

	public void setTs(Date ts) {
		this.ts = ts;
	}

}
@JsonIgnoreProperties</br>
此注解是类注解，作用是json序列化时将java bean中的一些属性忽略掉，序列化和反序列化都受影响。</br>
#### 3.3、后台代码功能及常见注解</br>

Controller
Controller层代码Train_CurrtypeController.java类</br>
> package com.yonyou.iuap.appdemo.web;
> 
> import com.yonyou.iuap.appdemo.entity.Train_currtype;
> import com.yonyou.iuap.appdemo.service.Train_currtypeService;
> import com.yonyou.iuap.mvc.type.SearchParams;
> import com.yonyou.iuap.example.web.BaseController;
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.data.domain.Page;
> import org.springframework.data.domain.PageRequest;
> import org.springframework.web.bind.annotation.*;
> import java.util.List;
> 
/**
 * <p>
 * Title: CardTableController
 * </p>
 * <p>
 * Description: 卡片表示例的controller层
 * </p>
 */
@RestController
@RequestMapping(value = "/train_currtype")
public class Train_currtypeController extends BaseController {
	@Autowired
	private Train_currtypeService service;

    /**
     * 查询分页数据
     * 
     * @param pageRequest
     * @param searchParams
     * @return
     */
    @RequestMapping(value = "/list", method = RequestMethod.GET)
    public @ResponseBody Object page(PageRequest pageRequest, SearchParams searchParams) {
        Page<Train_currtype> data = service.selectAllByPage(pageRequest, searchParams);
        return buildSuccess(data);
    }

    /**
     * 保存数据
     * 
     * @param list
     * @return
     */
    @RequestMapping(value = "/save", method = RequestMethod.POST)
    public @ResponseBody Object save(@RequestBody List<Train_currtype> list) {
    	service.save(list);
        return buildSuccess();
    }

    /**
     * 删除数据
     * 
     * @param list
     * @return
     */
    @RequestMapping(value = "/del", method = RequestMethod.POST)
    public @ResponseBody Object del(@RequestBody List<Train_currtype> list) {
    	service.batchDeleteByPrimaryKey(list);
        return buildSuccess();
    }
}

Train_CurrtypeController继承了iuap提供的Controller基类：</br>
com.yonyou.iuap.example.web.BaseController【提供了各种错误、成功提示信息包装】</br>


- Controller类上必须使用@RestController或者@Controller注解、@RequestMapping注解用户地址映射

- 	@RestController：继承自@Controller注解，Spring MVC的组件都使用@Controller来标识当前类是一个控制器servlet。使用这个特性，我们可以开发REST服务的时候不需要使用@Controller而专门的@RestController。
@RequestMapping：是一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径1、 value， method；value： 指定请求的实际地址，指定的地址可以是URI Template 模式；method：  指定请求的method类型， GET、POST、PUT、DELETE等；
在web.xml中，对所有的路径进行 过滤，或者拦截，不拦截的包括 *.mp3  /  *.js    /js/  ，等，这些都属于请求静态资源，并不需要访问controller。
针对请求后台的就会进行拦截。然后找到匹配的controller 中的方法进行处理


- @Autowired：用在JavaBean中的注解，通过byType形式，用来给指定的字段或方法注入所需的外部资源。

- @ResponseBody:用于将Controller的方法返回的对象，通过适当的
HttpMessageConverter转换为指定格式后，写入到Response对象的body数据区。【此处可以将我们的实体转为前端的Jason对象，用于前端数据显示，或者从前端的jason对象转为我们的实体】；
- 获取前台传值到后台的方法，可以用httpservletrequest ，更好的方法，是放到一个参数类中。可以直接用 参数类来进行钱后台数据交互，会根据你类的 字段名，进行自动匹配。
Service</br>
Service层代码Train_currtypeService.java</br>
package com.yonyou.iuap.appdemo.service;

import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

import org.apache.commons.collections.CollectionUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.stereotype.Component;

import com.yonyou.iuap.appdemo.entity.Train_currtype;
import com.yonyou.iuap.appdemo.mapper.Train_currtypeMapper;
import com.yonyou.iuap.mvc.type.SearchParams;

@Component
public class Train_currtypeService {
	@Autowired
	Train_currtypeMapper mapper;
	

    /**
     * Description:分页查询
     * Page<CardTable>
     * @param str
     */
    public Page<Train_currtype> selectAllByPage(PageRequest pageRequest, SearchParams searchParams) {
        return mapper.selectAllByPage(pageRequest, searchParams).getPage();
    }
    
    
    /**
     * Description:批量保存（包括新增和更新）
     * void
     * @param str
     */
    public void save(List<Train_currtype> recordList) {
        List<Train_currtype> addList = new ArrayList<>(recordList.size());
        List<Train_currtype> updateList = new ArrayList<>(recordList.size());
        for (Train_currtype cardTable : recordList) {

            if (cardTable.getPk_currtype() == null) {
            	cardTable.setPk_currtype(UUID.randomUUID().toString());
//            	cardTable.setDr(new Integer(0));
                addList.add(cardTable);
            } else {
                updateList.add(cardTable);
            }
        }
        if (CollectionUtils.isNotEmpty(addList)) {
        	mapper.batchInsert(addList);
        }
        if (CollectionUtils.isNotEmpty(updateList)) {
        	mapper.batchUpdate(updateList);
        }
    }
    
    /**
     * Description:批量删除
     * void
     * @param str
     */
    public void batchDeleteByPrimaryKey(List<Train_currtype> list) {
    	mapper.batchDeleteByPrimaryKey(list);
    }
    
    /**
     * Description:通过非主键字段查询
     * List<CardTable>
     * @param str
     */
    public List<Train_currtype> findByName(String code) {
        return mapper.findByName(code);
    }
    public List<Train_currtype> findByCode(String code) {
        return mapper.findByCode(code);
    }
}

Service类上使用@Component注解；</br>


- @Component：用于标注业务层组件，可以被Spring容器扫描
**Dao/Mapper**
注解使用 @MyBatisRepository</br> 
定义了持久层一系列接口</br>
Dao层代码Train_currtypeDao.java</br>
package com.yonyou.iuap.appdemo.mapper;

import com.yonyou.iuap.appdemo.entity.Train_currtype;
import com.yonyou.iuap.mvc.type.SearchParams;
import com.yonyou.iuap.mybatis.type.PageResult;
import com.yonyou.iuap.persistence.mybatis.anotation.MyBatisRepository;

import org.apache.ibatis.annotations.Param;
import org.springframework.data.domain.PageRequest;

import java.util.List;


@MyBatisRepository
public interface Train_currtypeMapper {
	
	//单个增删改查
	int insert(Train_currtype record);

	int insertSelective(Train_currtype record);
	
	int updateByPrimaryKeySelective(Train_currtype record);

    int updateByPrimaryKey(Train_currtype record);

    int deleteByPrimaryKey(String pk);

    Train_currtype selectByPrimaryKey(String pk);
    
	//根据某一非主键字段查询实体
	List<Train_currtype> findByName(String name);
	//根据某一非主键字段查询实体
	List<Train_currtype> findByCode(String code);
    
	PageResult<Train_currtype> selectAllByPage(@Param("page") PageRequest pageRequest, @Param("search") SearchParams searchParams);
    
   //批量操作
    void batchInsert(List<Train_currtype> addList);

    void batchUpdate(List<Train_currtype> updateList);

    void batchDeleteByPrimaryKey(List<Train_currtype> list);
    
}

**映射文件xml **
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.yonyou.iuap.appdemo.mapper.Train_currtypeMapper">
	<resultMap id="BaseResultMap" type="com.yonyou.iuap.appdemo.entity.Train_currtype">
		<id column="pk_currtype" property="pk_currtype"/>
		<result column="name" property="name"/>
		<result column="currdigit" property="currdigit"/>
		<result column="code" property="code"/>
		<result column="ts" property="ts"/>
	</resultMap>
	<sql id="Base_Column_List">
		name,currdigit,code,pk_currtype,ts
	</sql>
	<select id="selectByPrimaryKey" resultMap="BaseResultMap"
		parameterType="java.lang.String">
		select
		<include refid="Base_Column_List" />
		from train_currtype
		where pk_currtype = #{pk_currtype}
	</select>

	<select id="selectAllByPage" resultMap="BaseResultMap"
		resultType="java.util.List">
		SELECT
		<include refid="Base_Column_List" />
		from train_currtype where 1=1
		<if test="search != null">
			<if
				test="search.searchMap.searchParam!=null and search.searchMap.searchParam!='' ">
				and 
			</if>
		</if>
		<if test="page != null">
			<if test="page.sort!=null">
				order by
				<foreach collection="page.sort" item="item" separator=" ">
					${item.property} ${item.direction}
				</foreach>
			</if>
		</if>
	</select>

	<delete id="deleteByPrimaryKey" parameterType="java.lang.String">
		delete from train_currtype
		where pk_currtype = #{pk_currtype}
	</delete>

	<delete id="batchDeleteByPrimaryKey" parameterType="java.util.List">
		delete from train_currtype
		where
		pk_currtype in
		<foreach collection="list" item="item" index="index"
			separator="," open="(" close=")">
		#{item.pk_currtype}
		</foreach>
	</delete>

	<insert id="insert" parameterType="com.yonyou.iuap.appdemo.entity.Train_currtype">
		insert into train_currtype (
		name,currdigit,code,pk_currtype,ts
		)
		values (
		#{name},
		#{currdigit},
		#{code},
		#{pk_currtype},
		#{ts}
		)
	</insert>

	<insert id="batchInsert" parameterType="java.util.List">
		insert into train_currtype (
		name,currdigit,code,pk_currtype,ts
		)
		values
		<foreach collection="list" item="item" index="index"
			separator=",">
			(
		#{item.name},
		#{item.currdigit},
		#{item.code},
		#{item.pk_currtype},
		#{item.ts}
			)
		</foreach>
	</insert>

	<insert id="insertSelective" parameterType="com.yonyou.iuap.appdemo.entity.Train_currtype">
		insert into train_currtype
		<trim prefix="(" suffix=")" suffixOverrides=",">
			<if test="name != null">
				name,
			</if>
			<if test="currdigit != null">
				currdigit,
			</if>
			<if test="code != null">
				code,
			</if>
			<if test="pk_currtype != null">
				pk_currtype,
			</if>
			<if test="ts != null">
				ts,
			</if>
		</trim>
		<trim prefix="values (" suffix=")" suffixOverrides=",">
			<if test="name != null">
				#{name},
			</if>
			<if test="currdigit != null">
				#{currdigit},
			</if>
			<if test="code != null">
				#{code},
			</if>
			<if test="pk_currtype != null">
				#{pk_currtype},
			</if>
			<if test="ts != null">
				#{ts},
			</if>
		</trim>
	</insert>
	<update id="updateByPrimaryKeySelective" parameterType="com.yonyou.iuap.appdemo.entity.Train_currtype">
		update train_currtype
		<set>
			<if test="name != null">
				name = #{name},
			</if>
			<if test="currdigit != null">
				currdigit = #{currdigit},
			</if>
			<if test="code != null">
				code = #{code},
			</if>
			<if test="pk_currtype != null">
				pk_currtype = #{pk_currtype},
			</if>
			<if test="ts != null">
				ts = #{ts},
			</if>
		</set>
		where pk_currtype = #{pk_currtype} 
		<!--and ts = #ts-->
	</update>
	<update id="updateByPrimaryKey" parameterType="com.yonyou.iuap.appdemo.entity.Train_currtype">
		update train_currtype
		set
				name = #{name},
				currdigit = #{currdigit},
				code = #{code},
				ts = #{ts}
		where pk_currtype = #{pk_currtype}
		<!-- and ts = #ts-->
	</update>

	<update id="batchUpdate" parameterType="java.util.List">
		<foreach collection="list" item="item" index="index" open=""
			close="" separator=";">
			update train_currtype
			set
				name = #{item.name},
				currdigit = #{item.currdigit},
				code = #{item.code},
				ts = #{item.ts}
			where pk_currtype = #{item.pk_currtype} 
			<!--and ts = #{item.ts}-->
		</foreach>
	</update>
</mapper>

**SQL语句标签**</br>
如下图新增的示例：</br>
![](https://i.imgur.com/omcDveq.jpg) 



- id="xxxx" >>> 表示此段sql执行语句的唯一标识，也是接口的方法名称【必须一致才能找到】


- -parameterType="" >>>表示该sql语句中需要传入的参数类型， 类型要与对应的接口方法的类型一致【可选】


- -取值方式：#{value jdbcType = valuetype}： jdbcType 可选，表示该属性的数据类型在数据库中对应的类型；

![](https://i.imgur.com/JgcuG08.jpg) 



- resultMap=“ ”>>> 定义出参，调用已定义的<resultMap>映射管理器的id值


- resultType=“ ”>>>定义出参，匹配普通java类型或自定义的pojo【出参类型若不指定，将为语句类型默认类型，如<insert>语句返回值为int】


- 引入SQL片段。通过<include refid="" />标签引用，refid="" 中的值指向需要引用的<sql>中的id=“”属性
上图中引入了Base_Column_List这个SQL片段，下面章节中介绍SQL片段定义的方式

**SQL片段标签<sql>**

 ![](https://i.imgur.com/3VK6wzG.jpg)

通过该标签可定义能复用的sql语句片段，在执行sql语句标签中直接引用即可。这样既可以提高编码效率，还能有效简化代码，提高可读性。需要配置的属性：

- id="" >>>表示需要改sql语句片段的唯一标识


- 引用：通过<include refid="" />标签引用，refid="" 中的值指向需要引用的<sql>中的id=“”属性

**映射管理器**
![](https://i.imgur.com/caCZnaC.jpg)
 
映射管理器resultMap：映射管理器，是Mybatis中最强大的工具，使用其可以进行实体类之间的关系，并管理结果和实体类间的映射关系。
需要配置的属性：<resultMap id="  " type="  "></resutlMap>   

- id=" ">>>表示这个映射管理器的唯一标识，外部通过该值引用； 

- type = " ">>> 表示需要映射的实体类；
如上图中实体类为com.yonyou.iuap.appdemo.entity.Train_currtype
 主键配置： <id column = " " property= " " />   

- <id>标签指的是：结果集中结果唯一的列【column】 和 实体属性【property】的映射关系，注意：<id>标签管理的列未必是主键列，需要根据具体需求指定；
如上图中主键字段：

![](https://i.imgur.com/Lvn8djN.jpg)
    
普通列值配置： <result column= " " property=" " />  

- <result>标签指的是：结果集中普通列【column】 和 实体属性【property】的映射关系；
如name名称字段

 ![](https://i.imgur.com/4TSkOPF.jpg)

**动态SQL**

- if
- foreach
 
![](https://i.imgur.com/RnMFAFR.jpg)

动态 SQL 的一个常用的操作需求是对一个集合进行遍历，通常是在构建 IN 条件语句的时候。</br>
foreach 元素的功能非常强大，它允许你指定一个集合，声明可以在元素体内使用的集合项（item）和索引（index）变量。它也允许你指定开头与结尾的字符串以及在迭代结果之间放置分隔符。这个元素是很智能的，因此它不会偶然地附加多余的分隔符。</br>
可以将任何可迭代对象（如 List、Set 等）、Map 对象或者数组对象传递给 foreach 作为集合参数。当使用可迭代对象或者数组时，index 是当前迭代的次数，item 的值是本次迭代获取的元素。当使用 Map 对象（或者 Map.Entry 对象的集合）时，index 是键，item 是值。</br>
####3.4、查询方法
打开币种页面，默认查询页面数据
**1)	Train_currtypeController**</br>
根据前台传入的参数，构造后台查询参数</br>
PageRequest：pageNumber页数，pagesize页面数据大小，Sort排序
![](https://i.imgur.com/s9uShAt.jpg)
 
**2）Train_cutttypeService**</br>
调用Dao方法查询</br>
![](https://i.imgur.com/g1T0Ori.jpg)
 
**3）Train_currtypeMapper**</br>
接口方法定义</br>
 ![](https://i.imgur.com/9qrDcGy.jpg)

**4）映射文件xml**

![](https://i.imgur.com/njPViI2.jpg) 

iuap-mybatis组件会加载插件处理分页参数，并封装结果集返回</br>
在applicationContext-persistence.xml配置文件中如下所示：
注意在方法名要以selectAllByPage结尾
 
![](https://i.imgur.com/Ipis07q.jpg)

####3.5、保存
**1)	Train_currtypeControlle**</br>
传入的参数为list结果集

![](https://i.imgur.com/5mIfGt9.jpg)
 
**2）Train_currtypeService**

![](https://i.imgur.com/ksP23Cw.jpg)
 
**3) Train_currtypeMapper**

![](https://i.imgur.com/LycDs5c.jpg)
 
**4）映射文件xml**</br>
 ![](https://i.imgur.com/gDRDOHb.jpg)

![](https://i.imgur.com/HIWQNBg.jpg)

说明：<foreach>标签实现sql条件的循环，可完成类似批量的sql
<foreach>标签的用法：</br>
六个参数：</br>
- collection：要循环的集合

- index：循环索引

- item：集合中的一个元素（item和collection，按foreach循环理解）
  
- open：以什么开始
 
- close：以什么结束
 
- separator：循环内容之间以什么分隔

#### 3.6、删除

1)	Train_currtypeControlle.del</br>
传入的参数为被删除的币种实体数组，同时调用后台删除</br>
 ![](https://i.imgur.com/saSj1pn.jpg)

2）Train_cutttypeService</br>
调用mapper的删除方法</br>
![](https://i.imgur.com/qZokRTm.jpg)
 
3）Train_currtypeMapper</br>
批量删除方法</br>

![](https://i.imgur.com/W8JNfui.jpg)
 
4）映射文件xml</br>
![](https://i.imgur.com/66SVUHn.jpg) 

### 4、前台代码解析

#### 4.1、前端页面架构

![](https://i.imgur.com/tDnUFIH.jpg) 

Router——前端路由框架</br>
AMD——模块化管理工具</br>
数据模型——处理企业级复杂数据交互</br>
UI——快速构建前端页面</br>
UI模型——处理UI和数据模型之间的数据通信</br>

####4.2、单页面应用(SPA)
前端界面采用的单页面应用，单页Web应用（single page web application，SPA），就是只有一张Web页面的应用。单页应用程序 (SPA) 是加载单个HTML 页面并在用户与应用程序交互时动态更新该页面的Web应用程序。 浏览器一开始会加载必需的HTML、CSS和JavaScript，所有的操作都在这张页面上完成，都由JavaScript来控制。因此，对单页应用来说模块化的开发和设计显得相当重要。

单页面应用的优点是：</br>


- 速度：更好的用户体验，让用户在web app感受native app的速度和流畅，</br>
- MVC：经典MVC开发模式，前后端各负其责。</br>
- ajax：重前端，业务逻辑全部在本地操作，数据都需要通过AJAX同步、提交。</br>
- 路由：在URL中采用#号来作为当前视图的地址,改变#号后的参数，页面并不会重载</br>

在平台中,实际只有index.html这一个单页面，在这个单页面中，加载了必须的CSS和JavaScript。然后有一个内容区，在点击菜单时，改变地址hash，我们用hash的变化从而推动界面相应的功能变化并在内容区中渲染出来。</br>
那如何根据hash变化调用所需的js方法呢？来让前端路由吧！

#### 4.3、前端路由

前端路由主要是用在单页面应用中，解决单页面应用里，地址变化，整体页面并不需要重新渲染，只重新渲染局内容。
平台中采用director.js开源框架做为默认的路由框架。基本用法如下：


1	var router = Router();</br>
2	//定义order路径所对应的操作</br>
3	router.on('/order', function(){</br>
4	    // 具体的实现代码</br>
5	});</br>
6	 </br>
7	router.init();</br>

当浏览器地址变为：http://localhost#/ currtype时,会触发上面监听的function，找到对应的币种节点。</br>
在平台前端主页面中，使用了router的相关方法来监听地址变化。同时使用requirejs来做动态加载。从而实现的整个单页面应用的运转。

#### 4.4、AMD模块化规范

AMD是"异步模块定义”。它采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。</br>
AMD采用require()语句加载模块，但是不同于CommonJS，它要求两个参数：</br>
1	require([module], callback);</br>
第一个参数[module]，是一个数组，里面的成员就是要加载的模块；第二个参数callback，则是加载成功之后的回调函数。例如:</br>
1	require(['math'], function (math) {</br>
2	　　math.add(2, 3);</br>
3	});</br>
math.add()与math模块加载不是同步的，加载完math模块，运用math的add方法，这样浏览器不会发生假死现象。所以很显然，AMD比较适合浏览器环境。</br>
目前，require.js是主要是实现了AMD规范的javascript库。平台中也是使用的require.js做为AMD模块化规范。可参见：http://www.requirejs.cn/ 学习requirejs的用法。

#### 4.5、Currtype.html解析
 
![](https://i.imgur.com/QZfPAWf.jpg)

![](https://i.imgur.com/qDN55UW.jpg)

![](https://i.imgur.com/2xxECqh.jpg)
 

 
#### 4.6、meta.js

向导生成数据模型，字段默认类型为String

![](https://i.imgur.com/ppVjDZC.jpg)
 
#### 4.7、Currtype.js解析

使用requirejs的方式引用，前三个分别是：对应的html、数据模型js、对用的css。

![](https://i.imgur.com/fkgYtHW.jpg) 

事件：
打开节点时的页面初始化方法pageInit()，初始化界面及数据模型：
 
![](https://i.imgur.com/2Wtxfq8.jpg)

新增修改保存方法：
 
 ![](https://i.imgur.com/mcUQ1ek.jpg)

![](https://i.imgur.com/22Ek5pJ.jpg)

![](https://i.imgur.com/s8fRkjf.jpg)
 
### 5项目配置解析

#### 5.1、Maven配置

iUAP开发工具使用Maven作为软件项目管理工具。使用Maven构建工程、管理各服务间的jar包依赖。同时配置本地仓库统一管理外部依赖包和内部依赖包。</br>
配置maven需要以下几个步骤：</br>
1、确认私服地址。</br>
如果项目内部需要建立自己的私服。则可以参考《maven工具应用》文档创建。如果使用用友网络内部私服，则直接使用默认地址即可。</br>
2、确认项目需要应用的组件jar包、版本等；</br>
3、配置本地jar包私有仓库位置。</br>
	Maven有两个重要的配置文件，pom.xml和settings.xml。

**pom.xml**

Pom.xml配置文件可以配置项目内部依赖、构建等jar包关系。</br>
Pom.xml的位置在项目根目录下

![](https://i.imgur.com/bUXtO5g.jpg)
 
配置文件解析：包含基础配置信息、依赖信息、构建信息等及部分组成。</br>
**1、基础配置信息：**</br>


- groupId：项目或者组织的唯一标志，并且配置时生成路径也是由此生成，如org.myproject.mojo生成的相对路径为：/org/myproject/mojo；
- artifactId：项目的通用名称
- version：项目的版本
- packaging：打包机制，如pom,jar,maven-plugin,ejb,war,ear,rar,par。
- name：用户描述项目的名称，可选
- description：对项目的描述，可选。
- properties是为pom定义一些常量，在pom中的其它地方可以直接引用。
【如：配置<iuap.modules.version>2.0.1-RELEASE</iuap.modules.version>，则使用时为${iuap.modules.version }】</br>
**2、依赖关系**

- groupId、artifactId、version：这三个组合标示依赖的具体工程的配置，这个依赖工程必需是maven中心包管理范围内的；
- exclusions：声明maven排除依赖jar处理，但是这样在某些时候会造成一些不可预测的异常。比如如果X需要A，A包含B依赖，那么X可以声明不要B依赖，只要在exclusions中写入

**3、构建插件**

- groupId、artifactId、version：这三个组合标示依赖的具体插件工程的配置；
- configuration依赖插件的配置；

![](https://i.imgur.com/8jgLcD8.jpg)
 
**settings.xml**

settings.xml：定义Maven的全局环境信息。可以通过这个文件来定义本地仓库、远程仓库和联网使用的代理信息等。
Settings.xml的位置可以在工具中找到：
在iUAPStudio中菜单栏，窗口—>首选项，选择Maven—>User Settings找到配置文件的目录。
 
![](https://i.imgur.com/puO4xKa.jpg)

Settings.xml主要功能是配置本地仓库，依赖包的地址。</br>
1、本地jar包仓库位置的配置：</br>
localRepository：表示Maven用来在本地储存信息的本地仓库的目录。
![](https://i.imgur.com/tAvAAkw.jpg) 

2、Maven依赖包的地址
Maven会通过pom中的配置id匹配到settings.xml中的profile的id，并找到对应的下载地址并下载对应版本的jar包。
Repository: 存储组件的仓库
 ![](https://i.imgur.com/jvSqVVO.jpg)

pluginRepositories：存储plugin插件的仓库，方法类似

#### 5.2、Spring集成

iuap 平台集成了Spring框架进行组件的配置和管理，以及Spring MVC作为后端MVC框架，更方便业务开发者进行开发使用。</br>
![](https://i.imgur.com/pZayfc3.jpg) 

从pom.xml中可以查看到Spring的版本，集成时此版本务必保持一致。

![](https://i.imgur.com/nytnJZO.jpg)
 
#### 5.3 SpringMVC

Spring MVC是当前最优秀的MVC框架，自从Spring 2.5版本发布后，由于支持注解配置，易用性有了大幅度的提高。Spring 3.0更加完善，实现了对Struts 2的超越。现在越来越多的开发团队选择了Spring MVC。</br>
配置解析</br>
1.Dispatcherservlet</br>
　　DispatcherServlet是前置控制器，配置在web.xml文件中的。拦截匹配的请求，Servlet拦截匹配规则要自已定义，把拦截下来的请求，依据相应的规则分发到目标Controller来处理，是配置spring MVC的第一步。
2.InternalResourceViewResolver
　　视图名称解析器
3.以上出现的注解
　　@Controller 负责注册一个bean 到spring 上下文中
　　@RequestMapping 注解为控制器指定可以处理哪些 URL 请求

**SpringMVC常用注解**</br>
　　@Controller</br>
　　负责注册一个bean 到spring 上下文中</br>
　　@RequestMapping</br>
　　注解为控制器指定可以处理哪些 URL 请求</br>
　　@RequestBody</br>
　　该注解用于读取Request请求的body部分数据，使用系统默认配置的HttpMessageConverter进行解析，然后把相应的数据绑定到要返回的对象上 ,再把HttpMessageConverter返回的对象数据绑定到 controller中方法的参数上</br>
　　@ResponseBody</br>
　　该注解用于将Controller的方法返回的对象，通过适当的HttpMessageConverter转换为指定格式后，写入到Response对象的body数据区</br>
　　@ModelAttribute 　　</br>　
　　在方法定义上使用 @ModelAttribute 注解：Spring MVC 在调用目标处理方法前，会先逐个调用在方法级上标注了@ModelAttribute 的方法</br>
　　在方法的入参前使用 @ModelAttribute 注解：可以从隐含对象中获取隐含的模型数据中获取对象，再将请求参数 –绑定到对象中，再传入入参将方法入参对象添加到模型中 </br>
　　@RequestParam　</br>
　　在处理方法入参处使用 @RequestParam 可以把请求参 数传递给请求方法</br>
　　@PathVariable</br>
　　绑定 URL 占位符到入参</br>
　　@ExceptionHandler</br>
　　注解到方法上，出现异常时会执行该方法</br>
　　@ControllerAdvice</br>
　　使一个Contoller成为全局的异常处理类，类中用@ExceptionHandler方法注解的方法可以处理所有Controller发生的异常</br>

## 三、功能开发

## 1、保存校验

保存时增加校验，编码、名称不能为空，编码名称唯一

校验类：</br>
package com.yonyou.iuap.appdemo.service;

import java.util.List;
import org.springframework.beans.factory.annotation.Autowired;
import com.yonyou.iuap.appdemo.entity.Train_currtype;
import com.yonyou.iuap.appdemo.mapper.Train_currtypeMapper;
import com.yonyou.iuap.iweb.exception.ValidException;
import com.yonyou.iuap.persistence.vo.pub.util.StringUtil;

public class CodeNameNotNullMeValidator {

	@Autowired
	private Train_currtypeMapper dao;

	public void valid(Train_currtype entity) {
		if (entity == null) {
			throw new ValidException("提交的数据集为空!");
		}
		
		/**
		 * 校验非空项
		 */
		StringBuilder builder = new StringBuilder();
		if (StringUtil.isEmpty(entity.getCode())) {
			builder.append("编码不能为空!\n");
		}

		if (StringUtil.isEmpty(entity.getName())) {
			builder.append("名称不能为空!");
		}
		if (builder.length() > 0) {
			throw new ValidException(builder.toString());
		}

		/**
		 * 校验重复
		 */
		List<Train_currtype> eqentity = dao.findByCode(entity.getCode());
		if (eqentity != null&& eqentity.size()>0) {
			builder.append(entity.getCode()).append(",");
		}

		eqentity = dao.findByName(entity.getName());
		if (eqentity != null&& eqentity.size()>0) {
			builder.append(entity.getName()).append(",");
		}

		if (builder.toString().length() > 0) {
			String codeStr = builder.deleteCharAt(builder.length() - 1)
					.toString();
			StringBuilder msgBuilder = new StringBuilder("编码为");
			msgBuilder.append(codeStr).append("的记录已经存在!");
			throw new ValidException(msgBuilder.toString());
		}
	}

}


dao中添加根据名称查询方法
 
![](https://i.imgur.com/M7COup5.jpg)

SQL映射文件train_currtype.xml增加名称查询方法
 
![](https://i.imgur.com/etmvN8p.jpg)