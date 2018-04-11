# 页面结构与交互

前端数据模型的定义示例: 

```
	var ctrlBathPath = ctx+'/goods';
	var app, viewModel, datas;
	var viewModel = {
		md: document.querySelector('#demo-mdlayout'),
		editoradd: '',
		searchText: ko.observable(''),
		searchFileds: ['productName','supplier'],
		mainDataTable: new u.DataTable({  //数据模型定义
			meta: {
				'productid':{
					type:'string'
				},
				'productName': {
					type: 'string'
				},
				'productNum': {
					type: 'integer'
				},
				'price': {
					type: 'float'
				},
				'supplier': {
					type: 'string'
				}, //供应商
				'proDate': {
					type: 'date'
				}, //生成日期
				'orgin': {
					type: 'string'
				} //原产地
			},
			pageSize: 10
		})
```


前后端交互ajax:  

```
            //查询主数据
			queryMain: function(){
				var queryData = {};
				var searchValue = viewModel.searchText();
				
				for (var i = 0; i < viewModel.searchFileds.length; i++) {
					var key = 'search_LIKE_' + viewModel.searchFileds[i];
					queryData[key] = searchValue;
				}
		        
		        queryData["pageIndex"] = viewModel.mainDataTable.pageIndex();
		        queryData["pageSize"] = viewModel.mainDataTable.pageSize();
				$.ajax({ //发送请求到服务器
					type : 'GET',
					url : ctrlBathPath+'/page',
					data : queryData,
					dataType : 'json',
					success : function(result) {
						var data = result.data;
						if(data!=null){
							viewModel.mainDataTable.setSimpleData(data.content);
							viewModel.mainDataTable.totalPages(data.totalPages);
						} else {
							
						}
					}
				});
			}
            
```

前端在入门阶段不作详细说明，在这里只做简单了解就可以，后面我们有专门的前端技术网站支持学习与使用；