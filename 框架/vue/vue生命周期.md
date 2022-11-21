# Vue生命周期

> 生命周期（life circle）：表示vue实例从创建到销毁的全过程就是生命周期

> 创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、卸载

## 1.生命周期

八阶段

### 1.1 beforeCreate(创建之前)

在内存中初始化vue实例，此时还没有data和methods

`new Vue()`->`init()`

### 1.2 created(创建)

+ 设置完毕响应式数据（数据监测）、计算属性、方法和侦听器

+ 可以调用methods方法，访问修改data数据触发响应式属性修改渲染dom，可以computed、watch监测数据变化

+ 此时vm.$el还没有挂载，但是vm已经创建

### 1.3 beforeMount(挂载之前)

判断此时存在el选项，不存在则停止编译，直到调用vm.$mount(el)才会继续编译

+ 优先级 render>template>outerHtml

此时vm.el完成dom初始化，但是还没有挂载在el上

### 1.4 mounted(挂载)

此时vm.el完成挂载，vm.$el替换为el对应的dom元素

### 1.5 beforeUpdate(更新之前)

- 更新的数据必须是被渲染在模板上的（`el`、`template`、`render`之一）
- 此时`view`层还未更新（数据仍未旧数据）
- 若在`beforeUpdate`中再次修改数据，不会再次触发更新方法

### 1.6 updated(更新)

- 完成`view`层的更新
- 若在`updated`中再次修改数据，会再次触发更新方法（`beforeUpdate`、`updated`）

### 1.7 beforeDestory(销毁之前)

- 实例被销毁前调用，此时实例属性与方法仍可访问

### 1.8 destoryed(销毁之后)

- 完全销毁一个实例。可清理它与其它实例的连接，解绑它的全部指令及事件监听器
- 并不能清除DOM，仅仅销毁实例

### 1.9 activated(keep-alive组件激活时)

### 1.10 deactivated(keep-alive组件停用)

### 1.11 errorCaptured(捕获错误)

来源

- 组件渲染
- 事件处理器
- 生命周期钩子
- `setup()` 函数
- 侦听器
- 自定义指令钩子
- 过渡钩子

![](https://img-blog.csdnimg.cn/2021032421314699.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1MjIzMTU5NTc0Mg==,size_16,color_FFFFFF,t_70)

## 2.使用场景

| 生命周期          | 描述                             |
| ------------- | ------------------------------ |
| beforeCreate  | 执行时组件实例还未创建，通常用于插件开发中执行一些初始化任务 |
| created       | 组件初始化完毕，各种数据可以使用，常用于异步数据获取     |
| beforeMount   | 未执行渲染、更新，dom未创建                |
| mounted       | 初始化结束，dom已创建，可用于获取访问数据和dom元素   |
| beforeUpdate  | 更新前，可用于获取更新前各种状态               |
| updated       | 更新后，所有状态已是最新                   |
| beforeDestroy | 销毁前，可用于一些定时器或订阅的取消             |
| destroyed     | 组件已销毁，作用同上                     |
