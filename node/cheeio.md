# cheeio

# cheerio -为服务器特别定制的，快速、灵活的jQuery核心实现

## 1.特性，为什么要使用？

* 相似的语法: Cheerio 包括了 jQuery 核心的子集。Cheerio 从jQuery库中去除了所有 DOM不一致性和浏览器尴尬的部分，揭示了它真正优雅的API。
* 闪电般的块: Cheerio 工作在一个非常简单，一致的DOM模型之上。解析，操作，呈送都变得难以置信的高效。基础的端到端的基准测试显示Cheerio 大约比JSDOM快八倍(8x)。
* 灵活性: Cheerio 封装了兼容的htmlparser。Cheerio 几乎能够解析任何的 HTML 和 XML document。

## 2.安装和初始化

```javascript
npm install cheeio
const cheeio = require('cheeio')
//使用load方法加载html标签进行解析
const $=cheeio.load('<html></html>',{可传配置对象}) 

const $=require('cheeio')
$('ul', '<ul id = "fruits">...</ul>'); 
$('li', 'ul', '<ul id = "fruits">...</ul>'); //最后的标签作为root等同于第三行

<ul id="fruits"> //用例的html标签
  <li class="apple">Apple</li>
  <li class="orange">Orange</li>
  <li class="pear">Pear</li>
</ul>
```

## 3.api

### 3.1 \$.(selector,\[context\],\[root\])

\$.(selector,\[context\],\[root\])：

**selector**在**context**的范围内搜索，**context**的范围又包含在**root**的范围内。**selector**和**context**可以是一个字符串，DOM元素，DOM数组或者cheerio实例。**root**一般是一个HTML文档字符串

选择器是文档遍历和操作的起点。如同在jQuery中一样，它是选择元素节点最重要的方法，但是在jQuery中选择器建立在CSS选择器标准库上。cheerio的选择器实现了大部分的方法

### 3.2 attr(name,value)

attr(name,value):

这个方法用来获取和设置属性。获取**第一个**符合匹配的元素的属性值。如果某个属性值被设置成null，那么该属性会被移除。你也可以把**map**和**function**作为参数传递进去，就像在jQuery中一样

```javascript
$('ul').attr('id') //获取html标签中的ul标签id
//=> fruits 
$('.apple').attr('id', 'favorite').html() 
//获取class为apple且id为favorite的标签
//Apple
```

### 3.3 val(\[value\])

val(\[value\])

获得和修改input,select,textarea(表单）的value。

### 3.4 remove(name)

remove(name)

移除名为name的属性

### 3.5 class操作函数

1. hasClass(className)

判断元素是否函数此类名

2. addClass(className)
   为匹配的元素加上此类名

3. removeClass(\[className\])
   从选择的elements里去除一个或多个有空格分开的class。如果className 没有定义，所有的classes将会被去除，也可以传函数。
   
   ### 3.6 is(selector) , is(function(index))
   
   is(selector) , is(function(index)):

有任何元素匹配selector就返回true。如果使用判定函数，判定函数在选中的元素中执行，所以this指向当前的元素。

### 3.7 选择

1. find(selector)

在当前元素集合中选择符合选择器规则的元素集合

2. parent()

获取元素集合第一个元素的父元素

3. next()

选择当前元素的下一个同级元素

4. prev()

选择当前元素的上一个兄弟元素

5. siblings()

获取元素集合中第一个元素的所有兄弟元素，不包含它自己

6. closest(\[selector\])

对于每个集合内的元素，通过测试这个元素和DOM层级关系上的祖先元素，获得第一个匹配的元素。

7. nextAll()

获得本元素之后的所有同级元素

8. preAll()

获得本元素前的所有同级元素

9. slice(start,\[end\])

获得选定范围内的元素

10. children(selector)

获被选择元素的子元素

11. first()

会选择chreeio对象的第一个元素

12. last()

会选择chreeio对象的最后一个元素

13. eq(i)

通过索引筛选匹配的元素。使用.eq(-i)就从最后一个元素向前数。

### 3.8 遍历

1. each( function(index, element) )

遍历函数返回false即可终止遍历

2. map(function(index, element))

迭代一个cheerio对象，为每个匹配元素执行一个函数。Map会返回一个迭代结果的数组。

3. filter(selector), filter(function(index))

迭代一个cheerio对象，滤出匹配选择器或者是传进去的函数的元素。如果使用函数方法，这个函数在被选择的元素中执行，所以this指向当前元素。

### 3.9 改变dom

append( content, \[content, ...\] ) //最后插入

prepend( content, \[content, ...\] )

after( content, \[content, ...\] )  //之后插入

before( content, \[content, ...\] )

remove( \[selector\] ) //从DOM中去除匹配的元素和它们的子元素。选择器用来筛选要删除的元素。

replaceWith( content )

empty() //清空一个元素，移除所有的子元素

### 3.10 获取

1. html( \[htmlString\] )

获得元素的HTML字符串。如果htmlString有内容的话，将会替代原来的HTML。

2. text( \[textString\] )

获得元素的text内容，包括子元素。如果textString被指定的话，每个元素的text内容都会被替换。

3. toArray()

取得所有的在DOM元素，转化为数组。

4. clone()

克隆cheerio对象

### 3.11  \$

1. \$.root

有时候你想找到最上层的root元素,那么\$.root()就能获得

2. \$.contains( container, contained )

查看contained元素是否是container元素的子元素

3. \$.parseHTML( data \[ context \] \[ keepScripts \] )

将字符串解析为DOM节点数组。context参数对chreeio没有意义，但是用来维护APi的兼容性。
