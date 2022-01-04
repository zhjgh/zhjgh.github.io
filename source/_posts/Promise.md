---
title: Promise
date: 2022-01-02
---

# 特点
1. 三种状态：分别是等待中（pending）, 完成了（resolved）,拒绝了（rejected）
2. 状态一旦从等待中变成其他状态就永远不能更改状态
3. 状态一旦改变不可取消

# 实现一个简易版 Promise
```js
const PENDING = 'pending'
const RESOLVED = 'resolved'
const REJECTED = 'rejected'

function MyPromise(fn){
    const that = this
    that.state = PENDING
    that.value = null
    that.resolvedCallbacks = []
    that.rejectedCallbacks = []

    function resolve(value){
        if(that.state === PENDING){
            that.state = RESOLVED
            that.value = value
            that.resolvedCallbacks.map(cb => cb(that.value))
        }
    }

    function reject(value){
        if(that.state === PENDING){
            that.state = REJECTED
            that.value = value
            that.rejectedCallbacks.map(cb => cb(that.value))
        }
    }

    try{
        fn(resolve, reject)
    }catch(e){
        reject(e)
    }
}

MyPromise.prototype.then = function(onFulfilled, onRejected){
    const that = this
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : v => v
    onRejected = typeof onFulfilled === 'function' ? onRejected : err => { throw err }

    if(that.state === PENDING){
        that.resolvedCallbacks.push(onFulfilled)
        that.rejectedCallbacks.push(onRejected)
    }

    if(that.state === RESOLVED){
        onFulfilled(that.value)
    }

    if(that.state === REJECTED){
        onRejected(that.value)
    }
}

new MyPromise((resolve, reject) => {
    setTimeout(() => {
        resolve(1)
    }, 0)
}).then(value => {
    console.log(value)
})
```
