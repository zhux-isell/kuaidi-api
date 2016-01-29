# HtmlAPI #
**支持包括EMS、邮政、顺丰和申通在内的所有公司**，稳定性强、速度快，易开发、安全性高 。如果有疑问，可以添加企业QQ 800036857转“小佰”咨询

# 1、API请求地址： #
> http://www.kuaidi100.com/applyurl?key=[]&com=[]&nu=[]
（配参数时请先将 中括号去掉）

# 2、传入参数说明： #
| **参数** | **是否必需** | **说明** |
|:-------|:---------|:-------|
| key    | 是        | 快递100的授权key，**如果已有快递100的key，可以直接使用已有的**；如果还没有申请，请点击 [快递查询API](http://www.kuaidi100.com/openapi/api_2_02.shtml)进行申请。|
|  com   | 是        | 快递公司编码见下表|
|  nu    | 是        | 快递单号   |

# 3、支持的公司 #
HtmlAPI支持快递100所有支持的公司，以下只罗列最常见的几个快递，更多的编码请点击[详细列表](http://code.google.com/p/kuaidi-api/wiki/Open_API_API_URL)查看

|**分类**| **快递公司代码** | **公司名称** |
|:-----|:-----------|:---------|
|**E** |            |          |
|      |ems         |ems       |
|      |emsguoji    |ems国际件    |
|**S** |            |          |
|      |shentong    |伸通        |
|      |shunfeng    |顺丰        |
|**Y** |            |          |
|      |youzhengguonei|中国邮政国内包裹/挂号信/国内小包大包|
|      |youzhengguoji|中国邮政国际包裹/挂号信/国外小包大包|


# 4、返回结果说明： #
> 提交请求后，快递100会给您返回一个可以看到结果的url地址，如：http://www.kuaidi100.com/kuaidiresult?id=23  ，您直接访问或用iframe页调用该url（调用方法见后面第四章），即可以看到结果。效果：
> > ![http://kuaidi-api.googlecode.com/files/htmlAPI%E7%BB%93%E6%9E%9C5.jpg](http://kuaidi-api.googlecode.com/files/htmlAPI%E7%BB%93%E6%9E%9C5.jpg)



### 特别提醒： ###

> 因为EMS、顺丰和申通偶尔会不稳定， **不稳定时会先显示验证码** （如下图所示），所以请勿直接将这个页面直接解析成JSON等形式，否则会出错！如下图：

> ![http://kuaidi-api.googlecode.com/files/HtmlAPI%E9%AA%8C%E8%AF%81%E7%A0%812.jpg](http://kuaidi-api.googlecode.com/files/HtmlAPI%E9%AA%8C%E8%AF%81%E7%A0%812.jpg)


# 5、整体使用流程： #
> 第一步，后台创建链接，调用：http://www.kuaidi100.com/applyurl?key=[]&com=[]&nu=[] ，调用后系统会返回一个url地址，如：http://www.kuaidi100.com/kuaidiresult?id=23 。

> 第二步：在要显示结果的页面添加一个iframe标签，将上述结果url地址传入该iframe标签的的src值，即可在该页面查看到结果（如果要实现系统自动地将结果url传入iframe标签的src，请参考下面第五章），iframe代码示范:
```
<iframe name="kuaidi100" src="结果url地址" width="600" height="380" marginwidth="0" marginheight="0" hspace="0" vspace="0" frameborder="0" scrolling="no"></iframe>
```

## 如果要实现系统自动地将结果url传入iframe标签的src，请参考下面 ##



# 6、PHP示范——ems快递查询API接口应用程序开发方法 #
﻿（1）显示结果文件(假如以index.php命名)
将网站所用的快递公司名称的参数设置成shipping\_name,将运单号的参数设置成invoice\_no，在要显示结果的位置插入一个div，以便通过这个div将结果显示出来，代码参考：
```
　　<h5><span>物流跟踪</span></h5>　
　　<divclass="blank"></div>　
　　<tablewidth="100%" border="0" cellpadding="5"cellspacing="1" bgcolor="#dddddd">　
　　<tr> <tdbgcolor="#ffffff"><div id="retData"></div></td> </tr>　
　　<tr><span>快递100提供技术支持</span></tr>　
　　</table>　
```
在显示文件的`</body>`之前加入如下代码，当页面加载后，先向快递100提交请求，然后将请求结果显示到上述div中中去：
```
　　<script language="javascript">　
　　document.getElementById("retData").innerHTML="<center>正在查询物流信息，请稍后...</center>";
　　var expressid =document.getElementById("shipping_name").innerHTML;　
　　var expressno = document.getElementById("invoice_no").innerHTML;　Ajax.call('kuaidi100/express.php?com='+expressid+'&nu=' + expressno,'showtest=showtest', function(data){document.getElementById("retData").innerHTML=data;},'GET', 'TEXT'); //这里的kuaidi100/express.php指请求文件，见下面
　　</script>　
```

（2）查询文件(假如以kuaidi100/express.php命名)

```
//注释：第一步，向http://www.kuaidi100.com/applyurl?key=[]&com=[]&nu=[] 发起请求，代码参考：
　　<?php
　　$typeCom = $_GET["com"];//获得快递公司
　　$typeNu = $_GET["nu"];//获得快递单号
　　if(isset($typeCom)&&isset($typeNu)){
　　$AppKey='XXXXXXX';//请将XXXXXX替换成您在快递100网站申请到的授权key，授权key请到http://www.kuaidi100.com/openapi获得
　　$url= 'http://www.kuaidi100.com/applyurl?key='.$AppKey.'&com='.$typeCom.'&nu='.$typeNu;//生成完整的请求URL

　　//优先使用curl模式发送数据（以下一大段都是在PHP语言下向$url发送请求并获得返回结果的代码，其他语言应该不用这么复杂,ASP的可以参考下这个http://xiao5461.blog.163.com/blog/static/22754562201181881820798/）
　　if(function_exists('curl_init') == 1){
　　$curl = curl_init();
　　curl_setopt ($curl, CURLOPT_URL, $url);
　　curl_setopt ($curl, CURLOPT_HEADER,0);
　　curl_setopt ($curl, CURLOPT_RETURNTRANSFER,1);
　　curl_setopt ($curl,CURLOPT_USERAGENT,$_SERVER['HTTP_USER_AGENT']);
　　curl_setopt ($curl, CURLOPT_TIMEOUT,5);
　　$get_content = curl_exec($curl);
　　curl_close ($curl);
　　}else{
　　include("snoopy.php");//该文件为PHP请求所需要的类文件，请忽略
　　$snoopy = new snoopy();
　　$snoopy->referer ='google';//请将google替换成google的完整网址
　　$snoopy->fetch($url);
　　$get_content = $snoopy->results;
　　}
　　/$get_content=iconv('UTF-8','GB2312//IGNORE', $get_content);
　　//if(strpos($get_content,'地点和跟踪进度')== false){
　　// echo '查询失败，请重试';
　　//}

//注释：第二步：将上面获得的返回结果传入iframe的src值，并将iframe显示出来，代码参考：
　　print_r('<iframe src="'.$get_content.'" width="580"height="380"><br/>' . $powered);
　　}else{
　　echo'查询失败，请重试';
　　}
　　exit();
　　?>
```

# 7、JAVA示范 #
（1）显示结果的页面（indexOne.jsp）
```
<%@page import="com.suyin.dtcc.order.model.OrderBean"%>
<%@page import="com.suyin.dtcc.kuaidi.KuaiDi"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@taglib uri="/struts-tags" prefix="s"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<%
	/**
	* 查看物流
	*/
	//获取快递单号（根据自己的业务逻辑来写）
	OrderBean orderBean = (OrderBean)request.getAttribute("orderBean");
	String kuaidiCode = orderBean.getKuaiDiCode();
	KuaiDi kuaidi = new KuaiDi();
	//调用快递接口 并将kuaidi100接口所生成的url放在request上。
	//searchkuaiDiInfo方法的第一个参数为key值=====kuaidi100申请的唯一的key值，
	//                      第二个参数为快递公司代码，
	//				        第三个参数为快递单号。
	kuaidi.searchkuaiDiInfo("******", "shunfeng", kuaidiCode);
%>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>江苏苏银信息科技有限公司</title>
<script type="text/javascript">
	/**
	* 查看物流
	* 参数wuliuSrc值为：上面调用kuaidi100接口时放入request上的值。
	*/
	function lookWuliu(wuliuSrc)
	{
		//给iframe的src赋值。
		document.getElementById("kuaidi100").src = wuliuSrc;
	}
</script>
</head>
<body class=" mangeApp">
	<s:form name="form1" id="form1" method="post"
		action="Order_doUpdate.action">
			<div class="innerContent">
				<div class="inputCon floatLeft">
					<p>
						<s:if test="orderBean.kuaiDiCode != null">
							<a class="wuliu jbGre2 shadowAll" href="javascript:lookWuliu('${requestScope.wuliuSrc }')">查看物流</a>
						</s:if>
					</p>
					<p>
						<iframe name="kuaidi100" id="kuaidi100" src="" width="600" height="380" marginwidth="0" marginheight="0" 
						hspace="0" vspace="0" frameborder="0" scrolling="no"></iframe>
					</p>
				</div>
			</div>
		</div>
	</s:form>
</body>
</html>
```
（2）查询文件（KuaiDi.java）
```
/*
 * 文件名：KuaiDi.java 版权：Copyright by www.suyinchina.com 描述： 修改人：hxl 修改时间：2013-9-13 跟踪单号： 修改单号： 修改内容：
 */

package com.kuaidi;


import java.io.InputStream;
import java.net.URL;
import java.net.URLConnection;

import org.apache.log4j.Logger;


/**
 * 快递查询
 * 
 * @author hxl
 * @version 2013-9-12
 * @see KuaiDi
 * @since
 */
public class KuaiDi
{
    /**
     * 日志
     */
    private Logger log = Logger.getLogger(KuaiDi.class);

    /**
     * 快递查询接口方法
     * 
     * @param key
     *            ：商家用户key值，在http://www.kuaidi100.com/openapi申请的
     * @param com
     *            ：快递公司代码，在http://www.kuaidi100.com/openapi网上的技术文档里可以查询到
     * @param nu
     *            ：快递单号，请勿带特殊符号，不支持中文（大小写不敏感）
     * @return 快递100返回的url，然后放入页面iframe标签的src即可
     * @see
     */
    public String searchkuaiDiInfo(String key, String com, String nu)
    {
        String content = "";
        try
        {
            URL url = new URL("http://www.kuaidi100.com/applyurl?key=" + key + "&com=" + com
                              + "&nu=" + nu);
            URLConnection con = url.openConnection();
            con.setAllowUserInteraction(false);
            InputStream urlStream = url.openStream();
            byte b[] = new byte[10000];
            int numRead = urlStream.read(b);
            content = new String(b, 0, numRead);
            while (numRead != -1)
            {
                numRead = urlStream.read(b);
                if (numRead != -1)
                {
                    // String newContent = new String(b, 0, numRead);
                    String newContent = new String(b, 0, numRead, "UTF-8");
                    content += newContent;
                }
            }
            urlStream.close();
        }
        catch (Exception e)
        {
            e.printStackTrace();
            log.error("快递查询错误");
        }
        return content;
    }

    public static void main(String[] agrs)
    {
        KuaiDi kuaidi = new KuaiDi();
	//
        String content = kuaidi.searchkuaiDiInfo("快递100申请的key值", "快递公司代码（快递100有文档可以查）", "运单号");
        System.out.println(content);
    }
}

```