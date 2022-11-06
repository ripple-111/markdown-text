# Node-js 网络爬虫

## 1.环境安装

### 1.1 request库

 const request=require('request')

#### 使用方式

```javascript
// 方式一 

request.get(url,options,callback) 

// 方式二 

let options = { url } //url 必填

request.get(options,callback) 

// 方式一 

request.post(url,options,callback) 

// 方式二 

let options = { url } 

request.post(options,callback)
```

#### 源码

```javascript
function request (url, options, callback) {

  if (typeof url === 'undefined') {

   throw new Error('undefined is not a valid uri or options object.')

  }

  var params = initParams(url, options, callback)

  if (params.method === 'HEAD' && paramsHaveRequestBody(params)) {

   throw new Error('HTTP HEAD requests MUST NOT include a request body.')

  }

  return new request.Request(params)

}

function initParams (url, options, callback) {

 // 处理没有传 options 的情况

  if (typeof options === 'function') {

    callback = options

  }

  var params = {}

  if (typeof options === 'object') {

   extend(params, options, {url: url}) //合并 后者相同覆盖前者

   // 传递的 url 最终也会被合并到 pramas 上

   // 并且如果你在 options 传递了 url 会被第一参数覆盖，优先级以 第一个入参url为准

  } else if (typeof url=== 'string') {

   extend(params, {url: url})

  } else {

   // 处理第一参数不是url的情况

   extend(params, url)

  }

  params.callback = callback || params.callback

  return params

}

可以在option中传 baseUrl:'',统一设置公共域名和公共部分

####
```

#### 格式

application/x-www-form-urlencoded

通过form字段来接收入参，方法如下，直接将传入的参数对象传递给 form 即可。

```javascript
function fetchPost(path,params){ 

return new Promise( (resolve,reject)=>{ 

request.post(path,{ form:params }, //利用form字段传参

function(err, httpResponse, body){ 

if(err){ reject(err) }

else

{ resolve(body) } 

}) 

}) 

}
```

#### form-data 文件上传

通过 formData 来传递文件，代码如下：使用 fs.createReadStream 去拿到中间层的文件，然后通过 formData 方式发送给后端。

后端接收到到 content-type 为 multipart/form-data ， 我们并没有手动的去设置请求的 content-type 会自动添加上。

```javascript
function fetchPost(path,params){

  return new Promise( (resolve,reject)=>{

   let formData = {

      file:fs.createReadStream(__dirname+'/../static/images/icon-arrow.png')

   }

    request.post(path,{
      formData //简写形式 formData:formData
   },

function(err, httpResponse, body){ //错误回调函数
     if(err){
       reject(err)
     }else{
       resolve(body)
     }
   })

  })

}
```

#### application/json

将参数通过 body 传递，并且设置 json为ture，那么请求时会自动将 content-type 设置为 application/json 并且将传递给 body 的对象转义为 JSON

```javascript
function fetchPost(path,params){
  return new Promise( (resolve,reject)=>{
    request.post(path,{
      baseUrl:"http://localhost:9000/react/",
      body:params, //body传输
      json:true //json格式
    },
function(err, httpResponse, body){
      if(err){
        reject(err)
      }else{
        resolve(body)
      }
    })
  })
}
```

#### head

```javascript
request.post(path,{

  form:params,

  headers:{

   // 'content-type':'application/json',

   // ... 任意其他字段

    name:'dd',

    agent:'request'

  }

})
```

### 1.2 cheeio

#### （1） 什么是cheeio

Cheerio 是一个在 Node.js 中解析 HTML 和 XML 的工具

❤ 熟悉的语法：Cheerio实现了核心jQuery的一个子集。Cheerio从jQuery库中删除了所有DOM不一致和浏览器的垃圾，揭示了其真正华丽的API。

ϟ 极快：Cheerio使用一个非常简单，一致的DOM模型。因此，解析、操作和渲染非常高效。

❁ 令人难以置信的灵活性：Cheerio环绕着[parse5](https://github.com/inikulin/parse5)解析器，可以选择使用

#### cheeio

## 2.代码实现

### 2.1 http + cheerio

```javascript
const request= require('http')

const cheerio=require('cheerio')

const fs=require('fs')

let url='http://news.baidu.com/'

var strHtml=''

var result=[]

request.get(url,(res)=>{ //请求获取目标网站

    res.on('data',(html)=>{

        strHtml+=html

   })

    res.on('end',()=>{

       let $=cheerio.load(strHtml)

       $('#channel-all li').each((item,i)=>{

            console.log($(i).text())

       })

   })

   })
```

### 2.2 request +cheerio

```javascript
const request= require('http')

const cheerio=require('cheerio')

const fs=require('fs')

const iconv = require('iconv-lite');

let url='http://news.baidu.com/'

var strHtml=''

var result=[]

request.get(url,(err,res,body)=>{

    strHtml+=html

    strHtml=iconv.decode(strHtml,'gbk') //利用iconv-lite库进行解码

   let $=cheerio.load(strHtml)

   $('#channel-all li').each((item,i)=>{

    console.log($(i).text())

       })

   })

//爬取图片

let url='https://cn.bing.com/images/trending?FORM=ILPTRD'

var strHtml=''

request.get(url,(err,res,body)=>{

    strHtml+=body

   let $=cheerio.load(strHtml)

   $('img').each((item,i)=>{

            console.log($(i).attr('src'))

     })

   })

##
```

3.运行结果

![](https://kshttps0.wiz.cn/editor/e02d7c90-f222-11ec-8921-95b22f0c84ba/201a161e-9bf7-4731-9275-5122e04ba28e/resources/ywKBeG3K7ZZJuw2Ucva9dgDSV7_R3w43q3EH6x8c-XE.png?token=W.GQJRIZFMRIjkwUR0ZUC_IhVlzh0Ore9LJJuTGUQp6c5jn_V5UjEmchQaa686OM4)

![](https://kshttps0.wiz.cn/editor/e02d7c90-f222-11ec-8921-95b22f0c84ba/201a161e-9bf7-4731-9275-5122e04ba28e/resources/9AjWH5q1gzwK9zsMKXNoVoRWxX9X-7RNp3R8sZQLPCM.png?token=W.GQJRIZFMRIjkwUR0ZUC_IhVlzh0Ore9LJJuTGUQp6c5jn_V5UjEmchQaa686OM4)

JSDOM--模拟浏览器DOM
