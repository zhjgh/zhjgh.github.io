---
title: Proxy
date: 2022-01-02
---

# 语法
```js
let p = new Proxy(target, handler)
```

# 参数
1. target: 需要使用Proxy包装的目标对象（可以是任何类型的对象，包括原生数组、函数、甚至另一个代理）
2. handler: 一个对象，其属性是当执行一个操作时定义代理的行为的函数（可以理解为某种触发器）

# 示例
```js
let xiaoming = {
    name: '小明',
    age: 30
}
xiaoming = new Proxy(xiaoming, {
    get(target, key){
        let result = target[key]
        if(key === 'age') result += '岁'
        return result
    },
    set(target, key, value){
        if(key === 'age' && typeof value !== 'number'){
            throw Error('age字段必须是number类型')
        }
        return Reflect.set(target, key, value)
    }
})
console.log(`我叫${xiaoming.name} 我今年${xiaoming.age}了`) // 我叫小明，我今年30岁了
```

1. 首先创建了一个test对象，里面有name属性
2. 然后使用Proxy将其包装起来，在返回给test
3. 此时test已经成为了一个Proxy实例，我们对其的操作，都会被Proxy拦截