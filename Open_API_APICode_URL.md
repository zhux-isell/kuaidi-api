# 更新公告 #
APICode URL已经下线，请转用快递100提供的其他API：http://www.kuaidi100.com/openapi/

# 1.APICode URL支持的快递公司及参数说明 #
|**分类**| **快递公司代码** | **公司名称** |
|:-----|:-----------|:---------|
|**E** |            |          |
|      |ems         |EMS       |
|**S** |            |          |
|      |shentong    |伸通        |
|      |shentong    |伸通e物流     |
|      |shunfeng    |顺丰        |
|**Y** |            |          |
|      |youzhengguonei|中国邮政国内包裹/挂号信|
|      |youzhengguoji|中国邮政国际包裹/挂号信|

> ### 其他快递公司请直接使用 [Open\_API\_API\_URL](Open_API_API_URL.md) ###


# 2.利用APICode URL创建快递查询程序的整体步骤 #
### 第一步 ###
> 程序利用 **APICode URL** 获得验证码图片，显示出来。同时，程序获取响应头Response Headers中Set-Cookie的值，将Set-Cookie中的JSESSIONID的值取出。（详见后面《3.APICode URL使用说明》

### 第二步 ###
> 用户输入图片中的验证码，并提交

### 第三步 ###
> 程序构建一个http请求头，将第一步中获得的JESSIONID的值赋给Cookie，然后将快递公司编码、运单号、验证码值等参数传进 **API URL** ，提求请求即可获得结果，最后显示出来（详见后面《4.API URL使用说明》）

# 3.APICode URL使用说明 #
## 3.1 作用 ##
> 部分快递公司在查询时需要输入验证码，可用此API获得验证码图片和cookie等值，为查询运单信息的下一步骤作准备。

## 3.2 API地址 ##
> http://api.kuaidi100.com/verifyCode?id=[]&com=[]

## 3.3 输入参数说明 ##
| **名称** | **类型** | **是否必需** | **描述** |
|:-------|:-------|:---------|:-------|
|id      |String  |是         |身份授权key，请点 [快递查询API](http://www.kuaidi100.com/openapi) 进行申请，此key和API URL的Key是通用的（大小敏感）|
|com     |String  |是         |要查询的快递公司代码，参考《1.APICode URL所支持的快递公司及参数说明》（大小不敏感）|

## 3.4 返回结果说明 ##
> 返回一张验证码图片（图片的数据），如果所调用公司查询无需验证码，则无返回数据。


## 3.5 获取JSESSIONID的方法 ##
> （详见后面《5.2开发示范》，下载包中有详细介绍的文档)
> JESSIONID的值所在的位置如下：
> ![http://kuaidi-api.googlecode.com/files/JESSIONID.jpg](http://kuaidi-api.googlecode.com/files/JESSIONID.jpg)

# 4.API URL使用说明 #
## 4.1 作用 ##
> 查询并获得指定快递公司、快递单号的快递信息。

## 4.2 API地址 ##
> http://api.kuaidi100.com/api?id=[]&com=[]&nu=[]&valicode=[]&show=[0|1|2|3]&muti=[0|1]&order=[desc|asc]

## 4.3 输入参数说明 ##
| **名称** | **类型** | **是否必需** | **描述** |
|:-------|:-------|:---------|:-------|
|id      |String  |是         |身份授权Key，请到  [快递查询API](http://www.kuaidi100.com/openapi) 进行申请（大小敏感）|
|com     |String  |是         |要查询的快递公司代码，不支持中文，需要和APICode URL配合的公司见《1.APICode URL所支持的快递公司及参数说明》，不需要的见 [Open\_API\_API\_URL](Open_API_API_URL.md) 第7章的《API URL 所支持的快递公司及参数说明》 。如果找不到您所需的公司，请发邮件至 kuaidi@kingdee 咨询（大小不敏感）|
|nu      |String  |是         |要查询的快递单号，请勿带特殊符号，不支持中文（大小不敏感）|
|valicode|String  |否         |有两种意义：1、**用户输入的验证码，不在《1.APICode URL所支持的快递公司及参数说明》之列的公司请忽略；**  2、如果是佳吉物流公司，则要用此参数传入电话号码，其他公司请忽略 |
|show    |String  |否         |返回类型。0：返回json字符串，1：返回xml对象，2：返回html对象，3：返回text文本。如果不填，默认返回json字符串|
|muti    |Stirng  |是         |返回信息数量。1:返回多行完整的信息，0:只返回一行信息。不填默认返回多行|
|order   |Stirng  |否         |排序。desc：按时间由新到旧排列，asc：按时间由旧到新排列。不填默认返回倒序（大小不敏感）|


## 4.4 传JESSIONID值的方法 ##
> API使用用户必须保证获取验证码的请求与当次查询请求在同一个会话（session）中进行。具体的保持会话，即传JESSIONID的方法，见后面《5.2开发示范》，下载包中有详细介绍的文档。

  * 错误使用场景

> API用户始终使用同一个会话与快递100API服务通信，会导致当次查询所产生的验证码与查询用户获取到的验证码不一致，导致查询失败，提示验证码错误。

  * 正确使用场景

> API用户为每次用户查询建立一个新的会话并与快递API服务通信，保证获取的验证码是有效的。


## 4.5 返回结果说明 ##
| **字段名称** | **字段含义** |
|:---------|:---------|
|Message   |消息体       |
|Data      |数据集合      |
|Time      |每条数据的时间   |
|Context   |每条数据的状态   |
|state     | **快递单当前的状态** 。0：在途中,1：已发货，2：疑难件，3： **已签收** ，4：已退货。该状态还在不断完善中，有更多的参数需求，欢迎发邮件至 kuaidi@kingdee.com 提出|
|status    |查询的结果状态。0：运单暂无结果，1：查询成功，2：接口出现异常，408：验证码出错（仅适用于APICode url，可忽略) 。遇到其他情况，请按获得身份授权key的邮件中的方法获得技术支持。|


# 5.效果演示、开发示范和插件支持 #
## 5.1 效果演示 ##
> 地址：http://www.shihui.cn/app/order_query.html
> 示例单号：
> SH-SDD-12060515084350、SH-SDD-12060509465242

## 5.2 开发示范 ##
> 目前提供ASP.NET、PHP和Java三个版本的开发示范，包括时序图、关键步骤代码示范和DEMO代码，请点 [快递查询开发示范](http://www.kuaidi100.com/openapi/api_4_02.shtml?typeid=2) 下载使用。

## 5.3 插件支持 ##
> 目前提供最土团购等插件支持，请点 [快递查询插件](http://www.kuaidi100.com/openapi/api_6_02.shtml?typeid=1) 下载使用。如果找不到您所需的插件，可以参考《6.FAQ及开发支持》的方式与我们联系。

# 6.FAQ及技术支持 #
  * FAQ：[快递查询接口(API)常见问题](http://www.kuaidi100.com/help/qa.shtml)

  * 技术支持：
    1. 发邮件至 kuaidi@kingdee.com，
    1. http://blog.kuaidi100.com/?cat=6 论坛]，
    1. QQ、电话等：见获得授权key的邮件。

# 7.附录 #
[本文档下载地址](http://kuaidi-api.googlecode.com/files/APICode%20URL%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E%282012.8.1%E6%9B%B4%E6%96%B0%29.doc)