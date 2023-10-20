---
title: 在业务中基础和常见的 JavaScript 遍历代码（一）
toc: true
date: 2023-10-23 00:00:00
categories:
- Web开发
tags: 
- 前端
- JavaScript
- 开源
- 开发
---

## 一、如何遍历对象，并输出一个数组，其中每个元素都是包含 key 和 value 属性的对象?

### 要求：

下面有一个对象`obj1`：

```javascript
const obj = {
  "0740377c": "甲",
  "b216d19": "乙",
  "b4f607cb": "丙",
  "c9aa4fe7": "丁",
};
```

<!--more-->

请遍历这个对象，输出以下数组 `arr`。

```javascript
const arr = [
  {
    key: "0740377c",
    value: "甲",
  },
  { key: "b216d19", value: "乙" },
  {
    key: "c716efda",
    value: "丙",
  },
  {
    key: "c9aa4fe7",
    value: "丁",
  },
]
```

### 代码：

```javascript
let arr = [];

// 必须 [key,value]，否则会出问题。
for (let [key, value] of Object.entries(obj)) {
  arr.push({
    key: key,
    value: value,
  });
}
```

### 逻辑分析：

- 利用 Object.entries 将对象转换成二维数组
- 使用 for...of 遍历二维数组
- 解构出 key 和 value
- 将它们组装成对象推入结果数组



## 二、提取对象数组中的部分属性构造新对象数组

下面有一个对象数组`arr1`：

```javascript
const arr1 = [
  {
    "postId": "48472cb3",
    "postName": "前端",
    "postType": "Web",
    "postDescription": "HTML",
    "postSalary": "6000-8000",
    "postRequirement": "大专"
  },
  {
    "postId":"a32a28d5",
    "postName": "后端",
    "postType": "Web",
    "postDescription": "SpringBoot",
    "postSalary": "7000-10000",
    "postRequirement": "本科"
  }
],
```

请遍历这个对象，输出以下数组 `arr2`。

```javascript
const arr2 = [
  {
    label: "前端",
    value: "48472cb3",
  },
  { 
    label: "后端", 
    value: "a32a28d5" 
  },
]
```

### 代码：

```javascript
const arr2 = [];

for (let obj of arr1) {
  arr2.push({
    label: obj.postName,
    value: obj.postId 
  });
}
```

### 逻辑分析：

- 给定一个对象数组arr1,里面包含多个对象
- 每个对象包含postId、postName等多个属性
- 需要遍历这个arr1数组
- 从每个对象中取出postName和postId属性
- 将取出的postName和postId构造成一个新对象
- 将构造出的新对象收集到一个新数组arr2中
- 最终得到一个arr2数组,其中每个元素是一个包含label和value属性的对象
- label属性值来源于原数组对象的postName
- value属性值来源于原数组对象的postId
