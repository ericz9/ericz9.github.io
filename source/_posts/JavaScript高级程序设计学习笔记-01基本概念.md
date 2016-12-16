---
title: JavaScript高级程序设计学习笔记 - 01基本概念
date: 2016-12-16 19:53:31
tags: JavaScript高级程序设计学习笔记
---
前端社区的轮子越造越多，作为一个后端码农，看的是眼花缭乱，很容易经不起诱惑跟随潮流去学人家的轮子。索性去当当买一本纸质书“JavaScript高级程序设计”，跟随书本静心学习（**此系列文章仅为本人阅读笔记**）

## 数据类型
ECMAScript包含5种基本数据类型：Undefined、Null、Boolean、Number、String，1种复杂数据类型：Object

### typeof 操作符
由于ECMAScript是松散类型的，typeof用于检测给定变量的数据类型
* "undefined"：如果这个值未定义
* "boolean"：如果这个值是布尔值
* "string"：如果这个值是字符串
* "number"：如果这个值是数值
* "object"：如果这个值是对象或null（null被认为是一个空的对象引用）
* "function"：如果这个值是函数
> 从技术角度来讲，函数是对象，而不是一种数据类型。然而函数也确实有一些特殊的属性，因此有区分的必要。

### Boolean 类型
```javascript
var message = 'Hello world';
if(message){
    console.log('Value is true');
}
```
这段代码能进入if代码块的原因是，存在自动执行的Boolean转换。如可以这样转换`var messageAsBoolean = Boolean(message);`

### Number 类型
使用IEEE754格式来表示整数和浮点数值，并支持十进制、八进制和十六进制。
* 八进制：第一位为0，其余为0-7的数字，如果其中有数字超出8，那么前导0将被忽略，后面数值当作十进制解析。（八进制字面量在严格模式下无效）
* 十六进制：0x开头，后跟任意十六进制数字（0-9及A-F）
在进行算术计算时，所有以八进制和十六进制表示的数值都将被转换成十进制。
```javascript
if(a + b == 0.3){
    console.log('You got 0.3.');
}
```
**注意：永远不要做这样的测试。这是因为基于IEEE754数值的浮点计算的通病，会有舍入误差，可以使用`toFixed()`来解决这个问题。**

至于数值转换的三个函数：`Number()`、`parseInt()`、`parseFloat()`，转换规则实在太多，就不一一列举了，可参见W3school [ECMAScript 类型转换](http://www.w3school.com.cn/js/pro_js_typeconversion.asp)

## 语句

### label 语句 与 break和continue 语句配合使用
这种联合使用的情况，一般发生在循环嵌套的情况下
```javascript
var num = 0;
outermost:
for(var i = 0; i < 10; i++){
    for(var j = 0; j < 10; j++){
        if(i == 7 && j == 7){
            break outermost; //跳出外层循环，返回77
            //continue outermost; //退出内部循环，执行外部循环，返回97
        }
        num++;
    }
}
console.log(num);
```