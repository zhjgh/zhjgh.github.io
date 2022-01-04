---
title: JS基础
date: 2021-12-25
---

# 原始数据类型

* boolean
* null
* undefined
* null
* string
* symbol

注意：
1、原始类型存储的都是值，是没有函数可以调用的
2、typeof null输出object，这是JS存在的一个悠久Bug

# 对象类型
在JS中，除了原始类型，其他都是对象类型。对象类型和原始类型不同的是，原始类型存储的是值，对象类型存储的是地址（指针）。当你创建了一个对象类型的时候，计算机会在内存中开辟一个空间来存放值，但是我们需要找到这个空间，这个空间会拥有一个地址（指针）。

# typeof vs instanceof

## typeof
typeof 对于原始类型来说，除了null都是可以显示正确的类型
```js
typeof 1 // 'number'
typeof '1' // 'string'
typeof undefined // 'undefined'
typeof true // 'boolean'
typeof Symbol // 'symbol'
```

typeof 对于对象来说，除了函数都会显示object，所以 typeof 并不能准确判断变量到底是什么类型
```js
typeof [] // 'object'
typeof {} // 'object'
typeof console.log // 'function'
```

## instanceof
如果我们想判断一个对象的正确类型，这时候可以考虑使用 instanceof,因为内部机制是通过原型链来判断的
```js
const Person = function() {}
const p1 = new Person()
p1 instanceof Person // true

var str = 'hello world'
str instanceof String // true

var str1 = new String('hello world')
str1 instanceof String // true
```

对于原始类型来说，想直接通过 instanceof 来判断是不行的，当然我们还是有办法让 instanceof 判断原始类型的

# 类型转换

# this
```js
function foo(){
    console.log(this.a)
}
var a = 1
foo()

const obj = {
    a: 2,
    foo: foo
}
obj.foo()

const c = new foo()
```
* 对于直接调用foo来说，不管foo函数被放在了什么地方，this一定是window
* 对于obj.foo()来说，谁调用了函数，谁就是this，所以在这个场景下foo函数中this就是obj对象
* 对于new方式来说，this被永远绑定在了c上面，不会被任何方式改变this

箭头函数中的this
```js
function a(){
    return () => {
        return () => {
            console.log(this)
        }
    }
}
console.log(a()())
```
箭头函数是没有this的，箭头函数中的this只取决包裹箭头函数的第一个普通函数的this。在这个例子中，因为包裹箭头函数的第一个普通函数是a，所以此时的this是window。另外对箭头函数使用bind这类函数是无效的。

最后这种情况就是bind这些改变上下文的API了，对于这些函数来说，this取决于第一个参数，如果第一个参数为空，那么就是window。
```js
let a = {}
let fn = function() { console.log(this) }
fn.bind().bind(a) // window
```
可以从上述代码中发现，不管我们给函数bind几次，fn中的this永远由第一次bind决定，所以结果永远是widnow。

以上就是this的规则了，但是可能会发生多个规则同事出现的情况，这时候不同的规则之间会根据优先级最高的来决定this最终指向哪里。

new —> bind —> obj.foo() -> foo()

# 闭包
闭包的定义：函数A内部有个函数B，函数B可以访问函数A中的变量，那么函数B就是闭包
```js
function A(){
    let a = 1
    window.B = function(){
        console.log(a)
    }
}
A()
B() // 1
```
在JS中，闭包存在的意义就是让我们可以间接访问函数内部的变量。

经典面试题，循环中使用闭包解决`var`定义函数的问题
```js
for(var i = 0;i <= 5; i++){
    setTimeout(function timer(){
        console.log(i)
    }, i * 1000)
}
```
首先因为setTimeout是个异步函数，所以会等循环全部执行完毕，这时候i就是6了，所以会输出一堆6。

解决办法有三种，第一种是使用闭包方式
```js
for(var i = 0; i <= 5; i++){
    ;(function(j){
        setTimeout(function timer(){
            console.log(j)
        }, j * 1000)
    })(i)
}
```
在上述代码中，首先使用了立即执行函数将传入函数内部，这时候值就被固定在了参数j上面不会改变，当下次执行timer这个闭包的时候，就可以使用外部函数的变量j，从而达到母的。

第二种就是使用setTimeout的第三个参数，这个参数会被当成timer函数的参数传入。
```js
for(var i = 1; i <= 5; i++){
    setTimeout(function timer(j){
        console.log(j)
    }, i * 1000, i)
}
```

第三种就是使用let定义i来解决问题，这个也是最为推荐的方式。
```js
for(let i = 0; i <= 5; i++){
    setTimeout(function timer(){
        console.log(i)
    }, i * 1000)
}
```

