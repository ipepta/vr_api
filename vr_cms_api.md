<div style="
    position: fixed;
    right: 50px;
    bottom: 30px;
    cursor: pointer;
"><a href="#table">返回顶部</a></div>
#cms服务接口
<h2 id="table">目录</h2>
 1. [影院管理](#1)
  - [1.1创建影院](#1_1)
  - [1.2查询影院](#1_2)
  - [1.3修改影院](#1_3)
  - [1.4删除影院](#1_4)
 2. [放映厅管理](#2)
  - [2.1创建放映厅](#2_1)
  - [2.2查询放映厅](#2_2)
  - [2.3修改放映厅](#2_3)
  - [2.4删除放映厅](#2_4)
 3. [播放终端管理](#3)
  - [3.1创建终端](#3_1)
  - [3.2查询终端](#3_2)
  - [3.3修改终端](#3_3)
  - [3.4删除终端](#3_4)
  - [3.5生成终端二维码](#3_5)
 4. [影片管理](#4)
  - [4.1创建影片](#4_1)
  - [4.2查询影片](#4_2)
  - [4.3修改影片](#4_3)
  - [4.4为影片点赞](#4_4)
  - [4.5删除影片](#4_5)
  - [4.6分发到影院](#4_6)
 5. [点播任务管理](#5)
  - [5.1创建点播任务](#5_1)  
 6. [双端通信websocket(用于coms与cms通信)](#6)
  - [6.1有新影片生成](#6_1)
  - [6.2有新点播任务生成](#6_2)
 7. [cms用户管理](#7)
  - [7.1新增用户](#7_1)
  - [7.2上传用户头像](#7_2)
  - [7.3查询用户](#7_3)
  - [7.4修改用户](#7_4)
  - [7.5删除用户](#7_5)
 8. [获取系统枚举参数](#8)
 9. [头盔型号管理](#9)
 10. [座椅型号管理](#10)
 11. [文件上传](#11)
  - [11.1获取上传policy](#11_1)
  - [11.2文件上传callback](#11_2)

<h2 id="1">1.影院管理</h2>
<h3 id="1_1">1.1创建影院</h3>
###request
`POST /cms/cinemas`

`Content-Type : text/plain`

~~~
{
  "cinemaCode": "huabeiqu_cinema_0001",
  "name": "X立方长宁缤谷",
  "numberOfHalls": 4,
  "regionID": "fqj943f89ad8f9q3hf489ewa",
  "comsMACAddr": "ef:23:7e:cc:8a",  
  "comsIPAddr": "192.168.8.21",
  "description": "第二家店",
}
~~~

名称| 类型| 描述 |是否必须
----|-----|-----|-----
cinemaCode|string|影院的唯一的识别码，可读|是
name|string|影院名称，限制在256字符内|是
numberOfHalls|int|放映厅数量|否
regionID|string|区域id|是
comsMACAddr|string|COMS系统主机的mac地址|否
comsIPAddr|string|COMS主机的本地局域网IP地址|否
description|string|描述比如地址联系方式等，限制在256字符内|否

###response
####接口成功与否，通过http状态码来标识，200代表成功，400代表请求参数错误，500代表服务器错误，以下悉同。

成功 200
~~~
{
  "cinemaID": "fqj39fha8ef9q83fp983q9er"
}
~~~

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="1_2">1.2查询影院</h3>

###request

`GET /cms/cinemas?region_id={region_id}&cinema_id={cinema_id}&cinema_code={cinema_code}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
region_id|string|区域id|否
cinema_id|string|影院id，可多传，`cinema_id={cinema_id1}&cinema_id={cinema_id2}`|否
cinema_code|string|影院编码|否
start|int|分页开始，默认为0|否
limit|int|页面大小，默认为20|否

###response

成功 200
~~~
{
  "totalCount": 100,
  "itemList": [
    {
	  "cinemaID": "fqj39fha8ef9q83fp983q9er",
      "cinemaCode": "huabeiqu_cinema_0001",
      "name": "X立方长宁缤谷",
      "numberOfHalls": 4,
      "regionID": "fqj943f89ad8f9q3hf489ewa",
      "comsMACAddr": "ef:23:7e:cc:8a",
      "comsIPAddr": "192.168.8.21",
      "description": "第二家店"
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

<h3 id="1_3">1.3修改影院</h3>

###request

`PUT /cms/cinemas/{cinemasId}`

`Content-Type : text/plain`

~~~
{  
  "name": "X立方长宁缤谷",
  "numberOfHalls": 4,
  "regionID": "fqj943f89ad8f9q3hf489ewa",
  "comsMACAddr": "ef:23:7e:cc:8a",  
  "comsIPAddr": "192.168.8.21",
  "description": "第二家店",
}
~~~

####字段描述见[1.1创建影院](#1_1)

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="1_4">1.4删除影院</h3>

###request

`DELETE /cms/cinemas?cinema_id={cinema_id1}&cinema_id={cinema_id2}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
cinema_id|string|影院id，可多传，`cinema_id={cinema_id1}&cinema_id={cinema_id2}`|否

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~


<h2 id="2">2.放映厅管理</h2>

<h3 id="2_1">2.1创建放映厅</h3>

###request

`POST /cms/halls`

`Content-Type : text/plain`

~~~
{
  "hallCode": "guanghualu_cinema_0001_hall_01",
  "name": "放映厅01",
  "cinemaID": "qiejgq3984jf9384jf9ef93q8",
  "numberOfSeats": 10
}
~~~

名称| 类型| 描述 |是否必须
----|-----|-----|-----
hallCode|string|放映厅的唯一识别码，可读|是
name|string|放映厅的名称|是
cinemaID|string|放映厅所属影院id|是
numberOfSeats|int|座椅数|是

###response

成功 200
~~~
{
  "hallID": "fqj3fjp9ejfp9w8f983qf9q9p8"
}
~~~

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="2_2">2.2查询放映厅</h3>

###request

`GET /cms/halls?cinema_id={cinema_id}&hall_id={hall_id1}&hall_id={hall_id2}&hall_code={hall_code}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
cinema_id|string|所属影院id|否
hall_id|string|放映厅id，可多传|否
hall_code|string|放映厅编码|否
start|int|分页开始，默认为0|否
limit|int|页面大小，默认为20|否

###response
成功 200
~~~
{
  "totalCount": 100,
  "itemList": [
    {
      "hallID": "fqj3fjp9ejfp9w8f983qf9q9p8"
      "hallCode": "guanghualu_cinema_0001_hall_01",
      "name": "放映厅01",
      "cinemaID": "qiejgq3984jf9384jf9ef93q8",
      "numberOfSeats": 10
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

<h3 id="2_3">2.3修改放映厅</h3>

###request

`PUT /cms/halls/{hall_id}`

`Content-Type : text/plain`

~~~
{
  "hallCode": "guanghualu_cinema_0001_hall_01",
  "name": "放映厅01",
  "cinemaID": "qiejgq3984jf9384jf9ef93q8",
  "numberOfSeats": 10
}
~~~

参数描述见[2.1创建放映厅](#2_1)

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="2_4">2.4删除放映厅</h3>

###request

`DELETE /cms/halls?hall_id={hall_id1}&hall_id={hall_id2}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
hall_id|string|放映厅id，可多传|否

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h2 id="3">3.播放终端管理</h2>

<h3 id="3_1">3.1创建终端</h3>

###request

`POST /cms/vr_clients`

`Content-Type : text/plain`

~~~
{
  "hallID": "fqofjp98jf9p8q3948fq3",
  "seatNumber": "02",
  "hostModel": "windows07",  
  "seatModel": "vr0109",
  "HMDModel": "hmd109",
  "MACAddr": "99:ed:9d:3b:e8:36",
  "IPAddr": "192.168.8.28",
  "description": "描述"
}
~~~

名称| 类型| 描述 |是否必须
----|-----|-----|-----
hallID|string|放映厅id|是
seatNumber|string|座位号|是
hostModel|string|播放终端使用的主机型号|否
seatModel|string|座椅型号|否
HMDModel|string|头盔型号|否
MACAddr|string|座椅主机的mac地址|否
IPAddr|string|座椅主机的ip地址|是
description|string|描述(最长265个字)|否

###response

成功 200
~~~
{
  "clientID": "fqj3fjp9ejfp9w8f983qf9q9p8"
}
~~~

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="3_2">3.2查询终端</h3>

###request

`GET /cms/vr_clients?client_id={client_id}&cinema_id={cinema_id}&hall_id={hall_id}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
client_id|string|客户端id，可多传|否
cinema_id|string|影院id|否
hall_id|string|播放厅id|否
start|int|分页开始，默认为0|否
limit|int|页面大小，默认为20|否

###response

~~~
{
  "totalCount": 100,
  "itemList": [
    {
      "clientID": "fjefijsieuf93q849f8ejf",				//终端id
      "cinemaID": "qoifjp938f983q4udgihue",
      "hallID": "fqofjp98jf9p8q3948fq3",
      "sceneNumber": "10001",						//对应的公众号场景值
      "status": "IDLE",	  
      "seatNumber": "02",
      "hostModel": "windows07",
      "seatModel": "vr0109",
      "HMDModel": "hmd109",
      "MACAddr": "99:ed:9d:3b:e8:36",
      "IPAddr": "192.168.8.28",
      "description": "描述"
    },{
      ...
    }
  ]
}
~~~

`座椅状态(status)枚举：SHUT_DOWN(关机) / IDLE(空闲) / IN_VOD(点播) / PLAYING(播放) / ERROR(错误)`

<h3 id="3_3">3.3修改终端</h3>

###request

`PUT /cms/vr_clients/{client_id}`

`Content-Type : text/plain`

~~~
{  
  "seatNumber": "02",
  "hostModel": "windows07",  
  "status": "SHUT_DOWN",
  "seatModel": "vr0109",
  "HMDModel": "hmd109",
  "MACAddr": "99:ed:9d:3b:e8:36",
  "IPAddr": "192.168.8.28",
  "description": "描述"
}
~~~

字段描述见[3.1创建终端](#3_1)

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~


<h3 id="3_4">3.4删除终端</h3>

###request

`DELETE /cms/vr_clients?client_id={client_id1}&client_id={client_id2}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
client_id|string|客户端id，可多传|否

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="3_5">3.5生成终端二维码</h3>

###request

`GET /cms/vr_clients/{client_id}/qr_code`

###response

成功 200
~~~
二维码图片
~~~

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h2 id="4">4.影片管理</h2>

<h3 id="4_1">4.1创建影片</h3>

###request

`POST /cms/movies`

~~~
{
  "name": "NAME",
  "movieCode": "movieCode",
  "duration": 30,
  "description": "简介",
  "type": "MOVIE",
  "category": "ACTION",
  "country": "USA",
  "fileID": "q3eifj3f98h3w94hf83wffq324s",
  "iconID": "asdfhf09q38fdhuf98q938f98pq",
  "posterID": "g928gj98es9gw45jg98rsgfp9s8u",
  "scheduleStart": "1510208988793",
  "scheduleEnd": "1510208989793",  
  "online": true,
  "weixinURL": "https://mp.weixin.qq.com/s?__biz=MzI5NjQ3OTg1Mw==&mid=2247483989&idx=1&sn=df852bf7447d293b6d6f34d5791a9d80&chksm=ec42ff57db3576413dbacf1288bc6be4d271a7db7aefc093e485c01026b3a645130e3e7b41a2&scene=0#rd",
  "price": 2990,
  "isHot": true
}
~~~

名称| 类型| 描述 |是否必须
----|-----|-----|-----
name|string|电影名称|是
movieCode|string|电影编号|是
duration|int|电影时长，单位是分钟|是
description|string|电影简介|是
type|string|类型枚举，有MOVIE(电影)，GUIDE(前导片)，AD(广告)|是
category|string|电影分类枚举，有ACTION(动作)，SCIENCE(科幻)，等，可多传，用逗号(,)隔开|是
country|string|电影发行国家枚举，有USA(美国)，CHN(中国)等|是
fileID|string|上传影片文件时，oss返回的fileID，请参见[文件上传](#11)|否
iconID|string|上传肖像时，oss返回的fileID，请参见[文件上传](#11)|否
posterID|string|上传海报时，oss返回的fileID，请参见[文件上传](#11)|否
scheduleStart|long|档期开始日期|否
scheduleEnd|long|档期结束日期|否
online|boolean|影片是否上线，默认false|否
weixinURL|string|影片在微信上的简介|否
price|int|价格(分)|是
isHot|boolean|是否热映|否

###response

成功 200
~~~
{
  "movieID": "q03jfp9q38jfp9q38fp9ew"
}
~~~

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="4_2">4.2查询影片</h3>

###request

`GET /cms/movies`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
movie_id|string|电影id，可多传`movie_id={movie_id1}&movie_id={movie_id2}`|
open_id|string|微信用户id|否
cinema_id|string|影院id|否
name|string|电影名称，支持模糊查询|否
movie_code|string|电影编号|否
type|string|电影类型|否
category|string|电影种类|否
country|string|电影发行国家|否
start|int|分页开始，默认为0|否
limit|int|页面大小，默认为20|否
order_param|string|排序字段，默认根据档期开始日期降序排序|否
order_seq|string|排序顺序，默认为降序|否

###response

成功 200

~~~
{
  "totalCount": 100,
  "itemList": [
    {
      "name": "NAME",
      "movieCode": "movieCode",
      "duration": 30,
      "description": "简介",
      "type": "MOVIE",
      "category": "ACTION",
      "country": "USA",      
      "iconURL": "",					//肖像地址
      "posterURL": "",						//海报地址
      "scheduleStart": "1510208988793",
      "scheduleEnd": "1510208989793",
      "numOfPraise": 1000,				//点赞数
      "numOfView": 200,					//被点播数
      "online": true,
      "weixinURL": "https://mp.weixin.qq.com/s?__biz=MzI5NjQ3OTg1Mw==&mid=2247483989&idx=1&sn=df852bf7447d293b6d6f34d5791a9d80&chksm=ec42ff57db3576413dbacf1288bc6be4d271a7db7aefc093e485c01026b3a645130e3e7b41a2&scene=0#rd",
      "price": 2990,
      "isHot": true
    },{
      ...
    }
  ]
}
~~~

参数介绍见[4.1创建影片](#4_1)

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~


<h3 id="4_3">4.3修改影片</h3>

###request

`PUT /cms/movies/{movieID}`

~~~
{
  "name": "NAME",
  "movieCode": "movieCode",
  "duration": 30,
  "description": "简介",
  "type": "MOVIE",
  "category": "ACTION",
  "country": "USA",
  "fileID": "q3eifj3f98h3w94hf83wffq324s",
  "iconID": "asdfhf09q38fdhuf98q938f98pq",
  "posterID": "g928gj98es9gw45jg98rsgfp9s8u",
  "scheduleStart": "1510208988793",
  "scheduleEnd": "1510208989793",  
  "online": true,
  "weixinURL": "https://mp.weixin.qq.com/s?__biz=MzI5NjQ3OTg1Mw==&mid=2247483989&idx=1&sn=df852bf7447d293b6d6f34d5791a9d80&chksm=ec42ff57db3576413dbacf1288bc6be4d271a7db7aefc093e485c01026b3a645130e3e7b41a2&scene=0#rd",
  "price": 3000,
  "isHot": true
}
~~~

参数介绍见[4.1创建影片](#4_1)

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="4_4">4.4为影片点赞</h3>

###request

`POST /cms/movies/{movieID}/_praise`

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="4_5">4.5删除影片</h3>

###request

`DELETE /cms/movies`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
movie_id|string|电影id，可多传`movie_id={movie_id1}&movie_id={movie_id2}`|

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="4_6">4.6分发到影院</h3>

###request

`POST /cms/movies/{movie_id}/_distribute`

~~~
{
  "cinemaIDList":["fqjfj9p384u9f83jfq3948f983", "weori9348g9s8eh4p958jf98"]
}
~~~

名称| 类型| 描述 |是否必须
----|-----|-----|-----
movie_id|string|电影id|是
cinemaIDList|list|影院id列表，是分发到所有影院的id列表|是

###response

成功 200

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~


<h2 id="5">5.点播任务管理</h2>

<h3 id="5_1">5.1创建点播任务</h3>

###request

`POST /cms/vod_tasks`

~~~
{
  "movieList": [{
    "id": "q2pf8je98h3qp9h9q8h"
  },{
    "id": "fajwpoiefieauhgrh8g"
  }]
}
~~~

名称| 类型| 描述 |是否必须
----|-----|-----|-----
movieList[].id|string|影片id|是

###response

成功 200
~~~
{
  "taskID": "ef9843wjfp9ae8grp98q"  
}
~~~

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h2 id="6">6.双端通信长轮训(用于coms与cms通信)</h2>

###request

`/cms/long_poll/{cinemaID}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
cinemaID|string|影院id|是

###response

成功 200
~~~
{
  "messageID": "iejgq9834g9q83hg9898thp9",  //消息id  
  "type": "{TYPE}",          				//消息类型            
  "message": {MESSAGE_OBJECT}				//消息体
}
~~~

<h3 id="6_1">6.1有新影片生成</h3>

###message
~~~
{
  "messageID": "iejgq9834g9q83hg9898thp9",  //消息id  
  "type": "NEW_MOVIE",                      
  "message": {MOVIE_OBJECT}
}
~~~

<h3 id="6_2">6.2有新点播任务生成</h3>

###message
~~~
{
  "messageID": "gjwrpoijeoirjfo3w8989p85",  //消息id  
  "type": "NEW_VOD_TASK",                      
  "message": {VOD_TASK_OBJECT}
}
~~~


<h2 id="7">7.cms用户管理</h2>

<h3 id="7_1">7.1新增用户</h3>

###request

`POST /cms/users`

~~~
{
  "name": "张三",
  "alias": "zhangsan",
  "iconID": "qje0fiadpaij3q4fepf",
  "role": "SYS_ADMIN,MANAGER"
}
~~~

名称| 类型| 描述 |是否必须
----|-----|-----|-----
name|string|用户名称|是
alias|string|用户别名，登陆时使用，只能为英文字母/数字/下划线|是
iconID|string|[7.2上传用户头像](#7_2)接口返回的文件id|否
role|string|用户角色枚举，可传多个，枚举值列表从[获取系统枚举参数](#8)中获取|否

###response

成功 200
~~~
{
  "userID": "aepojfoiawrfiu3whri9898y9"
}
~~~

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="7_2">7.2上传用户头像</h3>

###request

`POST(Multipart request) /cms/files/users/icon`
~~~
{
  "userID": " vaiuyf8qubvawhrgaeg9q7",
  "file": 文件流
}
~~~

名称| 类型| 描述 |是否必须
----|-----|-----|-----
userID|string|用户id|是[如果当前用户已经存在]
file|stream|实体文件|是

###response

成功 200
~~~
{
  "fileID": "q03jfp9q38jfp9q38fp9ew"
}
~~~

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h3 id="7_3">7.3查询用户</h3>

###request

`GET /cms/users`

###response

<h3 id="7_4">7.4修改用户</h3>

###request

`PUT /cms/users/{userID}`

###response

<h3 id="7_5">7.5删除用户</h3>

###request

`DELETE /cms/users/{userID}`

###response

<h2 id="8">8.获取系统枚举参数</h2>

###request
`GET /cms/sys_params/enum`

###response
成功 200
~~~
{
  "movieCountryList": [				//电影发行国
    {
      "id": "CHN",
      "name": "中国",
      "isDefault": true
    },
    {
      "id": "USA",
      "name": "美国"
    }
  ],
  "movieCategoryList": [			//电影分类
    {
      "id": "ACTION",
      "name": "动作",
      "isDefault": true
    },
    {
      "id": "SCIENCE",
      "name": "科幻",
      "isDefault": true
    }  
  ],
  "cinemaRegionList": [				//影院区域
    {
      "id": "BEIJING",
      "name": "北京",
      "isDefault": true
    },
    {
      "id": "SHANGHAI",
      "name": "上海"
    }
  ],
  "userRoleList": [					//用户角色
    {
      "id": "SYS_ADMIN",
      "name": "系统管理员",
      "isDefault": true
    },{
      "id": "MANAGER",
      "name": "管理员"
    }]
}
~~~

失败 400/500
~~~
{
  "info": "失败描述"
}
~~~

<h2 id="9">9.头盔型号管理</h2>

<h2 id="10">10.座椅型号管理</h2>

<h2 id="11">11.文件上传</h2>
<p>本项目使用阿里云oss作为文件存储，前端直接向oss上传文件，上传之前需要向服务器索要上传的policy。上传到oss后，oss会调用服务器的callback接口，然后oss会将callback返回的信息返回给前端。详见[阿里云文档](https://help.aliyun.com/document_detail/31927.html?spm=5176.doc31925.6.633.BVDHBQ)<p>

<h3 id="11_1">11.1获取上传policy</h3>

###request

`GET /cms/upload/oss_policy?domain={domain}`

名称| 类型| 描述 |是否必须
----|-----|-----|-----
domain|string|文件所属的业务范围，CINEMA / HALL / VR_CLIENT / MOVIE / USER|是

###response

成功 200

~~~
{
  "accessid": "6MKOqxGiGU4AUk44",  
  "host": "http://post-test.oss-cn-hangzhou.aliyuncs.com",
  "policy": "eyJleHBpcmF0aW9uIjoiMjAxNS0xMS0wNVQyMDo1MjoyOVoiLCJjdb25kaXRpb25zIjpbWyJjdb250ZW50LWxlbmd0aC1yYW5nZSIsMCwxMDQ4NTc2MDAwXSxbInN0YXJ0cy13aXRoIiwiJGtleSIsInVzZXItZGlyXC8iXV19",
  "signature": "VsxOcOudxDbtNSvz93CLaXPz+4s=",
  "expire": 1446727949,
  "callback": "eyJjYWxsYmFja1VybCI6Imh0dHA6Ly9vc3MtZGVtby5hbGl5dW5jcy5jdb206MjM0NTAiLCJjYWxsYmFja0hvc3QiOiJvc3MtZGVtby5hbGl5dW5jcy5jdb20iLCJjYWxsYmFja0JvZHkiOiJmaWxlbmFtZT0ke29iamVjdH0mc2l6ZT0ke3NpemV9Jm1pbWVUeXBlPSR7bWltZVR5cGV9JmhlaWdodD0ke2ltYWdlSW5mby5oZWlnaHR9JndpZHRoPSR7aW1hZ2VJdbmZvLndpZHRofSIsImNhbGxiYWNrQm9keVR5cGUiOiJhcHBsaWNhdGlvbi94LXd3dy1mb3JtLXVybGVuY29kZWQifQ==",
  "dir": "tmp_upload/movices"
}
~~~

名称| 类型| 描述 
----|-----|-----
accessid|string|对应上传控件的multipart_params.OSSAccessKeyId
host|string|对应上传控件的url参数
policy|string|对应上传控件的multipart_params.policy
signature|string|对应上传控件的multipart_params.signature
expire|long|policy的过期时间
callback|string|对应上传控件的multipart_params.callback，是服务器回掉地址和参数的base64编码后生成的
dir|string|存储路径，加上文件名称生成上传空间的key

<h3 id="11_2">11.2阿里云oss回调</h3>

###request

`POST /cms/upload/oss_callback`

request 信息由[oss官方文档](https://help.aliyun.com/document_detail/31989.html?spm=5176.doc31927.6.882.4QDDuC)指定

###response

该接口的返回信息会通过oss返回给客户端

此接口无论成功与否，都会返回200。成功与否通过isSuccess指定

~~~
{
  "isSuccess": true,								//接口是否成功
  "fileID": "fj39fj09eafrp93q8jf9aj",				//文件id
  "Info": "成功"										//描述
}
~~~