# THE STAR

> 个人博客项目

## 1.需求分析

用户模块

+ 登录注册

+ 个人信息修改（包括基本信息和密码等信息）

博客模块

+ 清晰的博客显示页面
  
  + 增、删、改（在线修改）、查

+ 在线编辑功能

+ 批量文章导入导出

+ 分类、标签

管理模块

+ 博客的管理功能
  
  + 增、删、改、查

+ 评论管理
  
  + 删

+ 数据可视化
  
  + 访客数据
  
  + 文章数据

评论模块

+ 用户留言

+ 博客评论

*拓展功能*

+ 社区功能

+ 关注功能

+ 个人定制

## 2.技术选型

`electron+vue+typeScript+koa2+mysql+ipfs`

**前端**

electron 桌面端开发框架结合typescript开发页面

使用md-editor-v3作为markdown解析组件

tailwindcss、sass编写样式

**后端**

koa2 restful接口+sequelize+mysql+ipfs-client存储数据

库

+ [md-editor-v3]([imzbf/md-editor-v3: Markdown editor for vue3, developed in jsx and typescript, dark theme、beautify content by prettier、render articles directly、paste or clip the picture and upload it... (github.com)](https://github.com/imzbf/md-editor-v3))
