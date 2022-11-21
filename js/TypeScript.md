# TypeScript

## 基础类型

### 1. 布尔值(boolean)

`let isDone:boolean=false`

### 2. 数字(number)

> TS里所有数字都是浮点数，浮点数类型为number，除了支持十进制和十六进制，还支持二进制和八进制字面量

```ts
let number:number=6;
let number:number=0xf00d;
let number:number=0b1010;
let number:number=0o744;
```

### 3. 字符串(String)

`let string:string='11'`

### 4. 数组

+ let list:number[]=[1,2,3]（数字数组）

+ let list:Array<number>=[1,2,3] （数组范型）

### 5. 元组(Tuple)

> 已知元素数量和类型的数组

`let x:[string,number]=['1',1]`

访问越界元素，越界元素类型需要满足元祖元素的联合类型

e.g (string | number类型)

`x[3]=true` (报错，boolean不是（string|number）类型)

### 6. 枚举(Enum)

> 对js数据类型的补充，可以为一组数值赋予友好的名字

```ts
enum Color {Red,Green,Blue}
let c:Color=Color.Blue
let c:String=Color[2] // c为green
```

### 7. Any（移除类型检查）

> 当我们为还不清楚类型的变量指定一个类型，这些值可能是动态内容，这种情况下，我们不希望类型检测器对这些值进行检查，直接让他们通过编译阶段的检查，使用any类型

```ts
let notSure:any=4
notSure='1'
notSure=true
```

any类型的值可以被赋值为任意类型（除Never）

object类型有类似功能，但是不能调用它任意的方法，即使存在

```ts
let notSure:any=1
notSure.toFixed()
notSure.doSomeThing()
let notSure:Object=1
notSure.toFiexed() //报错，toFixed不存在
```

### 8. Void

> 与any类型相反，表示没有任何类型，一般用于函数没有返回值

```ts
function do():void{
    console.log(11)
}
```

声明void变量，只能被赋予undefined和null

### 9. Null Undefined

null和undefined是所有类型的子类型，所有可以把null，undefined赋值给任意类型变量

如果指定了 --strictNullChecks标记，null undefined只能赋值给void和它们各自。（使用联合类型，避免问题）

### 10. Never

> 永不存在的值的类型

表示永不存在值的类型，（抛出异常，不会有返回值，死循环）

never是任何类型的子类型，可以赋值给任意类型，但是除了never任何值都不能赋值给never

```ts
function err(message:string):never{
    throw new Error(message)
}
function fail(){
    return error(..)
}
function loop():never{
    while(true){
    
}
}
```

### 11. Object

> 表示非原始类型，除number，string, boolean,symbol,null或undefined之外的类型

### 12. 类型断言

+ 尖括号语法
  
  ```ts
  let some:any='string'
  let strlength:number=(<string>some).length
  ```

+ as语法(jsx)
  
  ```ts
  let some:any='string'
  let strlength:number=(some as string).length
  ```
