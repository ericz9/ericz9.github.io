---
title: 我的JavaScript学习之路 - 继承
date: 2016-10-25 14:15:53
tags: JavaScript学习之路
---
习惯了后端编程，对于JavaScript的面向对象处理方式，极度不适应，这个知识点的学习，还是主要来源于w3school。

## 实现思路
使用对象冒充继承父类构造函数的属性，用原型链继承父类prototype对象的方法

## 概念
对象冒充和原型链概念的理解

### 对象冒充
有两个函数A和A1，A1需要继承自A。只需在A1中定义一个变量，指向A，然后调用它，A1就会得到来自A中定义的属性和方法。如：
```javascript
    function A(name){
        this.name = name;
    }
    function A1(name){
        this.new = A;
        //执行A构造函数
        this.new(name);
        delete this.new;
    }
```
#### 更简洁的方式
使用call()和apply()，可以更简洁的实现继承，这两个方法的作用相同，就是参数不同。
```
fun.call(thisArg[, arg1[, arg2[, ...]]])
fun.apply(thisArg, [argsArray])
```
可以看到这两个方法的第一个参数是一样的，区别在于call从第二个参数开始，后面有许多参数；而apply的第二个参数接受一个数组参数。
**这两个方法的作用在于改变函数的作用域，使调用的构造函数的this指向为我们传递过去的对象。**

所以我们可以这样做：
```javascript
    function A1(name){
        A.call(this, name)
    }
```