# 懒加载-LazyLoad

## 1.懒加载

懒加载，即延迟加载(Lazyload)。简单来说就是一个长页面中需要展示很多图像的时候，如果在进入页面的时候一次性把所有图片加载完，需要很长的时间。为了提升用户体验，我们使用懒加载，当图片出现在浏览器可视区域时，才加载图片。

## 2.图片懒加载

![image](smfuLAzbg59rsWyzeo6BQtd2jEP3kNC4mHR_sTRDZn4.png)

**clientHeight**：浏览器视口的高度；直接通过img.offsetTop就可以获取；

**scrollTop**：滚动轴滚动的距离；通过document.documentElement.scrollTop获取；

**offsetTop**：图片的头部距离浏览器顶部的高度；通过document.documentElement.clientHeight获取；

**Element.getBoundingClientRect( )**：方法返回元素的大小及其相对于视口的位置。我们可以取得它的top值，它的top值就是元素左上角到视口顶部的距离。    

1. 当*Element.getBoundingClientRect().top < 视口高度*时触发加载；
2. 当图片的*offsetTop <= 视口高度clientHeight + 滚动条的长度scrollTop*

初始化时将图片的src保存在自定义属性data-src或者置为空，等到图片需要加载时，为src赋值。

```javascript
<body>
  <!--图片地址暂时先保存在data-src这个自定义属性上面-->
  <img data-src="./img-loop/img/1.jpg" alt="懒加载1" />
  <img data-src="./img-loop/img/2.png" alt="懒加载2" />
  <img data-src="./img-loop/img/3.jpg" alt="懒加载3" />
  <img data-src="./img-loop/img/4.jpg" alt="懒加载4" />
  <img data-src="./img-loop/img/5.jpg" alt="懒加载5" />
</body>

const imgs = document.getElementsByTagName('img');
    function lazyLoad(imgs) {
      // 视口的高度；
      const clientH = document.documentElement.clientHeight;
      // 滚动的距离，这里的逻辑判断是为了做兼容性处理；
      const clientT = document.documentElement.scrollTop || document.body.scrollTop;
      for(let i = 0; i < imgs.length; i++) {
        // 逻辑判断，如果视口高度+滚动距离 > 图片到浏览器顶部的距离就去加载；
        // !imgs[i].src 是避免重复请求，可以把该条件取消试试：可以看到不加该条件的话往回滚动就会重复请求；
        if(clientH + clientT > imgs[i].offsetTop && !imgs[i].src) {
          // 使用data-xx的自定义属性可以通过dom元素的dataset.xx取得；
          imgs[i].src = imgs[i].dataset.src;
        }
      }
    };
    // 一开始能够加载显示在视口中的图片；
    lazyLoad(imgs);
    // 监听滚动事件，加载后面的图片；
    window.onscroll = () => lazyLoad(imgs);//可以加节流函数避免重复执行
```

## 3.路由懒加载

```javascript
路由懒加载 vue-router
1.ES6标准语法import（）---------推荐使用！！！！！
// 将 import UserDetails from './views/UserDetails'
// 替换成
const UserDetails = () => import('./views/UserDetails')
const router = createRouter({
  // ...
  routes: [{ path: '/users/:id', component: UserDetails }],
})
同时component配置也能接受返回一个promise组件的函数，Vue-router第一次进入页面获取该函数后续使用缓存。

2.Vue异步组件
{
      path: '/problem',
      name: 'problem',
      component: resolve => require(['../pages/home/problemList'], resolve)
}
3.webpack提供的require.ensure()
const HelloWorld=resolve=>{
        require.ensure(['@/components/HelloWorld'],()=>{
        resolve(require('@/components/HelloWorld'))
        })
    }
export default new Router({
    routes:[{
    {path:'./',
    name:'HelloWorld',
    component:HelloWorld
    }
    }]
})
```

import与require的区别

1：import 是解构过程并且是编译时执行 

2：require 是赋值过程并且是运行时才执行，也就是异步加载 

3：require的性能相对于import稍低，因为require是在运行时才引入模块并且还赋值给某个变量
