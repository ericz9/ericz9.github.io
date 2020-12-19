---
title: JavaScript高级程序设计学习笔记 - 数据类型
date: 2020-12-17 09:30:51
tags: JavaScript高级程序设计学习笔记
---
ECMAScript中有6种原始类型：Undefined、Null、Boolean、Number、String、Symbol，一种复杂类型Object。

# typeof 操作符
* `typeof undefined; // undefined`
* `typeof null; // object`
* `typeof 'hello'; // string`
* `typeof 1; // number`
* `typeof true; // boolean`
* `typeof Symbol(); // symbol`

## 类型判断
从上面可以看出，typeof 可以准确的进行大部分原始类型的检测，唯一例外的是 `typeof null` 返回的是 object。

更严重的是，typeof 对于 Array、Date等类型均返回 object，在很多情况下，这可能并不是我们所期望的。

这时候，你也许需要 `Object.prototype.toString`。
```javascript
/**
 * 类型判断
 * 确保拿到的是原始类型，而不是对应的包装对象
 */
function getType(ele) {
  let eleType;

  // 判断数据是引用类型的情况
  if (typeof ele === 'object') {
    const eleClass = Object.prototype.toString.call(ele);
    const eleClasses = eleClass.split(' ')[1].split('');
    eleClasses.pop();
    eleType = eleClasses.join('').toLowerCase();
  } else {
    // 判断数据是基本数据类型和函数的情况
    eleType = typeof ele;
  }

  return eleType;
}

// test
getType(undefined); // undefined
getType(null); // null
getType('hello'); // string
getType(1); // number
getType(true); // boolean
getType(Symbol()); // symbol
getType({}); // object
getType([]); // array
getType(new Date()); // date
getType(new RegExp()); // regexp
getType(function() {}); // function
getType(window.JSON); // json
```

# Undefined 类型
变量已声明但未初始化时，它的值就是 undefined。
```javascript
let foo;
console.log(foo === undefined); // true
```

# Null 类型
null 值表示一个空对象指针，这也是给typeof 传一个 null 会返回"object"的原因。

undefined 值是由 null 值派生而来的，因此 ECMA-262 将它们定义为表面上相等。
```javascript
console.log(null == undefined); // true
console.log(null === undefined); // false
```

# Boolean 类型
Boolean 有两个字面值： true 和 false。

对于任意数据类型，都可以使用 Boolean() 转型函数获得一个布尔值，下表列出了详细的转换规则：

数据类型 | 转换为 true 的值 | 转换为 false 的值
:-: | :-: | :-:
String | 非空字符串 | 空字符串
Number | 非零数值（包括无穷值） | 0、NaN
Object | 任意对象 | null
Undefined | N/A（不存在） | undefined

# Number 类型
ECMAScript使用 IEEE 754 格式表示整数和浮点值，这就导致在小数运算中，经常产生舍入错误，如 `console.log(0.1 + 0.2); // 0.30000000000000004`

Number()、parseInt() 和 parseFloat() 都可以进行数值转换，它们的区别如下：
* 布尔值
```javascript
console.log(Number(true)); // 1
console.log(Number(false)); // 0
console.log(parseInt(true)); // NaN
console.log(parseInt(false)); // NaN
console.log(parseFloat(true)); // NaN
console.log(parseFloat(false)); // NaN
```
* null 和 undefined
```javascript
console.log(Number(null)); // 0
console.log(Number(undefined)); // NaN
console.log(parseInt(null)); // NaN
console.log(parseInt(undefined)); // NaN
console.log(parseFloat(null)); // NaN
console.log(parseFloat(undefined)); // NaN
```
* 字符串
```javascript
// 空字符串
// parseInt，如果第一个字符不是数值、加号或减号，立即返回 NaN
// parseFloat同上
console.log(Number('')); // 0
console.log(parseInt('')); // NaN
console.log(parseFloat('')); // NaN

// 小数
// parseInt 会取截至到小数点之前的数值
console.log(Number('1.1')); // 1.1
console.log(parseInt('1.1')); // 1
console.log(parseFloat('1.1')); // 1.1

// Number 和 parseFloat 始终会忽略前面的0
// parseInt 则特殊一点，如果以"0x"开头，则被解释为16进制；如果以“0”开头且紧跟数值字符，在非严格模式下，会被某些实现解释为8进制
console.log(Number('01.1')); // 1.1
console.log(parseInt('01.1')); // 1
console.log(parseFloat('01.1')); // 1.1

// 包含非数值字符
// parseInt 和 parseFloat 会依次检测每个字符，直到末尾，或遇到非数值字符
console.log(Number('1.1a')); // NaN
console.log(parseInt('1.1a')); // 1
console.log(parseFloat('1.1a')); // 1.1

// 以非数值、加号或减号开头，立即返回NaN
console.log(Number('a1.1')); // NaN
console.log(parseInt('a1.1')); // NaN
console.log(parseFloat('a1.1')); // NaN

// 多个小数点
// 其实情况与传入的参数为 "1.1a" 时一致
console.log(Number('1.1.1')); // NaN
console.log(parseInt('1.1.1')); // 1
console.log(parseFloat('1.1.1')); // 1.1
```

