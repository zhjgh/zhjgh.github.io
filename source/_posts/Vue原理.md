---
title: Vue原理
date: 2021-08-29
---

## Vue 响应式原理

Vue中的三个核心类：

1. Observer: 给对象的属性添加getter和setter, 用于**依赖收集**和**派发更新**
2. Dep: 用于收集当前响应式对象的依赖关系，每个响应式对象都有dep实例。dep.subs = watcher[]，当数据发生变更的时候通过dep.notify()通知各个watcher。
3. Watcher: 观察者对象，render watcher, computed watcher, user watcher

* 依赖收集

1. initState, 对computed属性初始化时，就会触发computed watcher依赖收集
2. initState, 对监听属性初始化时，触发user watcher依赖收集
3. render, 触发render watcher依赖收集

* 派发更新

1. 组件中对响应的数据进行了修改，会触发setter逻辑
2. dep.notify()
3. 遍历所有subs，调用每个watcher的update方法

总结原理：当创建vue实例时，vue会遍历data里的属性，Object.defineProperty为属性添加getter和setter对数据的读取进行劫持。

getter: 依赖收集
setter: 派发更新

每个组件的实例都会有对应的watcher实例

## 计算属性的实现原理

computed watcher, 计算属性的监听器

computed watcher 持有一个dep实例，通过dirty属性标记计算属性是否需要重新求值。

当computed的依赖值改变后，就会通知订阅的watcher进行更新，对于computed watcher会将dirty属性设置为true,并且进行计算属性方法的调用。

1. computed 所谓的缓存是指什么？

计算属性是基于它的响应式依赖进行缓存的，只有依赖发生改变的时候才会重新求值。

2. 那computed缓存存在的意义是什么？或者你经常在什么时候使用？

比如计算属性方法内部操作非常的耗时，遍历一个极大的数组，计算一次可能要耗时1s

类型转换，格式转换



```js
const largeArray = [
    {...},
    {...},
] // 10w

data: {
    id: 1
}

computed: {
    currentItem: function(){
        return largeArray.find(item => item.id === this.id)
    }
    stringId: function(){
        return String(this.id)
    }
}
```

3. 以下情况，computed可以监听到数据的变化吗？

```js
template
    {{ storageMsg }}

computed: {
    storageMsg: function(){
        return sessionStorage.getItem('xxx')
    },
    time: function(){
        return Date.now()
    }
}

created(){
    sessionStorage.setItem('xxx', 111)
}

onClick(){
    sessionStorage.setItem('xxx', Math.random())
}
```

答案：不会。

## Vue.nextTick的原理

```js
Vue.nextTick(() => {
    // TODO
})

await Vue.nextTick()
// TODO
```

Vue是异步执行dom更新的，一旦观察到数据的变化，把同一个event loop中观察数据变化的watcher推送进这个队列。在下一次事件循环时，Vue清空异步队列，进行dom的更新

异步队列执行顺序
Promise.then -> MutationObserver -> setImmediate -> setTimeout

vm.someData = 'new value', dom并不会马上更新，而是在异步队列被清除时才会更新dom.

事件循环执行顺序
宏任务 -> 微任务队列 -> UI render -> 宏任务

一般什么时候会用到nextTick呢？

在数据变化后要执行某个操作，而这个操作依赖因你数据改变而改变的dom，这个操作就应该被放到vue.nextTick回调中。

```js
<template>
    <div v-if="loaded" ref="test"></div>
</template>

async showDiv(){
    this.loaded = true;
    await Vue.nextTick();
    this.$refs.test.xxx(); // 才能获取到ref
}
```

## 手写一个简单的vue, 实现响应式更新

1. 新建一个目录

* index.html 主页面
* vue.js Vue主文件
* compiler.js 编译模板，解析指令，v-model v-html
* dep.js 收集依赖关系，存储观察者 // 以发布订阅的形式实现
* observer.js 数据劫持
* watcher.js 观察者对象类

2. index.html

```html
<!DOCTYPE html>
<html lang="cn">
    <head>My Vue</head>
    <body>
        <div id="app"></div>
        <script src="./index.js" type="module"></script>
    </body>
</html>
```

3. 初始化vue class, 新建vue.js

```js
/**
 * 包括vue构造函数，接收各种配置参数等等
 */
export default class Vue{
    constructor(options={}){
        this.$options = options;
        this.$data = options.data;
        this.$methods = options.methods;

        this.initRootElement(options);
        // 利用Object.defineProperty将data的属性注入到vue实例中
        this._proxyData(this.$data);
    }
    // 获取更元素，并存储到vue实例。简单检查一下传入的el是否合规
    initRootElement(options){
        if(typeof options.el === 'string'){
            this.$el = document.querySelector(options.el);
        }else if(options.el instanceof HTMLElement){
            this.$el = options.el;
        }

        if(!this.$el){
            throw new Error('传入的el不合法，请传入css selector或者HTMLElement')
        }
    }
    _proxyData(data){
        Object.keys(data).forEach(key => {
            Object.defineProperty(this, key, {
                enumerable: true,
                configurable: true,
                get(){
                    return data[key];
                },
                set(newValue){
                    if(data[key] === newValue){
                        return;
                    }
                    data[key] = newValue
                }
            })
        })
    }
}
```

4. 验证一下，新建index.js

```js
import Vue from './myvue/vue.js';

const vm = new Vue({
    el: '#app',
    data: {
        msg: 'hello world'
    },
    methods: {
        handle(){
            alert(111)
        }
    }
})

console.log(vm)
```

5. vue里可以通过this来获取data里的属性



