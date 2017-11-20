<div style="
    position: fixed;
    right: 50px;
    bottom: 30px;
    cursor: pointer;
"><a href="#table">返回顶部</a></div>
#wxs服务接口
<h2 id="table">目录</h2>
 1. [点播单管理](#1)
  - [1.1向点播单中添加影片(前端并无创建点播单操作，服务器会自动创建点播单)](#1_1)
  - [1.2从点播单中删除影片](#1_2)
  - [1.3查询点播单](#1_3)  
  - [1.4获取支付参数(微信客户端支付时，需要向服务器获取加密参数)](#1_4)
  - [1.5修改点播单](#1_5)
  - [1.6向点播单报告问题](#1_6)
 2. [微信用户管理](#2)
  - [2.1查询用户](#2_1)

<h2 id="1">1.点播单管理</h2>

<h3 id="1_1">1.1向点播单中添加影片(前端并无创建点播单操作，服务器会自动创建点播单)</h3>

###request

`POST /wxs/tickets/movies`

~~~
{
  "movieID": "fqofnaieujfoiq3jfoaew",
  "openID": "qjo3fjaefjap3jq83f9p8er9fpq4"
}
~~~

名称| 类型| 描述 |是否必须
----|-----|-----|-----
movieID|string|所添加的影片id|是
openID|string|微信给用户对本公众号分配的唯一标识，获取方法[参考](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421140842)|是

###response

成功 200
~~~
{
  "ticketID": "q03jfp9q38jfp9q38fp9ew"
}
~~~

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="1_2">1.2从点播单中删除影片</h3>

###request

`DELETE /wxs/tickets/{ticket_id}/movies/{movie_id}`

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="1_3">1.3查询点播单</h3>

###request

`GET /wxs/tickets?open_id={open_id}&status={status}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
open_id|string|微信用户openid|是
status|string|点播单状态|否
start|int|分页开始，默认为0|否
limit|int|页面大小，默认为20|否

###response

成功 200
~~~
{
  "totalCount": 30,
  "itemList": [
    {
	    "ticketID": "fqp9j384jf83498uf9e",			
	    "status": "PENDING_PAYMENT",				
	    "progressPhase": "TO_DISTRIBUTE",			
	    "price": 6000,
	    "actualPrice": 5000,
	    "payMode": "WEIXIN",
	    "payTime": 1510994684570,
	    "viewerOpenID": "23ofj9q08ef93q849fq83f",
	    "viewerNickName": "张三",
	    "vrClientID": "fdj3efjs98er9f83fj9e8f9",
		"seatNumber": "001",
	    "cinemaName": "西单vr影院",
	    "hallName": "001厅",	    
	    "movieIDList": ["aw0e9f9839fj93q8f","jf9fej9pq3ep9fq39f","jf9qjf983f9"],
	    "startTime": 1510995105068,
	    "expireTime": 3000
    },{
      ...
    }
  ]
}
~~~

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

名称| 类型| 描述 
----|-----|-----
ticketID|string|点播单id
status|string|点播单状态
progressPhase|string|点播单进度
price|int|价格，单位为分
actualPrice|int|实际价格
payMode|string|支付方式，WEIXIN / ALIPAY
payTime|long|支付时间
viewerOpenID|string|观众微信id
viewerNickName|string|观众微信昵称
vrClientID|string|订单所在座椅id
seatNumber|string|订单所在座椅编号
cinemaName|string|订单所在影院名称
hallName|string|订单所在影厅名称
movieIDList|list|订单包含影片id
startTime|long|订单生效时间
expireTime|int|订单失效时间间隔


`点播单状态(status)枚举：PENDING_PAYMENT(待付款) / ALREADY_PAID(已付款) / OVER_TIME(超出观看时间)`

`点播单进度(progressPhase)枚举:TO_DISTRIBUTE(待派发) / DISTRIBUTED(已派发) / PLAYING(已执行，播放中) / COMPLETED(播放完成) / TO_TERMINATE(待终止，即观众中途离开，并未点击观看完成) / TERMINATED(已终止，即管理员强行将播放终止)`

<h3 id="1_4">1.4获取支付参数(微信客户端支付时，需要向服务器获取加密参数)</h3>

###request

`GET /wxs/tickets/{ticket_id}/weixin_payment/params`

###response

成功 200
~~~
{
  "timestamp": 1510995105068,
  "nonceStr": "qoidsj9ifjq434aodsgjoa",
  "package": "prepay_id=90dskvaiejfjoieg3r",
  "signType": "MD5",
  "paySign": "fq3jefj9afrjao3jfr98ear"
}
~~~

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~


参数说明见[微信公众平台](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)
支付流程见[微信支付开发文档](https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=7_4)

<h3 id="1_5">1.5修改点播单</h3>

###request

`PUT /wxs/tickets/{ticket_id}`

~~~
{
  "movieOrderList": ["fjodifjp98a3j4pf", "jfp3q4j8fjp9dea8", "fjq39pjf8ef2dfdpoh"],
  "progressPhase": "COMPLETED"
}
~~~


名称| 类型| 描述 |是否必须
----|-----|-----|-----
movieOrderList|list|影片播放顺序|否
progressPhase|string|点播单进度|否

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="1_6">1.6向点播单报告问题</h3>

###request

`POST /wxs/tickets/{ticket_id}/complaint`

~~~
{
  "content": "影片没有播放完就终止了"
}
~~~


名称| 类型| 描述 |是否必须
----|-----|-----|-----
content|string|投诉内容|是

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h2 id="2">2.微信用户管理</h2>

<h3 id="2_1">2.1查询用户</h3>

###request

`GET /wxs/wx_users?open_id={open_id}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
open_id|string|用户id|否
start|int|分页开始，默认为0|否
limit|int|页面大小，默认为20|否

###reponse

成功 200

~~~
{
  "totalCount": 30,
  "itemList": [
    {
	    "openID": "fq23jfjp9a8e9q34",
	    "nickName": "张三",
	    "iconURL": "https://weixin.com.cn/ajfw90fa9.jpg",
	    "attentionTime": 1510995105068,
		"sceneID": 2000
    },{
      ...
    }
  ]
}
~~~

名称| 类型| 描述 
----|-----|-----
openID|string|用户id
nickName|string|用户昵称
iconURL|string|用户头像
attentionTime|string|用户关注本公众号的时间
sceneID|int|用户最新扫面的二维码场景值