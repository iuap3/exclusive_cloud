
#调用主数据REST服务客户端实现示例
##数据写入
###RestTemplate实现

import java.net.URI;
import java.net.URISyntaxException;
import javax.annotation.Resource;
import org.apache.log4j.Logger;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpMethod;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Component;
import org.springframework.web.client.RestClientException;
import org.springframework.web.client.RestTemplate;

import com.alibaba.fastjson.JSONArray;
import com.alibaba.fastjson.JSONObject;

@Component
public class RestClient {
	
	private static Logger logger = Logger.getLogger(RestClient.class);
	@Resource(name="simpleRestTemplate")
	RestTemplate restTmpl;
	
	public void invoke() {
		try {
			URI url = new URI("http://localhost:8082/iuapmdm/cxf/mdmrs/center/com.yonyou.iuapmdm.centerService/insertMd");
			JSONObject requestObj = new JSONObject();
			HttpHeaders header = new HttpHeaders();
			header.set("mdmtoken", "345cf06f-3d83-4155-920e-7fc2bb199826");
			header.set("tenantid", "kkfami91");
			header.setContentType(MediaType.APPLICATION_JSON);
			
			HttpEntity<JSONObject> requestEntity = new HttpEntity<JSONObject>(requestObj, header);
			JSONArray masterData = new JSONArray();
			JSONObject row = new JSONObject();
			row.put("id", "rest"+System.currentTimeMillis());
			row.put("a1", "a1_+"+System.currentTimeMillis());
			row.put("a2", "a2_+"+System.currentTimeMillis());
			
			masterData.add(row);
			
			requestObj.put("systemCode", "hr");
			requestObj.put("masterData", masterData.toJSONString());
			requestObj.put("gdCode", "yltest");
			
			ResponseEntity<JSONObject> responseEntity = 
					restTmpl.exchange(url , HttpMethod.POST, requestEntity , JSONObject.class);
			JSONObject responseObj = responseEntity.getBody();
			
			logger.info(responseObj);
			
		} catch (RestClientException e) {
			logger.error(e);
		} catch (URISyntaxException e) {
			logger.error(e);
		}
	}
}



# 分发接口REST服务实现示例

##分发实体VO
###参数实体VO
import java.io.Serializable;

import javax.xml.bind.annotation.XmlRootElement;

@XmlRootElement(name="distributePostDataVO")
public class DistributePostDataVO implements Serializable{

	private static final long serialVersionUID = -923702730339876372L;

	private String systemCode;
	private String mdType;
	private String action;
	private String masterData;
	public DistributePostDataVO() {
		super();
	}
	public DistributePostDataVO(String systemCode, String mdType, String action,
			String masterData) {
		super();
		this.systemCode = systemCode;
		this.mdType = mdType;
		this.action = action;
		this.masterData = masterData;
	}
	public String getSystemCode() {
		return systemCode;
	}
	public void setSystemCode(String systemCode) {
		this.systemCode = systemCode;
	}
	public String getMdType() {
		return mdType;
	}
	public void setMdType(String mdType) {
		this.mdType = mdType;
	}
	public String getAction() {
		return action;
	}
	public void setAction(String action) {
		this.action = action;
	}
	public String getMasterData() {
		return masterData;
	}
	public void setMasterData(String masterData) {
		this.masterData = masterData;
	}
	@Override
	public String toString() {
		return "DistributePostDataVO [systemCode=" + systemCode + ", mdType="
				+ mdType + ", action=" + action + ", masterData=" + masterData
				+ "]";
	}
	
}

###返回值实体VO

import java.io.Serializable;
import java.util.List;

import javax.xml.bind.annotation.XmlRootElement;

@XmlRootElement(name="distributeRetVO")
public class DistributeRetVO implements Serializable{

	private static final long serialVersionUID = 683574201644656079L;

	private List<MdMapingVO> mdMappings;
	private boolean success;
	private String message;

	public DistributeRetVO() {
		super();
	}

	public List<MdMapingVO> getMdMappings() {
		return mdMappings;
	}

	public void setMdMappings(List<MdMapingVO> mdMappings) {
		this.mdMappings = mdMappings;
	}

	public boolean isSuccess() {
		return success;
	}

	public void setSuccess(boolean success) {
		this.success = success;
	}

	public String getMessage() {
		return message;
	}

	public void setMessage(String message) {
		this.message = message;
	}

