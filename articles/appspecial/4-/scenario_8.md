# 数据权限 

**组件应用**

## 1	数据权限开发

### 1.1	 功能原理

业务系统在使用时，一般会需要进行访问控制，需要以系统内的用户身份登录系统后，才能访问系统内的相关功能或资源，同时不同的用户能访问的功能一般也不一致，需要根据不同的用户进行控制，由此引入了登录认证和权限控制的要求。

### 1.2	 数据权限应用
数据权限</br>
以系统管理员admin进入系统，管理中心下，找到数据权限节点。选择需要控制的“数据权限”业务角色，点击分配功能，可以选择可以进行数据权限控制的档案。此处以“组织”档案为例。</br>
![](/articles/application/4-/images/08/image2.png)
 
选择后，点击确定后，在界面中点击“分配”功能，可以给对应角色分配有权的“组织”</br>
 ![](/articles/application/4-/images/08/image3.png)

点击“分配”后，可以选择分配的组织</br>
![](/articles/application/4-/images/08/image3.png) 

点击确定后，分配完成</br>
![](/articles/application/4-/images/08/image4.png) 

配置完成后，当以业务角色1的用户sale1登陆后，在节点上打开功能点，加载数据时，按照数据权限过滤，仅能查看有权限档案数据。</br>
 ![](/articles/application/4-/images/08/image5.png)

编辑时，点击参照时，按照权限过滤</br>
![](/articles/application/4-/images/08/image6.png)

![](/articles/application/4-/images/08/image7.png)


###1.3	 数据权限开发

#### 1.3.1	 环境配置

1.	pom.xml中配置jar包依赖
        <!--数据权限SDK -->
		<dependency>
			<groupId>com.yonyou.iuap</groupId>
			<artifactId>iuap-authrbac-sdk</artifactId>
			<version>3.2.0-RC001</version>
		</dependency>

2.	配置用户登录SDK的配置文件authrbac-sdk.properties，位置如下图所示</br>
  ![](/articles/application/4-/images/08/image8.png)

文件内配置权限验证服务的地址，如下图中配置了工作台地址</br>
 ![](/articles/application/4-/images/08/image9.png)
 
#### 1.3.2	 资源注册

1.	参照注册ref_refinfo
 </br>
 ![](/articles/application/4-/images/08/image10.png)

2.	ieop_dpprofile_reg 权限资源注册表，在表中可以参考已有资源，注册案例中的币种资源，如下图所示。</br>
  ![](/articles/application/4-/images/08/image11.png)

resourcetypecode：为用户分配权限后，实现代码的时候要依据此字段来查询权限。</br>
dataconverturl：负责将币种转成应用平台可接受的格式以供应用平台展现数据。在代码部分会有详细介绍。

#### 1.3.3	 代码示例

##### 1.3.3.1	数据转换url的实现

在数据权限设置界面，需要根据存储的资源ID数据获取详细页面数据。所以需要业务组提供查询方法，根据资源ID加载业务数据。如下图所示，需要提供下图中红框部分的配置展示数据。
  ![](/articles/application/4-/images/08/image12.png) 

在com.yonyou.iuap.appdemo.web.Train_currtypeController类中增加数据权限服务，条件中传入需要查询的ID，结果集中返回data数据
	/**
	 * 币种提供数据权限
	 * 
	 * @param request
	 * @return
	 */
	@RequestMapping(value = { "/getByIds" }, method = { RequestMethod.POST })
	@ResponseBody
	public JSONObject getByIds(HttpServletRequest request) {
		JSONObject result = new JSONObject();
		String data = request.getParameter("data");
		if (StringUtil.isEmpty(data)) {
			return result;
		}
		JSONArray array = JSON.parseArray(data);

		List<Train_currtype> currtypeList = new ArrayList<Train_currtype>();
		if (!CollectionUtils.isEmpty(array)) {
			String[] strArray = (String[]) array.toArray(new String[array.size()]);
			currtypeList = this.service.getCurrtypeByIds(strArray);
		}
		result.put("data", transformToBriefEntity(currtypeList));
		return result;
	}
	
	private List<OrganizationBrief> transformToBriefEntity(List<Train_currtype> currtypeList){
	    List<OrganizationBrief> results = new ArrayList<OrganizationBrief>();
	    if (!CollectionUtils.isEmpty(currtypeList)) {
	      for (Train_currtype entity : currtypeList) {
	        results.add(new OrganizationBrief(entity.getPk_currtype(), entity.getName(), entity.getCode()));
	      }
	    }
	    return results;
	 }


