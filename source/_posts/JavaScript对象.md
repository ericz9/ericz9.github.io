---
title: JavaScript对象
date: 2016-09-29 16:27:04
tags: JavaScript学习
---
[原文](http://www.w3school.com.cn/js/js_objects.asp)
### 对象是指带有属性和方法的特殊数据类型

### 如何创建对象
1. #### 创建直接的实例
   1. 方式一
    ```javascript
    var person = new Object();
    person.age = 24;
    ```
   2. 方式二
   ```javascript
    var person = { age : 24 };
    ```
2. #### 使用对象构造器
   ```javascript
    function person(age){
        this.age = age;
    };
    
    var me = new person(24);
   ```
### for...in循环
```javascript
for(var x in person){
    //输出 age
    console.log(x);
    //输出 24
    console.log(person[x]);
}
```
### JavaScript类
JavaScript是面向对象的语言，但不使用类，而是基于构造函数和prototype
[ECMAScript定义类或对象的几种方式](http://www.w3school.com.cn/js/pro_js_object_defining.asp)
***
一般使用的两种方式：混合构造函数/原型方式、动态原型方式
* 混合构造函数/原型方式
> 联合构造函数和原型方式，就可以像其他程序语言一样创建对象。这种概念非常简单，即用构造函数定义对象的所有非函数属性，用原型方式定义对象的函数属性（方法）。结果是，所有的函数只创建一遍，而每个对象又都具有自己的对象属性实例。

```javascript
function Person(age){
    this.age = age;
}
Person.prototype.introAge = function(){
    console.log('my age is ' + this.age);
};
var oPerson = new Person(24);
console.log(oPerson.age);
console.log(oPerson.introAge());
```
* 动态原型方式
> 动态原型方式的基本想法与上述方式相同，唯一的区别在于赋予对象方法的位置，以达到对属性和方法进行视觉上的封装。

```javascript
function Person(age){
    this.age = age;

    //使用"_initialized"作为标记，来判断是否已给原型赋予了方法
    if(typeof Person._initialized == 'undefined'){
        Person.prototype.introAge = function(){
            console.log('my age is ' + this.age);
        };
        Person._initialized = true;
    }
}
var oPerson = new Person(24);
console.log(oPerson.age);
console.log(oPerson.introAge());
```