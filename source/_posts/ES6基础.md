---
title: ES6基础
---

## ECMAScript 6 简介

- Javascript 三大组成部分
  - ECMAScript
  - DOM 文档对象模型
  - BOM 浏览器对象模型

## ECMAScript 6

### 1.1 let 和 const

let 变量
```javascript
var：
  1. var可以重复声明
  2. 作用域：全局作用域和函数作用域
  3. 会进行预解析（变量提升）

let
  1. 统一作用域下不能重复声明
  2. 作用域：全局作用域和块级作用域{}
  3. 不进行预解析
```

const 常量

### 1.2 解构赋值

对象解构赋值

```javascript
const obj = { a: 1, b: 2 }
const { a, b } = obj
```

数组解构赋值

```javascript
const arr = ["a", "b", "c"]
const [e, f] = arr
```

字符串解构赋值

```javascript
const str = "abc"
const [e, f] = str
console.log(e, f) // a, b
```

### 1.3 展开运算符

```javascript
const arr = [1, 2, 3, 4]
const arr2 = ["a", "b", ...arr, "c", "d"]
console.log(arr2) // ["a", "b", 1, 2, 3, 4, "c", "d"]

// 剩余参数
const [a, b, ...c] = arr;
console.log(a, b, c) // 1, 2, [3, 4]

// 对象展开
const obj = { a: 1, b: 2 }
const obj2 = { ...obj, c: 3, d: 4 }
console.log(obj2)

// 问题
const obj3 = obj // 传址，会改变原来对象的属性值
obj3.a = 10
console.log(obj) // { a: 10, b: 2 }

// 解决
const obj3 = {...obj}
obj3.a = 10
console.log(obj) // { a: 1, b: 2 }
```

### 1.4 Set对象

```javascript
// 构造函数 用来构建某一类型的对象 - 对象的实例化

let arr = [1,2,3,4,5,5,5]
let s = new Set(arr)
console.log(s) 
arr = [...s]
console.log(arr) // [1,2,3,4,5] 数组去重

console.log(s.size) // size 数值的个数 => length

s.clear() // 清空所有值

s.delete(4) // 删除掉某个值

s.add(6) // 添加

s.has() // 查看是否包含某个值 返回 true or false
```

### 1.5 Map对象

```javascript
let arr = [
  ["a", 1]
  ["b", 2],
  ["c: 3"]
]

let m = new Map(arr)
console.log(m)

m.clear()
console.log(m)

delete(key) // 删除某一项，返回true or false 删除成功或不成功

get(key) // 获取某一项具体值

has(key) // 是否包含某一项

set(key, value) // 设置一个值, set("d", 4)
```

### 1.6 函数新增扩展

```javascript
// 箭头函数，没有不定参
{
  let fn = () => {
    console.log(arguments) // arguments is not defined 
  }
  fn(1,2,3)

  // rest参数
  let fn = (...arg) => {
    console.log(arg) // 1,2,3
  }

  fn(1,2,3)
}

// 箭头函数的this
{
  document.onclick = function(){
    console.log(this) // document
  }

  document.onclick = () => {
    console.log(this) // window
  }

  document.onclick = function(){
    let fn = () => {
      console.log(this) // document
    }
    function fn(){
      consoel.log(this) // window
    }
    fn()
  }
}

// 箭头函数本身没有this, 调用箭头函数的this时， 指向是其声明时所在的作用域的this
{
  let fn;
  let fn2 = function(){
    console.log(this) // window
    fn = () => {
      console.log(this) // window
    }
  }
  fn2 = fn2.bind(document.body)
  fn2()
  fn()
}

// 参数默认值
let fn = (a = 1, b = 2) => {
  console.log(a * b)
}
```

### 1.7 数组新增扩展

```javascript
// Array.form 类数组转成数组
{
  let lis = document.querySelectorAll("li")
  let arr = []
  lis = Array.from(lis, (item, index) => {
    console.log(item, index, this)
    return index;
  }, arr)
  console.log(lis)
}

// Array.of(1,2,3,4,'a') 将参数转成一个数组

// Array.isArray(data) 检测是否是数组
```

### 1.8 find和findIndex
```javascript
// find 查找数组中满足要求的第一个元素的值，参数和forEach一致
{
  let arr = [1, 2, 3, 4]
  let val = arr.find((item, index) => {
    if(item > 3){
      return true
    }
  })
  console.log(val)

  // 简写
  let val = arr.find(item => item > 3)
}

// findIndex 返回索引
{
  let arr = [1, 2, 3, 4]
  let index = arr.findIndex(item => item > 3)
}
```

### 1.9 数组扁平化
```javascript
// flat 扁平化多维数组
{
  let arr = [
    ["小明", 18],
    ["小刚", 18],
    [
      [1, 
          [3, 4],
          [5, 6]
          [
            [7],
            [8]
          ]
      ]
    ]
  ]
  console.log(arr.flat(3))
  console.log(arr.flat(Infinity))
}

// flatMap

// fill 数组填充
{
  let arr = [0, 1, 2, 3, 4]
  arr.fill("a", 4)
  console.log(arr) // [0, 1, 2, 3, 4, 'a']
  arr.fill("a", 1)
  console.log(arr) // [0, 'a', 'a', 'a', 'a']
  arr.fill("a", 1, 3)
  console.log(arr) // [0, 'a', 'a', 3, 4]
}

// includes   indexOf
{
  let arr = ["a", "b", "c"]
  console.log(arr.includes("b")) // true
  console.log(arr.includes("b", 2)) // false
}
```

### 2.0 字符串扩展方法
```javascript
// includes

// startsWith
{
  let str = "hello world"
  console.log(str.startsWith('hello')) // true
  console.log(str.startsWith('hello', 4)) // false
}

// endsWith 同上

// repeat
{
  let str = "a";
  console.log(str.repeat(2)) // aa
}

// 模板字符串
```

### 2.1 对象新增基础
```javascript
// 简洁表示法
{
  let a = 0
  let b = 1
  let obj = {
    a,
    b,
    c(){
      console.log("a")
    }
  }
}

// 属性名表达式
{
  let name = "小明";
  let obj = {
    [name]: 111
  }
  console.log(obj) // {小明: 111}
}

// 对象合并
{
  let obj = {
    a: 1,
    b: 2
  }

  let obj2 = {
    ...obj,
    c: 3,
    d: 4
  }

  console.log(obj2)
}

{
  let obj = {
    a: 1,
    b: 2
  }
  let obj2 = {
    c: 3,
    d: 4
  }
  Object.assign(obj2, obj) // obj合并到obj2
  console.log(obj2) // { c: 3, d: 4, a: 1, b: 2 }

  obj2 = Object.assign({}, obj, obj2)
  console.log(obj2) // { a: 1, b: 2, c: 3, d: 4 }
}

// Object.is(value1, value2) 判断两个值是否相等 -> ===
{
  console.log(+0 === -0) // true
  console.log(Object.is(+0, -0)) // false

  console.log(Object.is(NaN, NaN)) // true
}
```

### 2.2 babel使用（javascript编译器）







