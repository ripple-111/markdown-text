# React基础

## 1. react--helloWorld

```jsx
ReactDom.render(
    <h1>Hello,world</h1>,
    document.getElementById('root')
)
```

## 2. JSX

### 2.1 何为jsx

> jsx=js+xml 是javascript的拓展语法，jsx会转为js使用

`const ele1=<h1>hello</h1> //jsx`    

`const ele2='<h1>hello</h1>' //string`

### 2.2 为什么要用jsx

> react认为渲染逻辑本质上与ui逻辑耦合，例如在ui中需要绑定事件，ui需要根据数据响应式变化展示对应数据,react没有采用**标记与逻辑**进行分离到不同文件的人为分离方式

### 2.3 jsx中嵌入表达式

```jsx
const name='liu'
function getName(n){
    return n 
}
const ele=<h1>hello, {name}</h1>
const ele1=<h1>hello, {getName(name)}</h1>
ReactDom.render(
    ele,document.getElementById('root')
)
```

+ jsx可以在大括号中嵌入任何有效的js表达式，（而不是语句）

+ 编译之后，jsx表达式会转为js函数调用，并取值后得到js对象（可以在if，for语句中使用Jsx，将jsx赋值给变量，当做参数传入，或在函数中直接返回jsx）

+ jsx在引号中嵌入字符串字面量

### 2.4 xss注入

jsx直接使用用户输入内容是安全的--React Dom在渲染输入内容前，默认进行转义，将所有内容转换为字符串

## 2.5 jsx的创建

1. `const ele=(<h1 className='name'> hello </h1>)`

2. `const ele=React和creatElement('h1'，{className:'name'},'hello')`

以上两种方式等效,实际创建的元素

```js
const ele={
    type:'h1'
    props:{
    className:'name',
    children:'hello'
    }
}
```

这个对象被称为**React元素**

## 3. 元素渲染

> 元素描述了你在屏幕中想看到的内容

与浏览器DOM元素不同，react元素是创建开销极小的普通对象，通过react元素更新DOM。

### 3.1 根节点

`<div id='root'> </div>`

一般将其成为根DOM节点，该节点内的所有内容都将由React Dom管理。

***ReactDom.render( )***

用来将react元素渲染到根DOM节点

ReactDom.render(ele,root(dom节点)）

### 3.2 更新已渲染的元素

react元素是不可变对象，一旦被创建就无法修改他的属性和子元素。代表了特定时刻的UI（电影的单帧镜头）

利用setInterval()循环调用ReactDom.render()

### 3.3 更新之后

当react元素更新后，reactdom会将元素和他们的子元素与之前的状态进行比较，并只会进行必要的更新。（不会改变整个dom树）

## 4. 组件

> 类似于js函数接受任意的入参，返回用于描述页面的react元素

### 4.1 函数组件

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

### 4.2 类组件

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### 4.3 渲染组件

react元素不仅可以是jsx转换而来也可以是用户自定义的组件

当react元素为自定义组件的,会将jsx所接受的属性(attributes)

以及子组件(children)转换为单个对象传递给组件（类或函数）

这个对象被称之为**props**

<mark>组件名称首字母必须大写</mark>

react将以小写字母开头的组件视为原生dom标签

### 4.4 props

> react组件必须像纯函数一样保护他们的props不被更改

**只读性**

无论是何种形式的组件，都决不能修改自身的props

纯函数

- 它应始终返回相同的值。不管调用该函数多少次，无论今天、明天还是将来某个时候调用它。
- 自包含（不使用全局变量）。
- 它不应修改程序的状态或引起副作用（修改全局变量）

### 4.5 state

> 组件私有属性

+ 修改通过setState( ),通过赋值改变state属性只能在构造函数使用

+ 不要在setState中（对象）直接依靠state或props的值（异步），传入一个函数两个参数分别为上一次的state和props

+ setState()的修改是浅合并

### 4.6 组件生命周期

+ 挂载（componentDidMount）

+ 卸载（componentWillUnmount） 

+ 更新（shouldComponentUpdate)

### 4.7 单向数据流

state(组件私有数据),props(组件之间传递)

## 5. 事件处理

```js
function test(n){
    return n
}
```

js

```js
<button onclick="test()"></button>
```

jsx

```jsx
<button onClick={test}></button>
```

jsx阻止默认行为不能通过直接返回false