##### 1.3.3.2	参照模型配置权限

参照模型com.yonyou.iuap.refmodel.CurrtypeRefController，在加载数据时判断是否需要支持权限，如果支持则拼接对应权限SQL

1.	判断前端传入的参数，是否需要启动数据权限
	/**
	 * 是否需要启用数据权限，通过当前人角色以及前端配置共同判断
	 * @param refViewModelVo
	 * @return
	 */
	private boolean isUserDataPower(RefViewModelVO refViewModelVo) {
		if (isAdmin()) {
			return false;
		}

		boolean isUserDataPower = false;

		String clientParam = refViewModelVo.getClientParam();
		if ((clientParam != null) && (clientParam.trim().length() > 0)) {
			net.sf.json.JSONObject json = net.sf.json.JSONObject
					.fromObject(clientParam);
			if (json.containsKey("isUseDataPower")) {
				isUserDataPower = json.getBoolean("isUseDataPower");
			}
		}

		return isUserDataPower;
	}
查询或者搜索时加载权限SQL
	/**
	 * 查询参照数据
	 */
	@Override
	public @ResponseBody Map<String, Object> getCommonRefData(
			@RequestBody RefViewModelVO refViewModelVo) {
		
		Map<String, Object> results = new HashMap<String, Object>();
		
		int pageNum = refViewModelVo.getRefClientPageInfo().getCurrPageIndex() == 0 ? 1
				: refViewModelVo.getRefClientPageInfo().getCurrPageIndex();

		int pageSize = refViewModelVo.getRefClientPageInfo().getPageSize();
		
		PageRequest request = buildPageRequest(pageNum,pageSize,null);
		
		String searchParam = StringUtils.isEmpty(refViewModelVo.getContent()) ? null: refViewModelVo.getContent();
		
		Page<Train_currtype> pageUser = this.service.selectAllByPage(request,
				buildSearchParam(searchParam));

		List<Train_currtype> peopleList = pageUser.getContent();
		if (CollectionUtils.isNotEmpty(peopleList)) {
			List<Map<String, String>> list = buildRtnValsOfRef(peopleList,isUserDataPower(refViewModelVo));

			RefClientPageInfo refClientPageInfo = refViewModelVo
					.getRefClientPageInfo();
			refClientPageInfo.setPageCount(pageUser.getTotalPages());
			refClientPageInfo.setPageSize(50);
			refViewModelVo.setRefClientPageInfo(refClientPageInfo);

			results.put("dataList", list);
			results.put("refViewModel", refViewModelVo);
		}

		return results;
	}

调用权限API查询用户权限，并过滤数据集
	/**
	 * 参照数据组装
	 * 
	 * @param peoplelist
	 * @return
	 */
	private List<Map<String, String>> buildRtnValsOfRef(List<Train_currtype> list, boolean isUserDataPower) {
		// 数据权限集合set
		String tenantId = InvocationInfoProxy.getTenantid();
		String sysId = InvocationInfoProxy.getSysid();
		String userId = InvocationInfoProxy.getUserid();
		List<DataPermission> dataPerms = AuthRbacClient.getInstance().queryDataPerms(tenantId, sysId, userId, "currtype");
		
		Set<String> set = new HashSet<String>();
		for(DataPermission temp : dataPerms){
			set.add(temp.getResourceId());
		}
		
		List<Map<String, String>> results = new ArrayList<Map<String,String>>();

		if ((list != null) && (!list.isEmpty())) {
			for (Train_currtype entity : list) {
				if (!isUserDataPower || (isUserDataPower && set.contains(entity.getPk_currtype()))) {
					Map<String, String> refDataMap = new HashMap<String, String>();
					refDataMap.put("id", entity.getPk_currtype());
					refDataMap.put("refname", entity.getName());
					refDataMap.put("refcode", entity.getCode());
					refDataMap.put("refpk", entity.getPk_currtype());
					
					results.add(refDataMap);
				}
			}
		}
		return results;
	}


