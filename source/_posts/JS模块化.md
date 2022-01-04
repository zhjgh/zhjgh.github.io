---
title: JS模块化
date: 2022-01-02
---

# 好处
* 解决命名冲突
* 提供复用性
* 提高代码可维护性

# 立即执行函数
```js
(function(globalVariable){
    globalVariable.test = function(){}
    // ...声明各种变量、函数都不会污染全局作用域
})(globalVariable)
```

# AMD和CMD
```js
// AMD(require.js)
define(['./a', './b'], function(a, b){
    // 加载完毕可以使用
    a.do()
    b.do()
})

// CMD(sea.js)
define(function(require, exports, module){
    // 可以把require写在函数体的任意地方实现延迟加载
    var a = require('./a')
    a.do()
})
```

# CommonJS
```js
// 引入模块
const path = require('path')
// 导出模块
const path = () => {}
module.exports = path
```

特点：
* require()是同步加载模块
* 是基于值的拷贝
* node环境中默认使用CommonJS规范

# ES Module
```js
// 引入模块
import path from 'path'
import { doSomeThing } from 'path'
// 导出模块
export const doSomeThing = () => {}
export default path
```

特点：
* import是异步加载模块
* 基于值的引用
* ES Module会编译成 require/exports 来执行