# JSDOM

# \--Nodejs 中DOM

## 1.什么是jsdom？

JSDOM 是在 Node.js 中使用的文档对象模型的纯 Javascript 实现，DOM 对 Node 不可用，但是 JSDOM 是最接近的。它或多或少地模仿了浏览器。

通过编程与要爬取的 Web 应用或网站进行交互，也可以模拟单击按钮。

**（模拟交互）**

## 2.构造函数

```javascript
const dom = new JSDOM(``, {
  url: "https://example.org/",
  referrer: "https://example.com/",
  contentType: "text/html",
  userAgent: "Mellblomenator/9000",
  includeNodeLocations: true
});
//生成的对象是JSDOM类的一个实例，其中包括 window 对象在内的许多有用的属性和方法。
const { window } = new JSDOM(`...`);
const { document } = (new JSDOM(`...`)).window;
```

* `url` 设置的值可以通过`window.location`，`document.URL`和`document.documentURI`来返回，并会影响文档中相关URL的解析以及获取子资源时使用的同源限制和referrer。默认值为`"about:blank"`。
* `referrer` 仅仅影响`document.referrer`的值。默认没有引用（即为空字符串）。
* `contentType` 影响`document.contentType`的值，是按照HTML解析文档还是 XML来解析。它的值如果不是`text/html`或`XML mime type` 值的话将会抛出异常。默认值为`"text/html"`。
* `userAgent` 影响`navigator.userAgent`的值以及请求子资源时发送的`User-Agent`头。默认值为`Mozilla / 5.0（$ {process.platform}）AppleWebKit / 537.36（KHTML，如Gecko）jsdom / $ {jsdomVersion}`。
* includeNodeLocations 保留由HTML解析器生成的位置信息，允许您使用`nodeLocation()`方法（如下所述）检索它。它还能确保在`<script>`元素内运行的代码的异常堆栈跟踪中报告的行号是正确的。
  默认值为`false`以提供最佳性能，并且不能与XML内容类型一起使用，因为我们的XML解析器不支持位置信息。

## 3.使用样例

```javascript
const { JSDOM } = require('jsdom')
const { document } = new JSDOM(
    '<h2 class="title">Hello world</h2>'
).window
const heading = document.querySelector('.title') //模拟Dom的选择器
heading.textContent = 'Hello there!' //类似innerHtml
heading.classList.add('welcome') //改变dom的属性

heading.innerHTML
// <h2 class="title welcome">Hello there!</h2>
```

## 4.触发事件样例

```javascript
const { JSDOM } = require("jsdom")
const axios = require('axios')

const upvoteFirstPost = async () => {
  try {
    const { data } = await axios.get("https://old.reddit.com/r/programming/");
    const dom = new JSDOM(data, {
//除非runScripts设置为"dangerously"，否则事件处理程序属性（如<div onclick =“”>）也将不起作用。（但是，事件处理函数属性，比如div.onclick = ...，将无视runScripts参数 并且会起作用）
//如果您只是试图从“外部”执行脚本，而不是通过<script>元素（和内联事件处理程序）从内部运行“，则可以使用runScripts: "outside-only"选项，该选项会启用window.eval（利用eval执行js代码)
      runScripts: "dangerously", 
//要在页面内启用脚本--在DOM的<script>内部运行的代码如果足够深入，就可以访问Node.js环境，从而访问您的计算机。
      resources: "usable" 
//通过<script src="">来执行外部脚本，需要确保已经加载了它们
    });
    const { document } = dom.window;
    const firstPost = document.querySelector("div > div.midcol > div.arrow");
    firstPost.click();
    const isUpvoted = firstPost.classList.contains("upmod");
    const msg = isUpvoted //判断是否点击成功
      ? "Post has been upvoted successfully!"
      : "The post has not been upvoted!";

    return msg;
  } catch (error) {
    throw error;
  }
};

upvoteFirstPost().then(msg => console.log(msg));
```

## 5.[jsdom 中文文档（纯翻译） - SegmentFault 思否](https://segmentfault.com/a/1190000014844043)