Model完整代码如下：
package com.yonyou.iuap.refmodel;

import iuap.ref.ref.RefClientPageInfo;
import iuap.ref.sdk.refmodel.model.AbstractGridRefModel;
import iuap.ref.sdk.refmodel.vo.RefViewModelVO;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;

import org.apache.commons.collections4.CollectionUtils;
import org.apache.commons.lang3.StringUtils;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Sort;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

import com.yonyou.iuap.appdemo.entity.Train_currtype;
import com.yonyou.iuap.appdemo.service.Train_currtypeService;
import com.yonyou.iuap.context.InvocationInfoProxy;
import com.yonyou.iuap.mvc.type.SearchParams;
import com.yonyou.uap.ieop.security.entity.DataPermission;
import com.yonyou.uap.ieop.security.sdk.AuthRbacClient;

/**
 * 
 * 币种档案参照controller
 *
 */
@RestController
@RequestMapping({ "/restcurrtype" })
public class CurrtypeRefController extends AbstractGridRefModel {
	
	private static final Logger log = LoggerFactory.getLogger(CurrtypeRefController.class);

	@Autowired
	private Train_currtypeService service;
	
	

	/**
	 * 过滤
	 */
	@Override
	public List<Map<String, String>> filterRefJSON(	@RequestBody RefViewModelVO paramRefViewModelVO) {
		List<Map<String, String>> results = new ArrayList<Map<String,String>>();
		List<Train_currtype> rtnVal = this.service.likeSearch(paramRefViewModelVO.getContent());
		results = buildRtnValsOfRef(rtnVal,isUserDataPower(paramRefViewModelVO));
		return results;
	}

	@Override
	public List<Map<String, String>> matchPKRefJSON(@RequestBody RefViewModelVO paramRefViewModelVO) {
		List<Map<String, String>> results = new ArrayList<Map<String,String>>();
		try {
			List<Train_currtype> rtnVal = this.service.getByIds(null,Arrays.asList(paramRefViewModelVO.getPk_val()));
			results = buildRtnValsOfRef(rtnVal,isUserDataPower(paramRefViewModelVO));
		} catch (Exception e) {
			log.error("服务异常：", e);
		}
		return results;
	}

	@Override
	public List<Map<String, String>> matchBlurRefJSON(
			@RequestBody RefViewModelVO paramRefViewModelVO) {
		return null;
	}

	/**
	 * 查询参照数据
	 */
	@Override
	public @ResponseBody Map<String, Object> getCommonRefData(
			@RequestBody RefViewModelVO refViewModelVo) {
		
		Map<String, Object> results = new HashMap<String, Object>();
		
		int pageNum = refViewModelVo.getRefClientPageInfo().getCurrPageIndex() == 0 ? 1
				: refViewModelVo.getRefClientPageInfo().getCurrPageIndex();

		int pageSize = refViewModelVo.getRefClientPageInfo().getPageSize();
		
		PageRequest request = buildPageRequest(pageNum,pageSize,null);
		
		String searchParam = StringUtils.isEmpty(refViewModelVo.getContent()) ? null: refViewModelVo.getContent();
		
		Page<Train_currtype> pageUser = this.service.selectAllByPage(request,
				buildSearchParam(searchParam));

		List<Train_currtype> peopleList = pageUser.getContent();
		if (CollectionUtils.isNotEmpty(peopleList)) {
			List<Map<String, String>> list = buildRtnValsOfRef(peopleList,isUserDataPower(refViewModelVo));

			RefClientPageInfo refClientPageInfo = refViewModelVo
					.getRefClientPageInfo();
			refClientPageInfo.setPageCount(pageUser.getTotalPages());
			refClientPageInfo.setPageSize(50);
			refViewModelVo.setRefClientPageInfo(refClientPageInfo);

			results.put("dataList", list);
			results.put("refViewModel", refViewModelVo);
		}

		return results;
	}

