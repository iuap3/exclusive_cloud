# excel导入导出功能文档
## 1.导入导出
提供com.yonyou.iuap.common.utils.ExcelExportImportor工具类
## 2.如何开发？

## 分为前后台

#### 2.1 前台需要通过浏览器获取excel文件的二进制流传后台
示例代码：
       onUploadExcel: function(){

              var filevalue=document.getElementById("fileName").files;
              if(!filevalue ||filevalue.length < 1 ){
                  var demoInput = document.getElementById('fileName');
                  demoInput.removeEventListener('change',viewModel.event.onUploadExcel);
                  demoInput.addEventListener('change',viewModel.event.onUploadExcel);
                  return;
              }else{
                  $("#filenamediv2").html(filevalue[0].name);
              }
              var urlInfo = "/ygdemo_yw_info/excelDataImport";
              viewModel.event.uploadingshow();
              $.ajaxFileUpload({
                  url: appCtx+urlInfo,
                  secureuri:false,
                  fileElementId:'fileName',
                  dataType: 'json',
                  //data:{id},
                  success: function(data) {
                      var demoInput = document.getElementById('fileName');
                      demoInput.removeEventListener('change',viewModel.event.onUploadExcel);
                      demoInput.addEventListener('change',viewModel.event.onUploadExcel);
                      window.clearInterval(viewModel.loadingTimer);
                      $('.file-lodedPart').width(200);
                      viewModel.event.uploadingHide();
                      $("#uploadingIMG2").attr("src","../example/static/success.svg");
                      $("#uploadingMsg2").html("导入成功").addClass("success").removeClass("uploading").removeClass("fail");
                      viewModel.event.initUerList();
                      // md.close();
                  },
                  error: function(XMLHttpRequest, textStatus, errorThrown) {
                      // u.messageDialog({msg:errorThrown,title:'请求错误',btnText:'确定'});
                      $("#uploadingIMG2").attr("src","../example/static/fail.svg");
                      $("#uploadingMsg2").html("导入失败"+data.message).addClass("fail").removeClass("uploading").removeClass("success");
                      // u.messageDialog({msg:"上传失败！"+data.message,title:"提示", btnText:"OK"});
                      //是吧逻辑处理
                      window.clearInterval(viewModel.loadingTimer);
                  }
              });
          },

注：前台获取excel二进制流可以使用任何方式，不限于示例代码 

#### 2.2 后台取得前台获取的excel二进制流进行解析转换成对应的vo
示例代码<br/>
public void importExcelData(InputStream excelStream) throws Exception {
List<Ygdemo_yw_info> list = ExcelExportImportor.loadExcel(excelStream, getHeadInfo(), Ygdemo_yw_info.class);<br/>
//拿到vo可以做自己的业务<br/>
		for (Ygdemo_yw_info entity : list) {
			if (entity.getId() == null) {
				entity.setStatus(VOStatus.NEW);  //新增
			}
			else
			{
				entity.setStatus(VOStatus.UPDATED);  //编辑
			}
			saveEntity(entity);
		}
	}


private static String values = "{'id':'主键','code':'编码','name':'名称','state':'状态','tenant_id':'租户','create_time':'创建时间'}";

private Map<String, String> headInfo;

public Map<String, String> getHeadInfo() {
	if (headInfo == null) {
headInfo = new HashMap<String, String>();
net.sf.json.JSONObject json = net.sf.json.JSONObject.fromObject(values);
headInfo = (Map<String, String>) json;
		}
		return headInfo;
	}

List<Ygdemo_yw_info> list = ExcelExportImportor.loadExcel(excelStream, getHeadInfo(), Ygdemo_yw_info.class);

ExcelExportImportor工具类提供针对excel导入导出的api直接调用即可<br/>
其中接口中参数含义<br/>
{<br/>
excelStream：前台传来的二进制流<br/>
getHeadInfo()：excel表头字段与vo各个字段的对应关系<br/>
Class：vo实体类<br/>
}

