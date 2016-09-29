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
    var person = { age = 24 };
    ```
2. #### 使用对象构造器
   ```javascript
    function person(age){
        this.age = age;
    };
    
    var me = new person(24);
   ```
   
### JavaScript类
#### JavaScript是面向对象的语言，但不使用类，而是基于prototype

### JavaScript for...in循环
```javascript
for(var x in person){
    //输出 age
    console.log(x);
    //输出 24
    console.log(person[x]);
}
```
