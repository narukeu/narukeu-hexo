---
title: Object.keys() 和 Object.value() 哪一个更快？
toc: true
date: 2023-10-19 10:00:00
categories:
- Web开发
tags: 
- 前端
- JavaScript
- 开源
- 开发
---

前端中，我们常常需要获取对象属性，`Object.keys()` 和 `Object.values()` 都是获取对象属性的方法，两者的区别是:

- Object.keys() 返回对象的所有键名

- Object.values() 返回对象的所有键值

<!--more-->

## 进行一次测试

### 开始测试

我们建立一个测试的 JavaScript 文件，来测试一下结果：

```javascript
const obj = {
  name: "MacBook Pro 13.3",
  cpu: "Apple M2",
  os: "macOS Sonoma",
  ssd: "512gb",
  ram: "24gb",
};

console.time("keys");
for (let i = 0; i < 1000000; i++) {
  Object.keys(obj);
}
console.timeEnd("keys");

console.time("values");
for (let i = 0; i < 1000000; i++) {
  Object.values(obj);
}
console.timeEnd("values");
```

### 测试结果

在 Apple M2 的 MacBook 上，执行的结果如下：

```
keys: 26.555ms
values: 26.002ms
```

### 测试结论

`Object.values()` 比 `Object.keys()` 快 0.5 ms。

## 为什么会这样呢？

我们得先分析一下两个方法的原理：

- Object.keys()只是返回键名，需要遍历对象的属性进行推断。

- Object.values()可以直接读取内部属性值数组，减少了中间处理。

`Object.keys()` 的工作流程是遍历对象属性得到每个键名，然后将键名插入数组，最终返回一个数组。

而`Object.values()` 工作流程是访问[[Values]]内部属性，然后直接返回属性值数组。

换一句话说，`Object.values()` 跳过了遍历和插入数组的步骤，直接返回内部的属性值数组。

因此，在实际开发中，如果需要频繁获取对象属性，可以考虑优先使用 `Object.values()`。
