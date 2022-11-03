# vue-Router (vue2)

# ***Vue2中使用vue-router进行路由跳转***

## 1.安装

```javascript
npm install vue-router@3; yard add vue-router@3 //vue2 
npm i vue-router@4;yard add vue-router@4 //vue3
<script src="https://unpkg.com/vue-router@3"></script>
<script src="https://unpkg.com/vue-router@4"></script>
```

## 2.使用

### 2.1 装载与实例化

```javascript
router/index.js

import vue from 'vue' 
import router from 'vue-router' //引入路由构造函数
Vue.use(router)    //手动安装$route,$router,<router-link></router-link>,使用script引入自动安装
export default new router({ //创建路由实例
    routes:[]
})
```

```javascript
main.js

import router from '../router'
import vue from 'vue'
    new Vue({
    render:h=>h(App),
    router    //挂载根实例,router配置名字不能更改
    }).$mount('#app')
```

## **3.路由传参**

### 3.1 声明式

```javascript
//不带参数
<router-link :to="{name:'home'}"></router-link>    //命名式 (不带'/'即以当前路由为根路径）
<router-link :to="{path:'/home'}"></router-link>    //路径式    （带'/'以根路由为根路径）

//带参数
<router-link :to="{name:'home',params:{id:1}}"></router-link> 
 //1.router中需以/:id配置
 //2.刷新会保留
//html取参 $route.params.id
//script 取参 this.$route.params.id
<router-link :to="{name:'home',query:{id:1}}" /> 或者
<router-link :to="{path:'home',query:{id:1}}" /> 
//路由不用配置
//html 取参 $route.query.id
//script 取参 this.$route.query.id
```

### 3.2编程式

```javascript
//1.不带参数
this.$router.push('/home') //字符串路径
this.$router.push({name:'home'}) 
this.$router.push({path:'/home'})    //描述路径的对象
//2.带参数（query传参和params传参以及两者区别）
query传参（name,path都适用）
//第一种：this.$router.push({name:'home',query:{id:1,age:2}})
//基于name配置路由
{
path: '/hhhhhhh', //这里可以任意
name: 'home', //这里必须是home
component: Home
}
url:http://localhost:8080/#/hhhhhhh?id=1&age=2
//第二种：this.$router.push({path:'/home',query:{id:1,age:2}}) 
//基于path配置路由
{
path: '/home', //这里必须是home
name: 'hhhhhhhh', //这里任意
component: Home
}
url:http://localhost:8080/#/home?id=1234&age=12
//第三种：this.$router.push(`/home?id=1`);
//html取参   $route.query.id 
//script 取参 this.$route.query.id 

//params传参
this.$router.push({name:'home',params:{id:1,age:2}})  //params只能和name一起用哟
//路由配置：
{
    path:'/home/:id/:age',
    name:'W',
    component:W
}
//不配置path，第一次可以请求实现跳转，也能通过this.$router.params.id获取传过来的值，但是刷新页面id会消失  params比query严格
//配置path，刷新页面id会保留
//html 取参 $route.params.id
//script 取参 this.$router.params.id 
//query和params的区别

//query类似get，跳转之后页面url后面会拼接参数，类似?id=1，非重要性可以这样传，密码之类用params
//params类似post，跳转之后页面url后面不会拼接参数，但是刷新页面id会消失
```

---

[webpage](https://v3.router.vuejs.org/zh/guide/#javascript)
