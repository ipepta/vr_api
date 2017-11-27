<div style="
    position: fixed;
    right: 50px;
    bottom: 30px;
    cursor: pointer;
"><a href="#table">返回顶部</a></div>
#wxs服务接口
<h2 id="table">目录</h2>
 1. [点播单管理](#1)
  - [1.1向购物车中添加影片](#1_1)
  - [1.2修改购物车](#1_2)
  - [1.3提交购物车](#1_3)
  - [1.4查询购物车](#1_4)
  - [1.5查询点播单](#1_5)  
  - [1.6撤销点播单](#1_6)
  - [1.7获取支付参数(微信客户端支付时，需要向服务器获取加密参数)](#1_7)
  - [1.8向点播单报告问题](#1_8)
 2. [微信管理](#2)
  - [2.1查询微信用户](#2_1)
  - [2.2查询用户当前sceneid](#2_2)
  - [2.3获取AccessToken](#2_3)
  - [2.4微信回调接口](#2_4)

<h2 id="1">1.点播单管理</h2>

<h3 id="1_1">1.1向购物车中添加影片</h3>

###request

`POST /wxs/cart`

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

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="1_2">1.2修改购物车</h3>

###request

`PUT /wxs/cart`

~~~
{
  "movieID": "fqofnaieujfoiq3jfoaew",
  "openID": "qjo3fjaefjap3jq83f9p8er9fpq4",
  "action": "MOVEUP"
}
~~~

名称| 类型| 描述 |是否必须
----|-----|-----|-----
movieID|string|目标影片id|是
openID|string|微信给用户对本公众号分配的唯一标识，获取方法[参考](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421140842)|是
action|string|MOVEUP, MOVEDOWN, REMOVE 分别对应上移, 下移和删除影片|是

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="1_3">1.3提交购物车</h3>

###request

`POST /wxs/cart/submit`

~~~
{
  "openID": "qjo3fjaefjap3jq83f9p8er9fpq4"
}
~~~

名称| 类型| 描述 |是否必须
----|-----|-----|-----
openID|string|微信给用户对本公众号分配的唯一标识，获取方法[参考](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421140842)|是

###response

成功 200
~~~
{
  "ticketID": "fweiqb3285nxqojoew"
}
~~~

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="1_4">1.4查询购物车</h3>

###request

`GET /wxs/cart?open_id={openID}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
openID|string|微信给用户对本公众号分配的唯一标识|是

###response

成功 200
~~~
{
  "openID": "fweiqb3285nxqojoew",
  "movieList": ["fqp9j384jf83498uf9e", "9j3gfdja9ngf9ejf83", "1ht984nfvq84q39uf9e"]
}
~~~

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="1_5">1.5查询点播单</h3>

###request

`GET /wxs/tickets?open_id={openID}&status={status}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
ticketID|string|点播单ID|否
openID|string|微信用户openid|否
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


`点播单状态(status)枚举：PENDING_PAYMENT(待付款) / ALREADY_PAID(已付款) / OVER_TIME(超出观看时间)/CANCELLED(完成付款前被用户撤销)`

`点播单进度(progressPhase)枚举:PENDING_PAYMENT(待付款) / TO_DISTRIBUTE(待派发) / DISTRIBUTED(已派发) / PLAYING(已执行，播放中) / COMPLETED(播放完成) / TO_TERMINATE(待终止，即观众中途离开，并未点击观看完成) / TERMINATED(已终止，即管理员强行将播放终止)`

<h3 id="1_6">1.6撤销点播单</h3>

`撤销后, 点播单内的电影会回到购物车内, 可修改后再次提交. 而点播单本身不允许修改.`

###request

`DELETE /wxs/tickets?ticket_id={ticketID}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
ticketID|string|点播单ID|是

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="1_7">1.7获取支付参数(微信客户端支付时，需要向服务器获取加密参数)</h3>

###request

`GET /wxs/payment/wx/params?open_id={openID}&ticket_id={ticketID}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
openID|string|微信用户openid|是
ticketID|string|提交支付的点播单ID|是

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


<h3 id="1_8">1.8向点播单报告问题</h3>

###request

`POST /wxs/tickets/complaint`

~~~
{
  "ticketID": "fweiqb3285nxqojoew",
  "openID": "qjo3fjaefjap3jq83f9p8er9fpq4",
  "content": "影片没有播放完就终止了"
}
~~~


名称| 类型| 描述 |是否必须
----|-----|-----|-----
openID|string|微信用户openid|是
ticketID|string|投诉目标|是
content|string|投诉内容|是

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h2 id="2">2.微信管理</h2>

<h3 id="2_1">2.1查询微信用户</h3>

###request

`GET /wxs/users/wx?open_id={openID}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
openID|string|用户id|否
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
		"sceneID": 2000,
		"subscribed": 1
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
subscribed|bool|用户当前是否仍在关注

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="2_2">2.2查询用户当前sceneid</h3>

###request

`GET /wxs/users/sceneid?open_id={openID}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
openID|string|用户id|是

###reponse

成功 200

~~~
{
  "sceneID": 10025,
  "scanTime": "1511457450"
}
~~~

名称| 类型| 描述 
----|-----|-----
sceneID|int|场景ID
scanTime|string|扫描事件微信时间戳(10位)

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="2_3">2.3获取AccessToken</h3>

###request

`GET /wxs/wx/accesstoken`

###reponse

成功 200

~~~
"gzwzM7sD2y0sos7EhoibXawJ1VbUcqesUaUmc8SPJaR6rHvyfIR95GVQxSXtprAD0lFpg7OdhfVhh1"
~~~

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="2_4">2.4微信回调接口</h3>

###request

`GET&POST /wxs/wx/callback`

用于和微信服务器通信, 接收服务器验证握手, 事件推送等.