	@Override
	public String toString() {
		return "DistributeRetVO [mdMappings=" + mdMappings + ", success="
				+ success + ", message=" + message + "]";
	}

}

###映射关系实体VO
import java.io.Serializable;

import javax.xml.bind.annotation.XmlRootElement;

@XmlRootElement(name="mdMapingVO")
public class MdMapingVO implements Serializable{

	private static final long serialVersionUID = -5488253314295792733L;
	private String mdmCode;
	private String entityCode;  
	private String busiDataId;
	public MdMapingVO() {
		super();
	}
	public MdMapingVO(String mdmCode, String entityCode, String busiDataId) {
		super();
		this.mdmCode = mdmCode;
		this.entityCode = entityCode;
		this.busiDataId = busiDataId;
	}
	public String getMdmCode() {
		return mdmCode;
	}
	public void setMdmCode(String mdmCode) {
		this.mdmCode = mdmCode;
	}
	public String getEntityCode() {
		return entityCode;
	}
	public void setEntityCode(String entityCode) {
		this.entityCode = entityCode;
	}
	public String getBusiDataId() {
		return busiDataId;
	}
	public void setBusiDataId(String busiDataId) {
		this.busiDataId = busiDataId;
	}
	@Override
	public String toString() {
		return "MdMapingVO [mdmCode=" + mdmCode + ", entityCode=" + entityCode
				+ ", busiDataId=" + busiDataId + "]";
	}
	
}

##接口和实现
###接口
import javax.ws.rs.POST;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;

import com.klk.rest.distribute.entity.DistributePostDataVO;
import com.klk.rest.distribute.entity.DistributeRetVO;

@Path("/distributeService")
public interface IDistributeService {
	@POST
	@Path("/distributeMd")
	@Produces({MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML})
	public DistributeRetVO distributeMd(DistributePostDataVO postData);
}

###实现示例
distributeMd方法可根据实际需求修改。

import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.io.RandomAccessFile;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import java.util.Map;

import org.apache.cxf.common.util.StringUtils;

import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;
import com.klk.rest.distribute.IDistributeService;
import com.klk.rest.distribute.entity.DistributePostDataVO;
import com.klk.rest.distribute.entity.DistributeRetVO;
import com.klk.rest.distribute.entity.MdMapingVO;

public class DistributeServiceImpl implements IDistributeService {

	public DistributeRetVO distributeMd(DistributePostDataVO postData) {
		System.out.println("Enter distributeMd.." + postData);
		List<Map<String, Object>> mdList = new Gson().fromJson(postData.getMasterData(),
				new TypeToken<List<Map<String, Object>>>() {
				}.getType());
		DistributeRetVO retVO = new DistributeRetVO();
		List<MdMapingVO> mdMapings = new ArrayList<MdMapingVO>();
		if (mdList != null && !mdList.isEmpty()) {
			for (Map<String, Object> mdMap : mdList) {
				String id = (String) mdMap.get("id");
				String mdmcode = (String) mdMap.get("mdm_code");
				MdMapingVO mapping = new MdMapingVO();
				mapping.setMdmCode(mdmcode);
//				if (StringUtils.isEmpty(id)) {
					id = String.valueOf(new Date().getTime()); // 暂时取当前时间截作为数据ID
//				}
				id = postData.getSystemCode() + "_" + id + "_" + mdmcode;
				if(id.length() > 64){
					id.substring(0,64);
				}
				mapping.setBusiDataId(id);
				mdMapings.add(mapping);
			}
		}
		retVO.setMdMappings(mdMapings);

		// 将主数据输出到文件，实施时由第三方自行处理接收的主数据
		String filename = postData.getSystemCode() + "_" + postData.getMdType() + "_" + postData.getAction();
		writeToFile(postData.getMasterData(), filename);

		retVO.setSuccess(true);
		retVO.setMessage("分发成功");

		return retVO;
	}

	private void writeToFile(String md, String filename) {
		String dir = "d:/excel/output/";
		File file = new File(dir);
		if(!file.exists()){
			file.mkdirs();
		}
		filename =dir + filename + ".txt";
		BufferedWriter writer = null;
		try {
			writer = new BufferedWriter(new
					 FileWriter(filename));
			writer.write(md);
		} catch (Exception e) {
			throw new RuntimeException("输出主数据到文件失败 : " + filename, e);
		} finally{
			try {
				writer.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}

}


