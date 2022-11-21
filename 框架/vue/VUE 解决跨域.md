# VUE 解决跨域

# ***更改项目文件后，重启项目!!!***

## 1.跨域

**Access-Control-Allow-Origin--同源策略**

**同源策略**（Same origin policy）是一种约定，它是浏览器最核心也最基本的安全功能，同源是指：域名、协议、端口相同）--解决CRSF问题（cookie）

## 2.proxy解决(脚手架代理）--只能解决开发环境的跨域

```javascript
vue.config.js

module.exports = {
  devServer: {
    // Paths
    proxy: { // 配置跨域
    '/api':{     //告诉webpack遇到什么时使用代理
        target:`http://www.baidu.com/`, //请求后台接口
        changeOrigin:true, // 是否跨域，告诉服务器真实的请求地址
        pathRewrite:{
            '^/api' : '/' // 重写请求
        }
    }
  },
```

```javascript
//调用
this.$http.get('api/getData')    //假设原地址为http://www.baidu.com/getData
此时浏览器发送的地址为http://localhost:8080/api/getData
已使用跨域,此时的接口由devServer转发
```

`proxy`工作原理实质上是利用`http-proxy-middleware` 这个http代理中间件，实现请求转发给其他服务器。例如：本地主机A为`http://localhost:3000`，该主机浏览器发送一个请求，接口为`/api`，这个请求的数据（响应）在另外一台服务器B`http://10.231.133.22:80`上，这时，就可以通过A主机设置webpack proxy，直接将请求发送给B主机。

通过 proxy 实现代理请求后，会在浏览器与服务器之间添加一个代理服务器，本地发送请求时，中间代理服务器接收后转发给目标服务器，目标服务器返回数据，中间代理服务器将数据返回给浏览器。中间代理服务器与目标服务器之间不存在跨域资源问题。

## 3.jsonp解决--只支持get方法

**JSONP是JSON with Padding的略称。它是一个非官方的协议，它允许在服务器端集成Script tags返回至客户端，通过javascript callback的形式实现跨域访问（这仅仅是JSONP简单的实现形式）**

原理：script标签src引入内容不受同源策略影响（img,iframe)

具体实现：客户端定义回调函数传给服务器端，服务器端将所需的参数传入回调函数，客户端即可收到。

缺点：

1. 只支持get请求，如果向后台发送json格式数据，会报415报错(格式不正确）
2. 登录时需判断session判断用户当前的登录状态，跨域导致前后端所取到的session是不一样的。
3. jsonp跨域会导致问题

```javascript
//使用
npm i vue-jsonp
import { VueJsonp } from 'vue-jsonp'
Vue.use(VueJsonp)
this.$jsonp(url,option,callback)
```

```javascript
//客户端
<script type="text/javascript">
   //添加<script>标签的方法 只在有需要的时候创建该script标签发出请求
   function addScriptTag(src){
       var script = document.createElement('script');
       script.setAttribute("type","text/javascript");
       script.src = src;
       document.body.appendChild(script);
   }

   button.onclick = function(){
       //动态创建script
       addScriptTag("http://localhost:8080/api/jsonp?id=1&cb=Callback");

   }
   //自定义的回调函数result
   function Callback(data) {
       //输出数据
       alert(data);
   }
</script>
```

```javascript
//利用koa启动一个node服务器
const Koa = require('koa');
const app = new Koa();
const items = [{ id: 1, title: 'title1' }, { id: 2, title: 'title2' }] //模拟后台数据

app.use(async (ctx, next) => {
  if (ctx.path === '/api/jsonp') {
    const { cb, id } = ctx.query;
    const title = items.find(item => item.id == id)['title'] //查找后台数据
    ctx.body = `${cb}(${JSON.stringify({title})})`; //调用客户端的callback
    return;
  }
})
console.log('listen 8080...')
app.listen(8080);
```

## 4.cors

Cross-Origin Resource Sharing (CORS) 是W3c工作草案，它定义了在跨域访问资源时浏览器和服务器之间如何通信。CORS背后的基本思想是使用自定义的HTTP头部允许浏览器和服务器相互了解对方，从而决定请求或响应成功与否。

cors与jsonp对比

CORS与JSONP相比，更为先进、方便和可靠。

1、 JSONP只能实现GET请求，而CORS支持所有类型的HTTP请求。

2、 使用CORS，开发者可以使用普通的XMLHttpRequest发起请求和获得数据，比起JSONP有更好的错误处理。

3、 JSONP主要被老的浏览器支持，它们往往不支持CORS，而绝大多数现代浏览器都已经支持了CORS。

对一个简单的请求，没有自定义头部，要么使用GET，要么使用POST，它的主体是text/plain,请求用一个名叫Orgin的额外的头部发送。Origin头部包含请求页面的头部（协议，域名，端口），这样服务器可以很容易的决定它是否应该提供响应。

服务器端对于CORS的支持，主要就是通过设置Access-Control-Allow-Origin来进行的。

设置 Access-Control-Allow-Origin 允许跨域