# 深浅拷贝
我们了解了对象在赋值的过程中其实是复制了地址，从而会导致改变了一方其他也都被改变的情况。通常在开发过程中我们不希望出现这样的情况，我们可以使用浅拷贝来解决这个情况。
```js
let a = {
    age: 1
}
let b = a
a.age = 2
console.log(b.age) // 2
```

## 浅拷贝

### Object.assign
> 只会拷贝所有的属性值到新的对象中，如果属性值是对象的话，拷贝的是地址，所以并不是深拷贝
```js
let a = {
    age: 1
}
let b = Object.assign({}, a)
a.age = 2
console.log(b.age) // 1
```

### 展开运算符（...）
```js
let a = {
    age: 1
}
let b = {...a}
a.age = 2
console.log(b.age) // 1
```

通常浅拷贝就能解决大部分问题了，但是当我们遇到如下情况就可能需要使用到深拷贝了
```js
let a = {
    age: 1,
    jobs: {
        first: 'FE'
    }
}
let b = {...a}
a.jobs.first = 'native'
console.log(b.jobs.first) // native
```
浅拷贝只解决了第一层的问题，如果接下去的值还有对象的话，那么就又回到最开始的话题了，两者享有相同的地址。要解决这个问题，我们就得使用深拷贝了。

## 深拷贝
这个问题通常可以通过JSON.parse(JSON.stringify(object))来解决
```js
let a = {
    age: 1,
    jobs: {
        first: 'FE'
    }
}
let b = JSON.parse(JSON.stringify(a))
a.jobs.first = 'native'
console.log(b.jobs.first) // FE
```

但是该方法也是有局限性的：
* 会忽略undefined
* 会忽略symbol
* 不能序列化函数
* 不能解决循环引用的对象

在遇到函数、undefined或者symbol的时候，该对象也不能正常的序列化
```js
let a = {
    age: undefined,
    sex: Symbol('male'),
    jobs: function(){},
    name: 'zhj'
}
let b = JSON.parse(JSON.stringify(a))
console.log(b) // {name: 'zhj'}
```

## 手写实现简易版深拷贝
> 实现一个深拷贝是很困难的，需要我们考虑好多种边界情况，比如原型链如何处理、DOM如何处理等等，推荐使用lodash深拷贝函数

```js
function deepClone(obj){
    function isObject(o){
        return (typeof o === 'object' || typeof o === 'function') && o !== null
    }

    if(!isObject(obj)){
        throw new Error('非对象')
    }

    let isArray = Array.isArray(obj)
    let newObj = isArray ? [...obj] : {...obj}
    Reflect.ownKeys(newObj).forEach(key => {
        newObj[key] = isObject(obj[key]) ? deepClone(obj[key]) : obj[key]
    })

    return newObj
}

let obj = {
    a: [1,2,3],
    b: {
        c: 2,
        d: 3
    }
}
let newObj = deepClone(obj)
newObj.b.c = 1
console.log(obj.b.c) // 2
```

# 原型和原型链
每个实例对象都有一个私有属性__proto__，指向它的构造函数的原型对象（prototype）。原型对象也有自己的__proto__，层层向上直到一个对象的原型对象为null。这一层层原型就是原型链。

# 继承（原型继承和Class继承）

## 组合继承
```js
function Parent(value){
    this.val = value
}
Parent.prototype.getValue = function(){
    console.log(this.val)
}
function Child(value){
    Parent.call(this, value)
}
Child.prototype = new Parent()

const child = new Child(1)
console.log(child.getValue()) // 1
console.log(child instanceof Parent) // true
```

原理：
1、子类的构造函数中通过Parent.call(this)继承父类中的属性
2、改变子类的原型为new Parent()类继承父类中的函数

优点：
1、构造函数可以传参，不会与父类引用属性共享
2、可以复用父类的函数

缺点：继承父类函数的时候调用了父类构造函数，导致子类的原型上多了不需要的父类属性，存在内存上的浪费。

## 寄生组合继承
```js
function Parent(value){
    this.val = value
}
Parent.prototype.getValue = function(){
    console.log(this.val)
}
function Child(value){
    Parent.call(this, value)
}
Child.prototype = Object.create(Parent.prototype, {
    constructor: {
        value: Child,
        enumerable: false,
        writable: true,
        configurable: true
    }
})

const child = new Child(1)
console.log(child.getValue()) // 1
console.log(child instanceof Parent) // true
```

原理：
1、将父类的原型赋值给了子类
2、将构造函数设置为子类

优点：
1、解决了无用的父类属性问题
2、还能正确找到子类的构造函数

## Class 继承
```js
class Parent{
    constructor(value){
        this.val = value
    }
    getValue(){
        console.log(this.val)
    }
}
class Child extends Parent{
    constructor(value){
        super(value)
    }
}

const child = new Child(1)
console.log(child.getValue()) // 1
console.log(child instanceof Parent) // true
```
