# 介绍 #

kuaidi100 api 调用处理json数据的方法1 —— php调用示例

Kuaidi100 Api involve dealing with  json for php

本示例的内容由快递100技术群：[快乐团](http://www.kuaile-tuan.com/)  友情提供。

# 使用方法 #

对于PHP：用file\_get\_contents获得对方服务器返回的json字符串，赋给前端，前端用JavaScript获得字符串信息，然后eval一下转换成json对象，就可以获得其相应的值了。
示例：
后端：
//查询实时物流状态

```
$url="http://www.kuaidi100.com/api?id=yourId&com=zhongtong&nu=number&show=0&muti=1";
// 获取页面代码
$r = file_get_contents($url);
$this->assign('url',$r);//赋给前端显示，此处根据不同的模板引擎不同
```

前端：
//此处用的为jQuery
```

$(function(){
var url='{$url}';//返回的json字符串
var dataObj=eval("("+url+")");//转换为json对象
var html='<tr class="alt">';
	html+='<th>物流状态：</th>';
	html+='<td class="left">';
	if(dataObj.status==1){
		html+='<table width="520px" cellspacing="0" cellpadding="0" border="0" style="border-collapse: collapse; border-spacing: 0pt;">';
		html+='<tr>';
		html+='<td width="163" style="background-color:#e6f9fa;border:1px solid #75c2ef;font-size:14px;font-weight:bold;height:20px;text-indent:15px;">';
		html+='时间';
		html+='</td>';
		html+='<td width="354" style="background-color:#e6f9fa;border:1px solid #75c2ef;font-size:14px;font-weight:bold;height:20px;text-indent:15px;">';
		html+='地点和跟踪进度';
		html+='</td>';
		html+='</tr>';
		//输出data的子对象变量
		$.each(dataObj.data,function(idx,item){
			html+='<tr>';
			html+='<td width="163" style="border:1px solid #dddddd;font-size: 12px;line-height:22px;padding:3px 5px;">';
			html+=item.time;// 每条数据的时间
			html+='</td>';
			html+='<td width="354" style="border:1px solid #dddddd;font-size: 12px;line-height:22px;padding:3px 5px;">';
			html+=item.context;// 每条数据的状态
			html+='</td>';
			html+='</tr>';
		});
		html+='</table>';
	}else{
		//查询不到
		html+='<span style="color:#f00">Sorry！ '+dataObj.message+'</span>';
	}
	html+='</td></tr>';
	$("#shipping_detail").append(html);

});


```