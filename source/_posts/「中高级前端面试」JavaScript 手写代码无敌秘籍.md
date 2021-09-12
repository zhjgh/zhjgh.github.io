---
title: 「中高级前端面试」JavaScript 手写代码无敌秘籍
date: 2021-05-01
---

### 1. 实现一个 new 操作符

```javascript
function New(func) {
  var res = {};
  if (func.prototype !== null) {
    res.__proto__ = func.prototype;
  }
  var ret = func.apply(res, Array.prototype.slice.call(arguments, 1));
  if ((typeof ret === "object" || typeof ret === "function") && ret !== null) {
    return ret;
  }
  return res;
}
var obj = New(A, 1, 2);
// equals to
var obj = new A(1, 2);
```