**必须显式的使用e.preventDefault**

### 5.1 class组件中的this

类组件的方法默认不带this

```js
this.handle=this.handle.bind(this)
```

解决

1. classfiled
   
   ```js
   handle=()=>{
       console.log(this)
   }
   ```

2. 回调中使用箭头函数
   
   ```jsx
   <button onClick={()=>this.handle()}></button>
   ```
   
   问题：每次渲染创建不同回调函数，将该回调函数作为props传入子组件，可能进行额外的重新渲染

### 5.2 向事件处理程序传递参数

为事件处理函数传递额外的参数

```jsx
<button onClick={(e)=>this.delete(id,e)}></buton>
<button onClick={this.delete.bind(this,id)}></button>
```

通过箭头函数和function.prototype.bind()实现

## 6. 条件渲染

简单实现

```jsx
function UserGreeting(){
    return <h1>welcome back!</h1>
}
function GuestGreeting(){
    return <h1>please sign up!</h1>
}
```

```jsx
function Greet(props){
    const isLogin=props.isLogin
    if(isLogin)
    return <UserGreeting />
    return <GuestGreeting />
}
ReactDom.render(
    <Greet isLogin={false} />,
    document.getElementById('root')
)
```

+ 定义变量作为判断条件

+ &&,三目运算符

+ render方法返回null可以阻止组件渲染（不会影响组件生命周期）

## 7. 列表渲染

## 7.1 渲染多个组件

{}在jsx中构建一个元素集合

```jsx
const numbers=[1,2,3]
const listItem=numbers.map(i=><li>{li}</li>)
ReactDom.render(
    <ul>{listItem}</ul>,
    document.getElementById('root')
)
```

封装为组件

```jsx
function List(props){
    const number=this.props.number
    const listItem=number.map(i=><li key={i.toString()}>{i}</li>)
    return <ul>{listItem}</ul>
}
function List(props){
    const number=this.props.number
    return <ul>{number.map(i=><li key={i.toString()}>{i}</li>)}</ul>
}
const number=[1,2,3]
ReactDom.render(
    <List number={number} />,
    document.getElementById('root')
)
```

### 7.2 key

key帮助react识别哪些元素改变了（添加删除）

一个元素的key最好是这个元素在这个列表中拥有一个独一无二的字段（不推荐使用数组的index作为key）

key只是在兄弟节点之前必须唯一

## 8. 表单

> 表单元素与普通的dom元素不同，表单元素保持一些内部的state

### 8.1 受控组件

在html中，表单元素之类的表单自己维护state，并根据用户输入的值进行更新

在React中，可变状态通常保存在组件的state属性中，并且只能通过setState()来修改

### 8.2 使用

react表单组件一般都有value值代表表单元素的值，绑定表单元素的onChange()事件,使用event.target.value获取表单元素值,通过setState()更改元素的state值

```jsx
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('提交的名字: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          名字:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="提交" />
      </form>
    );
  }
}
```

多个input使用event.target.name区别不同的元素

受控组件指定value的prop会组织用户更改输入，如果指定了value，输入仍可编辑，可能value意外的被设置为undefined或null

### 9. context

> context 提供了一个无需为每层组件手动添加props，就能在组件树间进行数据传递的方法

Context设计目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前用户，主题。

e.g 主题

```jsx
function App(){
    return <Toolbar theme="dark" />
}
function Toolbar(props){
    return (
    <div>
        <ThemedButton theme={props.theme} />
    </div>
    );
}
function ThemedButton(props){
    return <Button theme={props.theme} />
}
```

使用context

```jsx
const ThemeContext=React.creatContext('light')
function App(){
    return (
    <ThemeContext.Provider value="dark">
    <Toolbar />
    </ThemeContext.Provider>
    )
}
function Toolbar(){
    return <div><ThemeButton /></div>
}
function ThemeButton(){
    static contextType = ThemeContext;
    return <Button theme={this.context}></Button>
}
```

1. 使用React.creatContext(defaultvalue)创建context

2. 使用context.Provider组件将当前的值传递给以下的组件（无论嵌套程度）

3. 中间的组件不必继续向下传递

4. `static contextType = ThemeContext;`为this绑定context,即可使用this.context访问最近的context.Provider

**使用context之前**

有时候需要考虑组件组合（比context更好）

即将组件放在顶层为其绑定好值后直接将组件作为props传递下去

