---
title: JavaScript 判断对象是否为空的办法
toc: true
date: 2023-10-19 14:11:16
categories:
- Web开发
tags: 
- 前端
- JavaScript
- 开源
- 开发
---

前端中，我们常常需要判断一个对象是否为空对象，在  JavaScript  中有几种方法：

<!--more-->

## 1. 使用 `Object.keys()` 获取对象的所有键,如果长度为 0 则为空对象:

```js
function isEmpty(obj) {
  return Object.keys(obj).length === 0;
}

let obj = {};
isEmpty(obj); // true
```
## 2. 使用`JSON.stringify()`将对象转为JSON字符串,如果为空对象则返回"{}"：

```js 
function isEmpty(obj) {
  return JSON.stringify(obj) === '{}'; 
}

let obj = {};
isEmpty(obj); // true
```
## 3. 检查对象是否有任何可枚举的属性,使用`for-in`循环：

```js
function isEmpty(obj) {
  for(let key in obj) {
    return false;
  }
  return true;
}

let obj = {};
isEmpty(obj); // true
```

## 4. 【ES6】检查对象是否有任何自身属性,使用`Object.hasOwnProperty()`：

```js
function isEmpty(obj) {
  if(Object.getOwnPropertyNames(obj).length > 0) {
    return false;
  }
  return true; 
}

let obj = {};
isEmpty(obj); // true
```

## 5. 【ES6】检查对象是否有任何属性值,使用 `Object.values()`：

```js
function isEmpty(obj) {
  return Object.values(obj).length === 0;
}

let obj = {}; 
isEmpty(obj); // true
```

## 6. 这些办法如何去选择：
考虑兼容性：使用 `Object.keys()`  或 `for-in` 循环,这些方法兼容性较好,能覆盖到较老版本的浏览器。
考虑性能：使用`Object.keys()`或`Object.values()`比较快，避免使用JSON转换字符串的`JSON.stringify()`。
考虑可读性：使用 `Object.keys().length===0` 这样的写法可读性较好。
考虑边界情况：如果对象可能为null可以先判断obj !== null,一些方法在null对象上会报错。
使用ES6方法：如果只考虑现代浏览器,优先使用ES6的 `Object.values()` 或者 `Object.getOwnPropertyNames()` 。
组合使用：如果需要综合考虑,可以组合使用 `Object.keys()` 和 `Object.getOwnPropertyNames()`,既考虑兼容性也检测所有属性。
所以，总结来说:
优先考虑使用 `Object.keys()` 或 `Object.values()`（个人更推荐后者）。
如果需要兼容老浏览器版本,可以使用 `for-in` 循环。
根据目标环境综合考虑选用兼容性好、性能高、可读性强的方法。
