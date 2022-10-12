---
title: 重学Vue
date: 2022-09-27
---

# Vue核心

## Vue简介

**Vue特点**

* 采用组件化模式，提高代码复用率，且让代码更好维护
* 声明式编码，让编码人员无需直接操作DOM，提高开发效率
* 使用虚拟DOM + 优秀Diff算法，尽量复用DOM节点

## 初识Vue

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>初识Vue</title>
  <!-- 引入Vue -->
  <script src="https://cdn.jsdelivr.net/npm/vue@2.7.10/dist/vue.js"></script>
</head>

<body>
  <script>
    // 关闭生产提示
    Vue.config.productionTip = false
  </script>
</body>

</html>
```

## 模板语法

```html
插值语法
<h3>Hello, {{name}}</h3>

指令语法
<!-- v-bind:href 简写:href -->
<a :href="url">点我去百度</a>
```

## 数据绑定

v-modal

## MVVM模型



## 数据代理

```js
let number = 18
let person = {
  name: '张三',
  sex: '男',
  // age: 18
}

Object.defineProperty(person, 'age', {
  value: 18,
  enumerable: true, // 控制属性是否可以枚举，默认是false
  writable: true, // 控制属性是否可以被修改，默认是false
  configurable: true, // 控制属性是否可以被删除，默认是false
  get(){
    console.log('有人读取了age属性')
    return number
  },
  set(value){
    console.log(`有人修改了age属性，且值是${value}`)
    number = value
  }
})

console.log(Object.keys(person))

delete person.age

console.log(person)
```

## 事件处理

```js
@click="showInfo($event, 66)"

事件修饰符
1. prevent 阻止默认事件（常用）
2. stop 阻止事件冒泡（常用）
3. once 事件只触发一次（常用）
4. capture 使用事件捕获模式
5. self 只有event.target是当前操作的元素时才触发事件
6. passive 事件的默认行为立即执行，无需等待事件回调执行完毕
```

## 计算属性与监视



## class和style绑定

```js
// 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态绑定
:class="string"

// 绑定class样式--数组写法，适用于：要绑定的样式个数不确定，类名也不确定
:class="classArr"

// 绑定class样式--对象写法，适用于：要绑定的样式个数确定，类名也确定，但要动态决定用不用
:class="classObj"

// 绑定style样式--对象写法
:style="styleObj"

// 绑定style样式--数组写法
:style="[styleObj1, styleObj2]"
```

## 条件渲染

```js
// template只能配合v-if，不能用v-show
<template v-if="n===1">
  <h2>111</h2>
  <h2>222</h2>
  <h2>333</h2>
</template>
```

## 列表渲染

```js

```

## 收集表单数据

## Vue实例生命周期

## 过渡&动画

## 过滤器

## 内置指令与自定义指令

## 自定义插件

# Vue组件化编程

# 使用Vue脚手架

# Vue中的ajax

# Vue

# Vue-router

# Vue UI组件库