**API**

1. React.createContext()
   
   `const MyContext=React.createContext(defaultValue)`
   
   创建一个context对象，当React渲染了这个Context对象的组件，这个组件会从组件树中离自身最近的那个匹配的Provider中读取到当前的context值
   
   只有组件树中没有匹配到provider时，其defaultValue参数才会有效

2. Context.Provider
   
   `<MyContext.Provider value={ } >`
   
   Context对象都会返回一个Provider React组件，它允许使用的组件订阅context的变化
   
   Provider接受一个value属性，传递给使用组件，一个Provider可以和多个消费组件有对应关系，多个Provider可以嵌套使用，里层会覆盖内层
   
   当Provider的value发生变化，内部所有的使用组件都会重新渲染，Provider及其内部consumer不受制于shouldComponentUpdate函数，因此consumer在祖先组件退出更新也能更新

3. Class.contextType
   
   class上的contextType属性会被赋值为由`React.createContext()`创建的Context对象
   
   可以使用**this.context**访问最近Context对象

4. Context.Consumer
   
   React组件可以订阅到context变更，（函数组件）
   
   需要函数作为子元素，（Function as a child ）,这个函数接受当前的context值，返回一个react节点。传递给函数的value值等同于离该context最近的Provider提供的value,如果没有Provider，value等同于defaultValue

5. Context.displatName
   
   context对象接受一个名为displayName的property，类型为字符串，（React DevTools使用该字符串确定context显示）

## 10. refs转发

> 将ref自动通过组件传递到其某一子组件的技巧

React.forwardRef来获取传递给它的ref，转发给它渲染的D

e.g

```jsx
const FancyButton=React.forwardRef((props,ref)=>{
    <button ref={ref} className="FancyButton">
        {props.children}
    </button>
}
const ref=React.createRef()
<FancyButton ref={ref}></FancyButton>
```

1. 我们通过调用 `React.createRef` 创建了一个 [React ref](https://react.docschina.org/docs/refs-and-the-dom.html) 并将其赋值给 `ref` 变量。
2. 我们通过指定 `ref` 为 JSX 属性，将其向下传递给 `<FancyButton ref={ref}>`。
3. React 传递 `ref` 给 `forwardRef` 内函数 `(props, ref) => ...`，作为其第二个参数。
4. 我们向下转发该 `ref` 参数到 `<button ref={ref}>`，将其指定为 JSX 属性。
5. 当 ref 挂载完成，`ref.current` 将指向 `<button>` DOM 节点。

## 11. Fragments

> 将子元素分组，但不添加额外节点

```jsx
class Table extends React.Component {
  render() {
    return (
      <table>
        <tr>
          <Columns />
        </tr>
      </table>
    );
  }
}
```

```jsx
class Columns extends React.Component {
  render() {
    return (
      <div>
        <td>Hello</td>
        <td>World</td>
      </div>
    );
  }
}
```

```jsx
<table>
  <tr>
    <div>  <!-- 有问题 -->
      <td>Hello</td>
      <td>World</td>
    </div>
  </tr>
</table>
```

```jsx
class Columns extends React.Component {
  render() {
    return (
      <React.Fragment key>        
        <td>Hello</td>
        <td>World</td>
      </React.Fragment>    );
  }
}
```

简单语法

```jsx
class Columns extends React.Component {
  render() {
    return (
      <>       
        <td>Hello</td>
        <td>World</td>
      </>    
    );
  }
}
```

## 12. 高阶组件(HOC)

> 高阶组件是参数为组件，返回值新组件的函数

组件是将props转换为ui，高阶组件是将组件转换为另一个组件

？？

## 13. 深入JSX

React.createElement（component,props,...children）

jsx仅仅是上述代码的语法糖

```jsx
<MyButton color="blue" theme={theme}>
Click
</MyButton>
```

```js
React.createElement(
    MyButton,
    {color:'blue',theme:theme},
    'Click'
)
```

### 13.1 React元素类型

大写字母开头的JSX标签意味着它们是React组件，这些标签会被编译为对变量的引用，JSX`<Foo /> `，Foo必须包含在作用域中

React也必须在JSX作用域中（调用了React.createElement）

### 13.2 属性展开

```jsx
<Button {...props}></Button>
传递整个props对象
或者使用 传递部分props
const {kind,...other}=props
<FancyButton {...other}></FancyButton>
```
