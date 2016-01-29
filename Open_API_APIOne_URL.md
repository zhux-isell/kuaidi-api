# APIOne URL于9月底停止服务，请转用快递100的其他API #
> APIOne URL将于2012年9月底停止服务，请各位用户转用快递100提供的其他类型的API，详情请参考：http://www.kuaidi100.com/openapi/index.shtml

# 样式: #

> http://api.kuaidi100.com/apione? com=[.md](.md)&nu=[.md](.md)&show=[0|1|2]
# 说明： #
> 支持多种参数供调用，包括快递公司、快递单号、显示类型等，详见参数说明，不需要申请，方便调用，即可返回最后一条快递记录和对应更多快递信息的查询地址。查询时传入公司代码（暂不支持中文），对应的公司代码如表2，如有添加，请关注本网站的更新公告。

# API用户授权类型： #
> 不需要.


# 参数说明： #
|**名称**|**类型**|**是否必需**|**描述**|
|:-----|:-----|:-------|:-----|
|com   |String|是       |查询公司代码（大小不敏感），不支持中文，目前支持本网站的不带验证码的54家快递公司，对应的公司代码如表2，如有添加，请关注本网站的更新公告。|
|nu    |String |是       |查询快递的单号，请勿带特殊符号，不支持中文|
|show  |String|否       |返回类型。目前本网站支持互联网上的4种通用合适，其中0：表示返回json字符串，1：表示返回xml对象，2：表示返回html对象，3：表示返回text文本。如果不填的话，默认返回json字符串|

## 表2：ApiOne URL 快递公司参数说明 ##
|**快递公司代码**|**公司名称**|
|:---------|:-------|
|aae       |AAE全球专递 |
|anjiekuaidi|安捷快递    |
|anxindakuaixi|安信达快递   |
|baifudongfang|百福东方    |
|biaojikuaidi|彪记快递    |
|bht       |BHT     |
|cces      |希伊艾斯快递  |
|coe       |中国东方（COE）|
|changyuwuliu|长宇物流    |
|datianwuliu|大田物流    | |
|debangwuliu|德邦物流    |
|dpex      |DPEX    |
|dhl       |DHL     |
|dsukuaidi |D速快递    |
|ems       |EMS快递（暂不支持，请转用[Open\_API\_APICode\_URL](Open_API_APICode_URL.md)）|
|fedex     |fedex   |
|feikangda |飞康达物流   |
|fenghuangkuaidi|凤凰快递    |
|gls       |GLS国际快递 |
|ganzhongnengda|港中能达物流  |
|guangdongyouzhengwuliu|广东邮政物流  |
|huitongkuaidi|汇通快运    |
|hengluwuliu|恒路物流    |
|huaxialongwuliu|华夏龙物流   |
|jiayiwuliu|佳怡物流    |
|jinguangsudikuaijian|京广速递    |
|jixianda  |急先达     |
|jiajiwuliu|佳吉物流    |
|jiayunmeiwuliu|加运美     |
|kuaijiesudi|快捷速递    |
|lianhaowuliu|联昊通物流   |
|longbanwuliu|龙邦物流    |
|lanbiaokuaidi|蓝镖快递    |
|minghangkuaidi|民航快递    |
|peisihuoyunkuaidi|配思货运    |
|quanchenkuaidi|全晨快递    |
|quanjitong|全际通物流   |
|quanritongkuaidi|全日通快递   |
|quanyikuaidi|全一快递    |
|santaisudi|三态速递    |
|shentong  |申通快递（暂不支持，请转用[Open\_API\_Chaxun\_URL](Open_API_Chaxun_URL.md)）|
|shunfeng  |顺丰速递（暂不支持，请转用[Open\_API\_APICode\_URL](Open_API_APICode_URL.md)）|
|shenghuiwuliu|盛辉物流    |
|suer      |速尔物流    |
|shengfengwuliu|盛丰物流    |
|shangda   |上大物流    |
|tiandihuayu|天地华宇    |
|tiantian  |天天快递    |
|tnt       |TNT     |
|ups       |UPS     |
|wanjiawuliu|万家物流    |
|wenjiesudi|文捷航空速递  |
|wuyuansudi|伍圆速递    |
|wanxiangwuliu|万象物流    |
|xinbangwuliu|新邦物流    |
|xinfengwuliu|信丰物流    |
|xingchengjibian|星晨急便    |
|xinhongyukuaidi|鑫飞鸿物流快递 |
|yafengsudi|亚风速递    |
|yibangwuliu|一邦速递    |
|youshuwuliu|优速物流    |
|yuanchengwuliu|远成物流    |
|yuantong  |圆通速递    |
|yuanweifeng|源伟丰快递   |
|yuanzhijiecheng|元智捷诚快递  |
|yuefengwuliu|越丰物流    |
|yunda     |韵达快运    |
|yuananda  |源安达     |
|Yuntongkuaidi|运通快递    |
|zhaijisong|宅急送     |
|ztky      |中铁快运    |
|zhongtong |中通速递    |
|zhongyouwuliu|中邮物流    |


# 返回结果： #

> 本返回值通过
http://api.kuaidi100.com/apione?com=tiantian&nu=11111&show=0,
为例说明.【佳吉物流请加 &valicode=[电话号码]】

## Json返回示例： ##
```

{"message":"更多信息，请访问 友商快递100。","status":"1","link":"http://kuaidi100.com/chaxun?com=tiantian&nu=11111","data":[{"time":"2010-08-30 11:10","context":"已签收,签收人是【邮件签收章】"}]}
```

Message：消息体

Data ： 数据集合

Time：每条数据的时间

Context： 每条数据的状态

status： 结果状态（返回0或1。0，表示无查询结果；1，表示查询成功）

link：查询的回调地址

## xml返回示例： ##
```

- <xml>
  <message>更多信息，请访问 友商快递100。</message> 
  <status>1</status> 
  <link>http://kuaidi100.com/chaxun?com=tiantian&nu=11111</link> 
- <data>
  <time>2010-08-30 11:10</time> 
  <context>已签收,签收人是【邮件签收章】</context> 
  </data>
  </xml>

```
Message：消息体

Data ： 数据集合

Time：每条数据的时间

Context： 每条数据的状态

status： 结果状态（返回0或1。0，表示无查询结果；1，表示查询成功）

link：查询的回调地址


## html返回示例： ##

<table cellpadding='0' width='520px' cellspacing='0' border='0'><tr><td width='163'>2010-08-30 11:10</td><td width='354'>已签收,签收人是【邮件签收章】</td></tr><tr><td align='center'><a href='http://kuaidi100.com/chaxun?com=tiantian&nu=11111'>更多信息，请访问 友商快递100</a></td></tr></table>

## text返回示例： ##

```
2010-08-30 11:10 已签收,签收人是【邮件签收章】; 
更多信息，请访问 友商快递100,网址是http://kuaidi100.com/chaxun?com=tiantian&nu=11111
```

# 本文档下载地址： #
[下载地址](http://kuaidi-api.googlecode.com/files/kuaidi100api.zip)

# php 使用案例： #

[php使用案例](http://code.google.com/p/kuaidi-api/wiki/Open_API_Articles_PHP)

# 最后更新：2011-04-08 13:21 #