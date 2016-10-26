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
    function A1(name, color){
        this.new = A;
        //执行A构造函数
        this.new(name);
        delete this.new;
        this.color = color;
    }
```
#### 更简洁的方式
使用call()和apply()，可以更简洁的实现继承，这两个方法的作用相同，就是参数不同。
```
fun.call(thisArg[, arg1[, arg2[, ...]]])
fun.apply(thisArg, [argsArray])
```
可以看到这两个方法的第一个参数是一样的，区别在于call从第二个参数开始，后面有许多参数；而apply的第二个参数接受一个数组。
**这两个方法的作用在于改变函数的作用域，使调用的构造函数的this指向为我们传递过去的对象。**

所以我们可以这样做：
```javascript
    function A1(name, color){
        A.call(this, name);
        this.color = color;
    }
    //或者
    function A1(name, color){
        A.apply(this, [name]);
        this.color = color;
    }
```

### 原型链
ECMAScript中描述了原型链的概念，并将原型链作为实现继承的主要方法。其基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。当查找一个对象属性时，会先在该对象实例中查找，如果没有找到，则会向上遍历原型链，如果一直没有找到，则最终会遍历到Object.prototype。这也是为什么所有的JavaScript对象都具有Object的基本方法。

这里把A1的prototype设置成A的实例，这样一来，A1将会得到A实例的所有属性。当然你也逐个设置prototype的值，但显然赋值为一个实例化对象来的更简单。
**注意：**
**1: 这里调用A构造函数的时候，并没有给它传递参数。这在原型链中是标准做法，要确保构造函数没有任何参数。** *(这只是标准做法，其实传递参数也没有问题，当你完全用原型链来实现继承的时候，对应的继承属性的值默认就是你传递过去的值，只不过它存在于对象的__proto__属性中。)*
**2: 原型链的方式不支持多重继承，因为会用另一类型的对象重写prototype属性。**
```javascript
    A1.prototype = new A();
```

## 实现
在上一次的"JavaScript对象"文章中，提到了创建类的最好方式是用构造函数定义属性，用原型定义方法，这种方式仍然适应于继承机制，使用对象冒充继承构造函数的属性，用原型链继承对象的方法。
```javascript
    function A1(name, color){
        A.call(this, name);
        this.color = color;
    }
    A1.prototype = new A();
```