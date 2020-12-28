---
title: JavaScript高级程序设计学习笔记 - 变量声明
date: 2020-12-16 11:17:35
tags: [红宝书, 红宝书第3章（语言基础）]
---
首先要知道的是，ECMAScript 中一切都区分大小写。无论是变量、函数名还是操作符，都区分大小写。

有 3 个关键字可以声明变量：var、const 和 let。其中，var 在ECMAScript 的所有版本中都可以使用，而 const 和 let 只能在 ECMAScript 6 及更晚的版本中使用。

# var 关键字
使用 var 操作符定义的变量有两个特点：
* **局部变量，声明范围为函数作用域**
它的作用域意外着，该变量会在函数退出时销毁

* **声明自动提升**
声明自动提升的意思是，把所有变量声明都拉到函数作用域顶部
```javascript
function foo() {
  console.log(age);
  var age = 26;
}
foo(); // undefined
```
之所以不会报错，是因为它等价于
```javascript
function foo() {
  var age;
  console.log(age);
  age = 26;
}
foo(); // undefined
```

# let 声明
* let 跟 var 最大的区别是，let 声明的范围是块级作用域
```javascript
function foo() {
  var name = 'Matt';
  if (true) {
    let age = 20;
    console.log(age); // 20
  }
  console.log(name); // Matt
  console.log(age); // ReferenceError: age is not defined
}
```

* let 声明的变量不会在作用域中被提升，所以在声明之前不能引用，此为“暂时性死区”（temporal dead zone）

* let 在全局作用域中声明的变量不会成为 window 对象的属性（var 声明的变量则会）

## for 循环中的 let 声明
使用 var 定义的迭代变量会渗透到循环体外部
```javascript
for (var i = 0; i < 5; ++i) {
  // 循环逻辑
}
console.log(i); // 5
```

使用 let 定义的迭代变量的作用域仅限于循环内部，JavaScript 引擎在后台会为每个迭代循环声明一个新的迭代变量
```javascript
for (let i = 0; i < 5; ++i) {
  // 循环逻辑
}
console.log(i); // ReferenceError: i is not defined
```

# const 声明
const 与 let 唯一一个重要的区别是，声明时必须同时初始化值，且后续不可再修改。

需要注意的是，这个限制只适用于它指向的变量的引用。换句话说，如果使用 const 定义了一个对象，则后续是可以再修改的。
```javascript
const foo = 'foo'
foo = 'bar' // Assignment to constant variable

const bar = {}
bar.name = 'bar' // ok

const baz = []
baz.push('baz') // ok
```