	private SearchParams buildSearchParam(String searchParam) {
		SearchParams param = new SearchParams();

		Map<String, Object> results = new HashMap<String, Object>();
		if (StringUtils.isNotEmpty(searchParam)) {
			results.put("searchParam", searchParam);
		}

		param.setSearchMap(results);
		return param;
	}

	/**
	 * 参照定义
	 */
	@Override
	public RefViewModelVO getRefModelInfo(
			@RequestBody RefViewModelVO refViewModel) {
		RefViewModelVO retinfo = super.getRefModelInfo(refViewModel);
		refViewModel.setRefName("币种");
		refViewModel.setRootName("币种列表");
		return retinfo;
	}

	/**
	 * 参照数据组装
	 * 
	 * @param peoplelist
	 * @return
	 */
	private List<Map<String, String>> buildRtnValsOfRef(List<Train_currtype> list, boolean isUserDataPower) {
		// 数据权限集合set
		String tenantId = InvocationInfoProxy.getTenantid();
		String sysId = InvocationInfoProxy.getSysid();
		String userId = InvocationInfoProxy.getUserid();
		List<DataPermission> dataPerms = AuthRbacClient.getInstance().queryDataPerms(tenantId, sysId, userId, "currtype");
		
		Set<String> set = new HashSet<String>();
		if(dataPerms!=null&&dataPerms.size()>0){
			for(DataPermission temp : dataPerms){
				if(temp!=null)
					set.add(temp.getResourceId());
			}
		}
		
		List<Map<String, String>> results = new ArrayList<Map<String,String>>();

		if ((list != null) && (!list.isEmpty())) {
			for (Train_currtype entity : list) {
				if (!isUserDataPower || (isUserDataPower && set.contains(entity.getPk_currtype()))) {
					Map<String, String> refDataMap = new HashMap<String, String>();
					refDataMap.put("id", entity.getPk_currtype());
					refDataMap.put("refname", entity.getName());
					refDataMap.put("refcode", entity.getCode());
					refDataMap.put("refpk", entity.getPk_currtype());
					
					results.add(refDataMap);
				}
			}
		}
		return results;
	}
	
	/**
	 * 是否需要启用数据权限，通过当前人角色以及前端配置共同判断
	 * @param refViewModelVo
	 * @return
	 */
	private boolean isUserDataPower(RefViewModelVO refViewModelVo) {
		if (isAdmin()) {
			return false;
		}

		boolean isUserDataPower = false;

		String clientParam = refViewModelVo.getClientParam();
		if ((clientParam != null) && (clientParam.trim().length() > 0)) {
			net.sf.json.JSONObject json = net.sf.json.JSONObject
					.fromObject(clientParam);
			if (json.containsKey("isUseDataPower")) {
				isUserDataPower = json.getBoolean("isUseDataPower");
			}
		}

		return isUserDataPower;
	}

	/**
	 * 判断是否系统管理员
	 * @return
	 */
	private boolean isAdmin() {
		String userId = InvocationInfoProxy.getUserid();
		if (userId.equals("U001")) {
			return true;
		}
		return false;
	}
	
	/**
	 * 构造分页参数
	 * @param pageNum
	 * @param pageSize
	 * @param sortColumn
	 * @return
	 */
	private PageRequest buildPageRequest(int pageNum, int pageSize,
			String sortColumn) {
		Sort sort = null;
		if (("auto".equalsIgnoreCase(sortColumn))
				|| (StringUtils.isEmpty(sortColumn))) {
			sort = new Sort(Sort.Direction.ASC, new String[] { "code" });
		} else {
			sort = new Sort(Sort.Direction.DESC, new String[] { sortColumn });
		}
		return new PageRequest(pageNum - 1, pageSize, sort);
	}
}
##### 1.3.3.3	根据数据权限过滤数据

