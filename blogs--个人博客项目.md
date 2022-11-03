# blogs--个人博客项目

# 1.环境搭建

## 1.yarn

安装

```javascript
npm install -g yarn //全局安装yarn
cd ~/path/to/project //进入项目目录
yarn set version berry //设置版本
```

## 2.vite

安装

```javascript
yarn create vite//创建vite项目
cd ..    //进入项目路径
yarn    //初始化项目             
yarn dev //启动项目
```

## 3.element-plus

```javascript
yarn add element-plus //安装element
main.js 引入
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'

import App from './App.vue'

const app=createApp(App)
app.use(ElementPlus)
app.mount('#app')
```

## 4.tailwindcss

```javascript
yarn tailwindcss@latest postcss@latest autoprefixer@latest
npx tailwindcss init -p
```

## 5.sass

# 开发进程

## 2022-09-01--2022-09-23

1. 完成了个人项目导航页面

2. 博客页面整体布局

3. markdown编辑转换的基本实现

4. 登录页面的构思--准备开发后台和项目的需求方向的加
   
   ## 2022-09-24--2022-10-02

5. 登录页面完成

6. 首页完成部分

7. 准备开始登录注册后台

8. 构思自定义功能的实现
