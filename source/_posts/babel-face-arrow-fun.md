---
title: Babel 是如何处理箭头函数的？
toc: true
date: 2023-09-16 20:40:29
categories:
- Web开发
tags: 
- 前端
- JavaScript
- 开源
- 开发
---

Babel 是一个 JavaScript 编译器，它可以把新版本的 JavaScript 代码转换成兼容旧环境的代码。其中一个常用的转换功能是把箭头函数转换成普通函数，但是仍然保持了箭头函数的词法作用域。本文将从 Babel 的特性出发，来介绍 Babel 转换箭头函数的原理。

<!--more-->

### 箭头函数特点

箭头函数是 ES2015+ 的一个新特性，它可以让我们用更简洁的语法来定义函数。箭头函数有以下几个特点：

- 箭头函数没有自己的 this，它会继承外层作用域的 this 值。
- 箭头函数没有自己的 arguments 对象，它会继承外层作用域的 arguments 对象。
- 箭头函数不能用作构造函数，也不能使用 new 关键字来调用。
- 箭头函数没有 prototype 属性。
- 箭头函数不能使用 yield 关键字，不能作为生成器函数。

这些特点使得箭头函数更适合用于一些简单的、无副作用的、纯函数式的场景，比如回调函数、数组方法、对象方法等。下面是使用箭头函数的例子：

```javascript
// 无参数的箭头函数
var a = () => {};
// 一个参数的箭头函数
var a = b => b;
// 多个参数的箭头函数
var a = (b, c) => b + c;
// 返回对象字面量的箭头函数
var a = b => ({ name: b });
// 包含多条语句的箭头函数
var a = b => {
  console.log(b);
  return b * 2;
};
// 作为数组方法的回调函数
const double = [1, 2, 3].map(num => num * 2);
console.log(double); // [2,4,6]
// 作为对象方法
var bob = {
  _name: "Bob",
  _friends: ["Sally", "Tom"],
  printFriends() {
    this._friends.forEach(f => console.log(this._name + " knows " + f));
  },
};
console.log(bob.printFriends());
```



### 利用 Babel 进行转换

然而，并不是所有的浏览器或环境都支持箭头函数，为了让代码能够在更多的平台上运行，前端需要使用 Babel 来把箭头函数转换成普通函数。Babel 提供了一个插件 @babel/plugin-transform-arrow-functions，它可以做到以下几件事情：

- 转换语法，把箭头符号（=>）替换成 function 关键字，并添加必要的括号和花括号。
- 填充缺失的特性，把箭头函数中使用到的 this 和 arguments 替换成外层作用域中对应的变量，并在必要时使用 .bind(this) 来绑定 this 的值。
- 添加运行时检查，确保箭头函数不会被实例化或使用 new 关键字调用。
- 添加函数名，根据变量名或属性名来给匿名的箭头函数添加一个 name 属性。



下面是使用 Babel 转换后的代码：

```javascript
// 无参数的箭头函数
var a = function() {};
// 一个参数的箭头函数
var a = function(b) {
  return b;
};
// 多个参数的箭头函数
var a = function(b, c) {
  return b + c;
};
// 返回对象字面量的箭头函数
var a = function(b) {
  return { name: b };
};
// 包含多条语句的箭头函数
var a = function(b) {
  console.log(b);
  return b * 2;
};
// 作为数组方法的回调函数
const double = [1, 2, 3].map(function(num) {
  return num * 2;
});
console.log(double); // [2,4,6]
// 作为对象方法
var bob = {
  _name: "Bob",
  _friends: ["Sally", "Tom"],
  printFriends() {
    var _this = this;
    this._friends.forEach(function(f) {
      return console.log(_this._name + " knows " + f);
    });
  },
};
console.log(bob.printFriends());
```



从上面的代码可以看出，Babel 把箭头函数转换成了普通函数，但是仍然保持了箭头函数的词法作用域。这是因为 Babel 使用了以下几种技巧：

- 在箭头函数中使用到 this 的地方，Babel 会把 this 替换成一个变量，比如 _this，并在外层作用域中声明这个变量，并赋值为 this。这样就可以避免使用 .bind(this) 来绑定 this 的值，提高性能。
- 在箭头函数中使用到 arguments 的地方，Babel 会把 arguments 替换成一个变量，比如 _arguments，并在外层作用域中声明这个变量，并赋值为 arguments。这样就可以避免使用 Array.prototype.slice.call(arguments) 来获取参数列表，提高性能。
- 在箭头函数中使用到 new.target 的地方，Babel 会把 new.target 替换成一个变量，比如 _newTarget，并在外层作用域中声明这个变量，并赋值为 new.target。这样就可以避免使用 Object.defineProperty 来定义 new.target 属性，提高性能。
- 在箭头函数的开头，Babel 会添加一个运行时检查，如果发现箭头函数被实例化或使用 new 关键字调用，就会抛出一个 TypeError 异常。这样就可以避免箭头函数被误用，保持语义一致。
- 在箭头函数的末尾，Babel 会添加一个 name 属性，根据变量名或属性名来给匿名的箭头函数添加一个名称。这样就可以方便调试和追踪错误。



### 总结

Babel 可以将箭头函数转换成普通函数，但是仍然保持了箭头函数的词法作用域。这使得我们可以在不影响代码逻辑和性能的情况下，使用更简洁和优雅的语法来定义函数。作为一个强大和灵活的 JavaScript 编译器，它既可以让我们享受最新的语言特性，同时兼容更多的浏览器和环境。
