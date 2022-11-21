# vue进阶

## 1.vue中v-if和v-for优先级

vue2：v-for优先级大于v-if

**解决**:

+ 条件在外面：在外面元素套用`<template></template>`

+ 条件在元素内部：使用计算属性提前过滤数据

vue3：相反

## 2.SPA单页应用首页加载优化

> 原因,spa应用需要第一次加载时导入大部分文件

- 网络延时问题
- 资源文件体积是否过大
- 资源是否重复发送请求去加载了
- 加载脚本的时候，渲染内容堵塞了

**解决方案：**

1. 路由懒加载--减少入口文件体积
   
   动态加载路由`component:()=>import('...xxx.vue')`,不同路由对应组件进行分割，路由被请求时会单独打包路由，减少入口文件体积

2. 静态资源缓存
   
   后端资源：
   
   + http缓存，设置`Cache-Control`，`Last-Modified`，`Etag`等响应头
   
   + service-worker离线缓存（代理服务器)
   
   前端资源：
   
   利用Localstorage存储数据

3. ui按需引入

4. 组件重复打包
   
   `A.js`文件是一个常用的库，现在有多个路由使用了`A.js`文件，这就造成了重复下载解决方案：在`webpack`的`config`文件中，修改`CommonsChunkPlugin`的配置`minChunks`为3表示会把使用3次及以上的包抽离出来，放进公共依赖文件，避免了重复加载组件

5. 图片压缩

6. 使用ssr技术

<img src="https://static.vue-js.com/4fafe900-3acc-11eb-85f6-6fac77c0c9b3.png" title="" alt="" width="696">
