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
let fn = () => {
  console.log(arguments) // arguments is not defined 
}
fn(1,2,3)

// rest参数
let fn = (...arg) => {
  console.log(arg) // 1,2,3
}

fn(1,2,3)

// 箭头函数的this
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

// 箭头函数本身没有this, 调用箭头函数的this时， 指向是其声明时所在的作用域的this

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

// 参数默认值
let fn = (a = 1, b = 2) => {
  console.log(a * b)
}
```

### 1.7 数组新增扩展

```javascript
Array.form // 类数组转成数组
{
  let lis = document.querySelectorAll("li")
  let arr = []
  lis = Array.from(lis, (item, index) => {
    console.log(item, index, this)
    return index;
  }, arr)
  console.log(lis)
}

Array.of
```

