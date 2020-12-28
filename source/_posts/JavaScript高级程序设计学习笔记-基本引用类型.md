---
title: JavaScript高级程序设计学习笔记 - 基本引用类型
date: 2020-12-23 10:25:19
tags: [红宝书, 红宝书第5章（基本引用类型）]
---
本文主要介绍 Date、RegExp 和 Math 的 API 方法

# Date
## 创建日期对象
```javascript
const foo = new Date(); // 当前时间
const bar = new Date(Date.now()); // 基于毫秒数创建，Date.now() 返回当前时间的毫秒数
const baz = new Date('12/23/2020'); // 基于日期创建（月/日/年）
```

## 格式化
```javascript
const foo = new Date('12/23/2020'); // 基于日期创建（月/日/年）
console.log(foo.toString()); // Wed Dec 23 2020 00:00:00 GMT+0800 (中国标准时间)
console.log(foo.toLocaleString()); // 2020/12/23 上午12:00:00
console.log(foo.toDateString()); // Wed Dec 23 2020
console.log(foo.toLocaleDateString()); // 2020/12/23
console.log(foo.toTimeString()); // 00:00:00 GMT+0800 (中国标准时间)
console.log(foo.toLocaleTimeString()); // 上午12:00:00
```

## 其他方法
```javascript
// ISO 8601 扩展格式“YYYY-MM-DDTHH:mm:ss.sssZ”，如 2019-05-23T00:00:00（只适用于兼容 ES5 的实现）
const foo = new Date('2020-12-23T11:12:12.678');

// 日期的毫秒表示
foo.getTime(); // 1608693132000
foo.setTime();

// 4位数年
foo.getFullYear(); // 2020
foo.setFullYear();

// 日期中的月（0 - 11）
foo.getMonth(); // 11
foo.setMonth();

// 日期中的日（1 - 31）
foo.getDate(); // 23
foo.setDate();

// 日期中的周几（0（周日） - 6）
foo.getDay(); // 3

// 日期中的时（0 - 23）
foo.getHours(); // 11
foo.setHours();

// 日期中的分（0 - 59）
foo.getMinutes(); // 12
foo.setMinutes();

// 日期中的秒（0 - 59）
foo.getSeconds(); // 12
foo.setSeconds();

// 日期中的毫秒
foo.getMilliseconds(); // 678
foo.setMilliseconds();
```

# RegExp
正则表达式可以使用字面量或构造函数来创建，它们是等效的。
```javascript
const foo = 'foobar';
const pattern1 = /bar/g;
const pattern2 = new RegExp('bar', 'g');
```

## 匹配标记
* g：全局匹配
* i：不区分大小写
* m：多行搜索
* y：粘附模式，表示只查找从 lastIndex 开始及之后的字符串
* u：使用 unicode 进行匹配
* s：允许 . 匹配换行符

## 特殊字符
字符 | 含义 | 示例
:-: | :-: | :-:
^ | 匹配输入的开始 | `/^A/.test('aA'); // false` <br /> `/^A/.test('Aa'); // true`
$ | 匹配输入的结束 | `/A$/.test('aA'); // true` <br /> `/A$/.test('Aa'); // false`
\* | 匹配前一个表达式 0 次或多次，等价于 {0,} |
\+ | 匹配前面一个表达式 1 次或多次，等价于 {1,} |
? | 匹配前面一个表达式 0 次或 1 次，等价于 {0,1} |
. | 默认匹配除换行符之外的任何单个字符 |
x\|y | 匹配 x 或 y |
{n} | n 是一个正整数，匹配前面一个字符刚好出现了 n 次 |
{n,} | n是一个正整数，匹配前一个字符至少出现了n次 |
{n,m} | n 和 m 都是整数。匹配前面的字符至少n次，最多m次。如果 n 或者 m 的值是0， 这个值被忽略 |
[xyz] | 匹配方括号中的任意字符 |
[^xyz] | 匹配任何没有包含在方括号中的字符 |
\d | 匹配一个数字，等价于[0-9] |
\D | 匹配一个非数字字符，等价于[^0-9] |
\n | 匹配一个换行符 |
\r | 匹配一个回车符 |
\s | 匹配一个空白字符，包括空格、制表符、换页符和换行符 |
\S | 匹配一个非空白字符 |
\w | 匹配一个单字字符（字母、数字或者下划线）。等价于 [A-Za-z0-9_] |
\W | 匹配一个非单字字符。等价于 [^A-Za-z0-9_] |

# Math
## 最大值与最小值
```javascript
console.log(Math.min(3, 54, 32, 16)); //3
console.log(Math.max(3, 54, 32, 16)); //54
```

## 舍入
```javascript
const foo = 2.49;

// 向上舍入为最接近的整数
console.log(Math.ceil(foo)); // 3

// 向下舍入为最接近的整数
console.log(Math.floor(foo)); // 2

// 四舍五入
console.log(Math.round(foo)); // 2
```

## 随机数
```javascript
console.log(Math.random()); // 返回 0 到 1 范围内的随机数
```

<style>
table th:first-child {
  width: 50px;
}
</style>