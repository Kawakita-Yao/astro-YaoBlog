---
title: "JavaScrpit Intro"
published: 2025-07-26
description: "Notes on JavaScript fundamentals for beginners."
image: "/images/JavaScript_Logo.png" # cover image
tags: [Notes, Intro]
category: "JavaScript"
draft: false
lang: zh-CN
---

# JS v.s. Python / C

> JS 与 C / Python 的异同速览

# Variables scope

> var / let / const，作用域 & Hoisting

## JavaScript 中的变量声明对比：var let const

### 语法对比表

| 特性                     | `var`                           | `let`                   | `const`                 |
| ------------------------ | ------------------------------- | ----------------------- | ----------------------- |
| 作用域                   | 函数作用域（Function Scope）    | 块作用域（Block Scope） | 块作用域（Block Scope） |
| 是否可重新赋值           | ✅ 是                           | ✅ 是                   | ❌ 否（不能重新赋值）   |
| 是否可重新声明           | ✅ 是（同一作用域内）           | ❌ 否                   | ❌ 否                   |
| **是否提升（Hoisting）** | ✅ 是（但初始化为 `undefined`） | ✅ 是（但不能提前访问） | ✅ 是（但不能提前访问） |
| 是否建议使用             | ❌ 不推荐（易出错）             | ✅ 推荐                 | ✅ 推荐（用于常量）     |

### Block Scope

```js
// 块作用域对比
{
  var x = 1;
  let y = 2;
  const z = 3;
}

console.log(x); // ✅ 1
console.log(y); // ❌ ReferenceError
console.log(z); // ❌ ReferenceError

// var 没有块作用域，let 和 const 有
```

### Hoisting

```js
console.log(a); // undefined
var a = 10;

console.log(b); // ❌ ReferenceError
let b = 20;

console.log(c); // ❌ ReferenceError
const c = 30;

// var 声明被提升但值为 undefined，let 和 const 声明也提升但不能访问（暂时性死区，TDZ）。
```

### const 不可修改值？

```js
const num = 5;
num = 10; // ❌ TypeError

const obj = { name: "Alice" };
obj.name = "Bob"; // ✅ 可以修改属性

// const 对象的属性可以修改，不可变的是变量绑定
```

# Types Coercion

> 动态类型、类型转换 & == vs ===

# Functions Closure

> 函数、箭头函数、闭包

## 箭头函数

ES6 新增，更加简洁

```js
// 传统函数写法
const hello = function () {
  return "Hello World!";
};

// 箭头函数写法
const hello = () => {
  return "Hello World!";
};

// 更简短写法（只有一句返回）
const hello = () => "Hello World!";
```

:::tip

- 如果你需要函数自己的 `this`，不要用箭头函数<br/>
- 普通函数可以做构造函数，箭头函数不行

:::

## JavaScript 回调函数 `Callback`

### 什么是回调函数？

回调函数就是**作为 arg 传递给另一个函数的函数**, 当前面的任务完成后才调用它

### 为什么用回调？

JavaScript 是单线程的, 但是实际上很多操作是异步的(_Asynchronous_)（比如网络请求、定时器）, 用回调可以确保在任务完成后执行代码

:::note[Example]

```js "callback"
function greet(name, callback) {
  console.log("Hello, " + name);
  callback();
}

function sayGoodbye() {
  console.log("Goodbye!");
}

greet("Alice", sayGoodbye);

// 输出：
// Hello, Alice
// Goodbye!
```

:::

# JS Objects Classes

> 对象、构造函数、class

## 构造函数写法（ES5 风格）

```js "this" "new"
function Car(brand) {
  this.brand = brand;
  this.model = "xxx";
  this.year = 0;

  this.set_model = function (model) {
    this.model = model;
  };
  this.set_year = function (year) {
    this.year = year;
  };
  this.get_info = function () {
    console.log(
      `Brand: ${this.brand}, Model: ${this.model}, Year: ${this.year}`
    );
  };
}

const car = new Car("BMW");
car.set_model("i8");
car.set_year(2022);
car.get_info(); // Brand: BMW, Model: i8, Year: 2022
```

:::note

- 用 `function` 定义构造函数

- 用 `new` 创建**Instance**, `this` 指向新对象

:::

## Class 写法

```js "class" "constructor"
class Car {
  constructor(brand) {
    this.brand = brand;
    this.model = "xxx";
    this.year = 0;
  }

  set_model(model) {
    this.model = model;
  }

  set_year(year) {
    this.year = year;
  }

  get_info() {
    console.log(
      `Brand: ${this.brand}, Model: ${this.model}, Year: ${this.year}`
    );
  }
}
```

:::note

- 更类似 Python 的 class
- 与 Python Class 的对比
  | 语言 | 用法 |
  | ---------- | ----------------------------------- |
  | Python | `class Car: def __init__...` |
  | JavaScript | `class Car { constructor() {...} }` |
  :::

├── 01-js-vs-python-c.md # JS 与 C / Python 的异同速览
├── 02-variables-scope.md # var / let / const，作用域 & Hoisting
├── 03-types-coercion.md # 动态类型、类型转换 & == vs ===
├── 04-functions-closure.md # 函数、箭头函数、闭包
├── 05-objects-arrays.md # 对象、数组操作与解构
├── 06-this-and-context.md # this、call/apply/bind
├── 07-async-await.md # 异步模型、Promise、async/await
├── 08-dom-basics.md # DOM API 简介，常用操作
├── 09-event-handling.md # 事件绑定、冒泡、事件对象
├── 10-modules-fetch.md # 模块 import/export、fetch 请求
├── 11-common-gotchas.md # JS 陷阱、易错点、怪异行为