> 书中建议，如果明确需要转换为整数，可以优先使用 parseInt() 函数，它更专注于字符串中是否包含数值模式。

# String 类型
字符串属于值类型，可以使用双引号"、单引号'或反引号` 表示，一旦创建不可再改变。
```javascript
/**
 * 我们先浅显的理解一下，“一旦创建不可再改变” 不是指变量创建后不能再赋值，而是说变量所保存的值不可再改变
 * 在这个示例中，在 foo 再次赋值时，会先创建一个 another value，保存到 foo，然后销毁 value
 * 而不是将 value 替换为 another value，这就是 “一旦创建不可再改变” 的意思
 */
let foo = 'value';
foo = 'another value';
```
再看一个例子
```javascript
/**
 * 因为 String 是值类型，所创建的变量按值访问
 * 所以 foo 和 bar 都得到了 str 的一个副本，它们相互独立，互不干扰
 */
let str = 'value';
let foo = str;
let bar = str;
str = 'another value';
console.log(foo); // value
console.log(bar); // value
```

## 模板字面量
技术上讲，模板字面量不是字符串，而是一种特殊的 JavaScript 表达式，只不过求值后得到的是字符串。
```javascript
// 所插入的值都会使用 toString() 强制转型为字符串
const foo = 'world';
console.log(`Hello ${foo[0].toUpperCase()}${foo.slice(1)}`); // Hello World
```

### 标签函数
```javascript
let a = 6;
let b = 9;

function simpleTag(strings, aValExpression, bValExpression, sumExpression) {
  console.log(strings);
  console.log(aValExpression);
  console.log(bValExpression);
  console.log(sumExpression);
  return 'foobar';
}

simpleTag`${ a } + ${ b } = ${ a + b }`;

// 这两种调用方式是等效的
// 对于有 n 个插值的模板字面量，传给标签函数的表达式参数的个数始终是 n，而传给标签函数的第一个参数所包含的字符串个数则始终是 n+1
// 第一个参数的值，应是每个插值和边界的间隔：
// 位置0：反引号 ` 和 插值 ${ a } 之间的边界
// 位置1：插值 ${ a } 和 插值 ${ b } 之间的边界
// 位置2：插值 ${ b } 和 插值 ${ a + b } 之间的边界
// 位置3：插值 ${ a + b } 和 反引号 ` 之间的边界
simpleTag(['', ' + ', ' = ', ''], 6, 9, 15);

// 都输出以下内容
// ["", " + ", " = ", ""]
// 6
// 9
// 15
```

个人觉得标签函数的应用场景在于，输出的字符串中，有多个位置需要动态插值。而标签函数的第一个参数的个数始终是 n+1，那我们可以利用这一点，使用数组的 reduce 方法，很方便的求值。

```javascript
function encodeHTML(strings, ...values) {
  return strings.reduce((prev, current, index) => {
    if(index > 0) {
      const value = values[index - 1].replace(/</g,"&lt;").replace(/>/g,"&gt;");
      prev += value;
    }

    return prev + current;
  }, '');
}

const badCode = '<script>alert("abc")</script>';
const message = encodeHTML`code ${badCode} is safely`;

// 等同于
// const message = encodeHTML(['code ', ' is safely'], '<script>alert("abc")</script>');

console.log(message); // code &lt;script&gt;alert("abc")&lt;/script&gt; is safely
```

### 原始字符串
使用 String.raw 或者 通过标签函数的第一个参数，可以方便的取得字符串的原始内容
```javascript
console.log(`\u00A9`); // ©
console.log(String.raw`\u00A9`); // \u00A9

function encodeHTML(strings, ...values) {
  strings.forEach((item) => { console.log(item); /* © */ })
  strings.raw.forEach((item) => { console.log(item); /* \u00A9 */ })
}
encodeHTML`\u00A9`;
```

# Symbol 类型
单独写一篇文章讲解

# Object 类型
Object 是所有对象的基类，它有几个常用属性或方法：
* hasOwnProperty
用来确定属性是在实例上，还是在原型对象上
```javascript
function Person() {};
Person.prototype.name = "Nicholas";
let person = new Person();
person.age = 20;
console.log(person.hasOwnProperty('name')); // false
console.log(person.hasOwnProperty('age')); // true
```
* isPrototypeOf
以上面代码为例，用于判断person对象的原型是否指向Person.prototype
```javascript
console.log(Person.prototype.isPrototypeOf(person)); // true
console.log(person.__proto__ === Person.prototype); // true
```