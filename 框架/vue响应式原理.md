# 响应式原理！

## 1.何为响应式？

改变数据的同时，视图同时更新。

### 实现:

react:

`this.setState( )`更新数据，根据新数据渲染新的虚拟dom，根据diff算法找到需要

更新的dom结构，更新对应的节点。

vue:

`Object.defineProperty( )`劫持数据的get，set实现的观察者模式

## 2.观察者模式（`Object.defineProperty()`）

```javascript
<input type="text" id="txt" />
<span id="sp"></span>

<script>
var txt = document.getElementById('txt'),
    sp = document.getElementById('sp'),
    obj = {}

// 给对象obj添加msg属性，并设置setter访问器
Object.defineProperty(obj, 'msg', {
  // 设置 obj.msg  当obj.msg反生改变时set方法将会被调用  
  set: function (newVal) {
    // 当obj.msg被赋值时 同时设置给 input/span
    txt.value = newVal
    sp.innerText = newVal
  }
})

// 监听文本框的改变 当文本框输入内容时 改变obj.msg
txt.addEventListener('keyup', function (event) {
  obj.msg = event.target.value
})
</script>
```

vue给所有的data属性添加object.defineProperty的get,set也即**reactive**化（**响应式**）

## 3.观察者模式：(注册事件+发布事件)

```javascript
function Observer(){
    this.dep=[]
    register(fn){
        this.dep.push(fn)
    }
    notify(){
        this.dep.forEach(item=>item())    
    }
}
const quene =new Observer();
quene.register(()=>console.log('注册第一个')) //注册事件
quene.register(()=>console.log('注册第二个'))

quene.notify() //发布事件
```

## 4.VUE的响应式原理

![](https://gitee.com/rippleber/picgo/raw/master/img/202211021259537.png)

1. init阶段

```typescript
class Dep{ //观察者类
    static target:?watcher
    subs:Array<watcher>  //
    register(){
        if(Dep.target)
        this.subs.push(Dep.target)
    }
    notify(){
        const subs=this.subs.slice()
        subs.forEach(item=>item.update())
    }
}
function defineReactive(obj:Object,key:string,...){
    const dep=new Dep() //观察者对象
    Object.defineProperty(obj,key,{
    enumerable: true, //能被枚举
    configurable: true, //能否通过 delete 删除属性、能否修改属性的特性，
                        //或者将属性修改为访问器属性。
    get(){
    dep.register()
    return value
    }
    set(){
    val=newVal
    dep.notify()
    }
})
}
```

2. mounted阶段（建立Watcher对象作为桥梁）

```typescript
mountComponent(vm: Component, el: ?Element, ...) {
    vm.$el = el
    updateComponent = () => {
      vm._update(vm._render(), ...)
    }
    new Watcher(vm, updateComponent, ...)
    ...
}

class Watcher {
  getter: Function;
  constructor(vm: Component, expOrFn: string | Function, ...) {
    ...
    this.getter = expOrFn //更新函数
    Dep.target = this                      // 注意这里将当前的Watcher赋值给了Dep.target
    this.value = this.getter.call(vm, vm)  // 调用组件的更新函数
    ...
  }
}
```

mounted阶段创建一个watcher对象，将Dep.target设为当前组件的watcher对象，同时调用组件更新函数，重新render，render时访问属性的getter函数即在Dep中注册对应属性get,set事件。**也就是watcher对象作为组件和Dep对象沟通的桥梁**（<mark>依赖收集</mark>）

3. 更新阶段

当Dep注册事件的属性发生变化时，调用Dep.notify()，通知所有的watcher对象调用update进行更新

下路代表总流程

<img title="" src="https://gitee.com/rippleber/picgo/raw/master/img/202211030033840.png" alt="" width="671">

## 5.响应式流程总结

**第一步：** 组件初始化的时候，先给每一个Data属性都注册getter，setter，也就是reactive化。然后再new 一个自己的Watcher对象，此时watcher会立即调用组件的render函数去生成虚拟DOM。在调用render的时候，就会需要用到data的属性值，此时会触发getter函数，将当前的Watcher函数注册进subs里。

**第二步：**当data属性发生改变之后，就会遍历sub里所有的watcher对象，通知它们去重新渲染组件。