业务单据查询时，根据数据权限过滤业务数据

###### 1.3.3.3.1	获取数据权限过滤数据API

List<DataPermission> com.yonyou.uap.ieop.security.sdk.AuthRbacClient.queryDataPerms(String tenantId, String sysId, String userId, String resourceTypeCode)

查询参数：

- tenantId租户ID
- sysId 系统ID
- userId 当前用户ID
- resourceTypeCode数据权限资源编码

返回结果集为有权限的数据权限数组，从如下方法可以获取资源ID
com.yonyou.uap.ieop.security.entity.DataPermission.getResourceId()

###### 1.3.3.3.2	API代码调用

1.	在分页查询方法中，拼接并传入参数SQL，返回结果集
  ![](/articles/application/4-/images/08/image13.png) 

2.	拼接SQL代码示例
	/**
	 * 构造数据权限SQL
	 * @return
	 */
	public String buildPermSql() {
		StringBuilder sqlBuilder = new StringBuilder();
		
		sqlBuilder.append(String.format(" and %s='%s'", CommonConstants.TENANT_FiELEDNAME, InvocationInfoProxy.getTenantid()));
	    
		HashMap<String, String> fieldDataPermResTypeMap = processFieldDataPermResTypeMap();
    	
    	Set<String> keySet = fieldDataPermResTypeMap.keySet();
    	Map<String, Set<String>> resTypeDataPermMap = new HashMap<String, Set<String>>();
    		
    	for (String columnname : keySet) {
			String resourceTypeCode = fieldDataPermResTypeMap.get(columnname);
			Set<String> set = resTypeDataPermMap.get(resourceTypeCode);
				
			if(set == null){
				set = getAuthData(resourceTypeCode);
				resTypeDataPermMap.put(resourceTypeCode, set);
			}
				
			if(!set.isEmpty()){					
				sqlBuilder.append(" and ").append(columnname).append(" in (");
					
				for (String param : set) {						
					sqlBuilder.append(String.format("'%s',", param));
				}
					
				sqlBuilder.deleteCharAt(sqlBuilder.length() - 1);
				sqlBuilder.append(")");
			}
		}
    	    	
    	return sqlBuilder.toString();
	}
3.	返回需要支持数据权限的资源，可以配置多个，配置多个时，在上面方法中需要对多个资源处理
	/**
	 * 需要支持数据权限的资源
	 * @return
	 */
	private HashMap<String, String> processFieldDataPermResTypeMap(){
		HashMap<String, String> fieldDataPermResTypeMap = new HashMap<String, String>();
		fieldDataPermResTypeMap.put("pk_currtype", "currtype"); //字段与权限资源名称的对应关系
		return fieldDataPermResTypeMap;
	}
4.	调用API获取权限数据
	/**
	 * 调用API获取有权限的数据
	 * @param resourceTypeCode
	 * @return
	 */
	private Set<String> getAuthData(String resourceTypeCode) {
		String tenantId = InvocationInfoProxy.getTenantid();
		String sysId = InvocationInfoProxy.getSysid();
		String userId = InvocationInfoProxy.getUserid();
		
		List<DataPermission> dataPerms = AuthRbacClient.getInstance().queryDataPerms(tenantId, sysId, userId, resourceTypeCode);
		
		Set<String> set = new HashSet<String>();
		for (Iterator<DataPermission> iterator = dataPerms.iterator(); iterator.hasNext();) {
			DataPermission dataPermission = (DataPermission) iterator.next();
			set.add(dataPermission.getResourceId());
		}
		
		return set;
	}

### 1.4	应用效果

1.	给角色分配数据权限资源
 
  ![](/articles/application/4-/images/08/image14.png) 

2.	打开业务节点时，仅能加载出有权限的业务数据
 
  ![](/articles/application/4-/images/08/image15.png)
 
3.	新增时，点击币种参照，仅能查询出已分配的权限资源。
 
  ![](/articles/application/4-/images/08/image16.png) 
