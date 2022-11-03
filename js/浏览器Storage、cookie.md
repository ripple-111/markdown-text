# 浏览器Storage、cookie

# LocalStorage/SessionStorage（本地存储）

## 1.区别

存储在 `localStorage` 的数据可以**长期保留**；

当页面会话结束(页面被关闭时)，存储在 `sessionStorage` 的数据会被清除

## 2.共同点

存储大小为5MB，都保存在客户端，不与服务器进行交互通信，有相同的Web API

## 3.webApi

```javascript
//存储值
1. setItem(key,value)
2. localStorage.key=value
3. localStorage[key]=value
//获取值
1. getItem(key)
2. value=localStorage.key
3. value=localStorage[key]

//如何存储数组对象？
Storage只能存储字符串格式=>利用JSON.stringfy()转换为字符串存储，
取出时使用JSON.parse()
//删除对应键值
removeItem(key) 
//清空
clear()
```

## 4.应用场景

sessionStorage应用场景

进行页面传值

localStorage应用场景

适合长期保存在本地的数据

可以用于存储该浏览器对该页面的访问次数

# cookie（浏览器服务器交流）

## 1.什么是cookie

cookie是当你浏览某个网站的时候，由web服务器存储在你的机器硬盘上的一个小的文本文件。它其中记录了你的用户名、密码、浏览的网页、停留的时间等等信息。当你再次来到这个网站时，web服务器会先看看有没有它上次留下来的cookie。如果有的话，会读取cookie中的内容，来判断使用者，并送出相应的网页内容，比如在页面显示欢迎你的标语，或者让你不用输入ID、密码就直接登录等等。

当客户端要发送http请求时，浏览器会先检查下是否有对应的cookie。有的话，则**自动**地添加在request header中的cookie字段。注意，每一次的http请求时，如果有cookie，浏览器都会**自动**带上cookie发送给服务端。那么把什么数据放到cookie中就很重要了，因为很多数据并不是每次请求都需要发给服务端，毕竟会增加网络开销，浪费带宽。所以对于那设置“每次请求都要携带的信息（最典型的就是身份认证信息）”就特别适合放在cookie中，其他类型的数据就不适合了。

(1) cookie是以小的文本文件形式（即纯文本），完全存在于客户端；cookie保存了**登录的凭证**，有了它，只需要在下次请求时带着cookie发送，就不必再重新输入用户名、密码等重新登录了。

(2) 是设计用来在**服务端**和**客户端**进行**信息传递**的；

## 2.特点

1. 存储数据不能超过4k，*（storage--5M）*
2. 在过期时间前一直有效,（即使窗口或浏览器关闭）
3. 弊端
   1. 每个特定的域名下最多生成**20**个cookie
   2. 安全性问题。如果cookie被人拦截了，那人就可以取得所有的session信息。即使加密也与事无补，因为拦截者并不需要知道cookie的意义，他只要原样转发cookie就可以达到目的了。
   3. 部分不能存储在客户端，（为了防止重复提交表单，我们需要在服务器端保存一个计数器。如果我们把这个计数器保存在客户端，那么它起不到任何作用。）
4. 优点
   1. 极高的扩展性和可用性
   2. 通过良好的编程，控制保存在cookie中的session对象的大小。
   3. 通过加密和安全传输技术（SSL），减少cookie被破解的可能性。
   4. 只在cookie中存放不敏感数据，即使被盗也不会有重大损失。
   5. 控制cookie的生命期，使之不会永远有效。偷盗者很可能拿到一个过期的cookie。

## 3.使用

```javascript
document.cookie //查看cookie，只能获取非 HttpOnly 类型的cookie
//cookie选项包括：expires、domain、path、secure、HttpOnly。    
//设置这些属性时，属性之间由一个分号和一个空格隔开。
"key=name; expires=Sat, 08 Sep 2018 02:26:00 GMT; domain=ppsc.sankuai.com; path=/; secure; HttpOnly"
```

## 4.属性

* expires,max Age: //设置cookie 什么时间内有效 

expires是cookie失效日期，Expires必须是 GMT 格式的时间可以通过 (`newDate().toGMTString()`或者 `new Date().toUTCString() `来获得）。

 如果没有设置该选项，这样的cookie称为会话cookie。它存在内存中，当会话结束，也就是浏览器关闭时，cookie消失。

>  Expires是 http/1.0协议中的选项，在http/1.1协议中Expires已经由 Max age 选项代替，两者的作用都是限制cookie 的有效时间。Expires的值是一个时间点（cookie失效时刻= Expires），而Max age的值是一个以秒为单位时间段（cookie失效时刻= 创建时刻+ Max age）。 另外， Max age的默认值是 -1(即有效期为 session )； Max age有三种可能值：负数、0、正数。负数：有效期session；0：删除cookie；正数：有效期为创建时刻+ Max age

* Domain和Path
  Domain是域名，Path是路径，两者加起来就构成了 URL，Domain和Path一起来限制 cookie 能被哪些 URL 访问。即请求的URL是Domain或其子域、且URL的路径是Path或子路径，则都可以访问该cookie，

> 发生跨域xhr请求时，即使请求URL的域名和路径都满足 cookie 的 Domain和Path，默认情况下cookie也不会自动被添加到请求头部中。

* Size

Cookie的大小

* Secure

Secure选项用来设置cookie只在确保安全的请求中才会发送。当请求是HTTPS或者其他安全协议时，包含 Secure选项的 cookie 才能被发送至服务器。

* httpOnly

这个选项用来设置cookie是否能通过 js 去访问。默认情况下，cookie不会带httpOnly选项(即为空)，所以默认情况下，客户端是可以通过js代码去访问（包括读取、修改、删除等）这个cookie的。**当cookie带httpOnly选项时，客户端则无法通过js代码去访问（包括读取、修改、删除等）这个cookie。**

**只要是httponly类型的,在控制台通过document.cookie是获取不到的，也不能进行修改。**

> 之所以限制客户端去访问cookie，主要还是出于安全的目的。因为如果任何 cookie 都能被客户端通过document.cookie获取，那么假如合法用户的网页受到了XSS攻击，有一段恶意的script脚本插到了网页中，这个script脚本，通过document.cookie读取了用户身份验证相关的 cookie，那么只要原样转发cookie，就可以达到目的了。

## 5.设置

```javascript
document.cookie="name=lynnshen" 
```

## 6.与session区别

cookie是存在客户端浏览器上，session会话存在服务器上。会话对象用来存储特定用户会话所需的属性及配置信息。当用户请求来自应用程序的web页时，如果该用户还没有会话，则服务器将自动创建一个会话对象。当会话过期或被放弃后，服务器将终止该会话。cookie和会话需要配合。

当cookie失效、session过期时，就需要重新登录了。

# 本地存储的使用

## 1.怎么使用storage存储对象

## 以uniapp为例

```javascript
uni.setStorage({ //异步接口
    key:'',//所存储的键名
    value:'' //值
}) //可以加.then或者使用success(){}回调
uni.setStorageSync({}) //同步接口 必须用try{} catch(err){}
uni.getStorage({
    key:'',
    success(){}
    //必须写上success(){}回调
})
```

` //存储对象 uni.setStorage({key:'',value:JSON.stringify()})`

使用JSON.stringify和JSON.parse两个方法转换后存储
