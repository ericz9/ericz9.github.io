---
title: JavaScript高级程序设计学习笔记 - 数据类型(Symbol)
date: 2020-12-21 09:50:17
tags: JavaScript高级程序设计学习笔记
---
Symbol（符号） 是原始值，其实例是唯一、不可变的，它的用途是用做对象的非字符串形式的属性，而不会产生同名冲突。

# 符号初始化
```javascript
// 符号需要使用 Symbol() 函数初始化
let genericSymbol = Symbol();
let otherGenericSymbol = Symbol();
console.log(genericSymbol == otherGenericSymbol); // false 

let fooSymbol = Symbol('foo');
let otherFooSymbol = Symbol('foo');
console.log(fooSymbol == otherFooSymbol); // false 
```

# 全局符号
使用 Symbol.for() 创建全局符号，它是一个幂等操作。
```javascript
let globalSymbol = Symbol.for('foo'); // 创建全局符号
let otherGlobalSymbol = Symbol.for('foo'); // 重用已定义的全局符号
console.log(globalSymbol === otherGlobalSymbol); // true
```

# 符号属性
```javascript
let fooSymbol = Symbol('foo');
let barSymbol = Symbol('bar');
let bazSymbol = Symbol('baz');
let quxSymbol = Symbol('qux');

let o = {
  foo: 'foo',
  [fooSymbol]: 'foo symbol'
};

Object.defineProperty(o, barSymbol, { value: 'bar symbol' });

Object.defineProperties(o, {
  [bazSymbol]: { value: 'baz symbol' },
  [quxSymbol]: { value: 'qux symbol' }
});

// 返回常规属性
console.log(Object.getOwnPropertyNames(o)); // ['foo']

// 返回符号属性
console.log(Object.getOwnPropertySymbols(o)); // [Symbol(foo), Symbol(bar), Symbol(baz), Symbol(qux)]

// 返回常规属性和符号属性
console.log(Reflect.ownKeys(o)); // ["foo", Symbol(foo), Symbol(bar), Symbol(baz), Symbol(qux)]
```

# 常用内置符号
内置符号就是全局函数 Symbol 的普通字符串属性，指向一个符号的实例，它们最重要的用途是方便开发者重新定义，从而改变原生结构行为。

## Symbol.asyncIterator
> 一个方法，该方法返回对象默认的 AsyncIterator。由 for-await-of 语句使用

## Symbol.hasInstance
> 一个方法，该方法决定一个构造器对象是否认可一个对象是它的实例。由 instanceof 操作符使用

在 ES6 中，instanceof 操作符会使用 Symbol.hasInstance 函数来确定关系，它定义在 Function 的原型上，因此可以在类的静态方法中定义
```javascript
class Foo {
 static [Symbol.hasInstance]() {
  return false;
 }
}

let foo = new Foo();
console.log(foo instanceof Foo); // false
```

## Symbol.isConcatSpreadable
> 一个布尔值，如果是 true，则意味着对象应该用 Array.prototype.concat()打平其数组元素
```javascript
let foo = ['foo'];
let bar = ['bar'];

bar[Symbol.isConcatSpreadable] = false;
console.log(foo.concat(bar)); // ['foo', Array(1)]
```

## Symbol.iterator
> 一个方法，该方法返回对象默认的迭代器。由 for-of 语句使用

## Symbol.match
> 一个正则表达式方法，该方法用正则表达式去匹配字符串。由 String.prototype.match()方法使用
```javascript
console.log('foobar'.match(/bar/)); // ["bar", index: 3, input: "foobar", groups: undefined]

class StringMatcher {
  constructor(str) {
    this.str = str;
  }

  /**
   * 重新定义 Symbol.match 函数，从而让 match() 方法可以使用非正则表达式实例
   * @param {String} target 调用 match() 方法的字符串
   */
  [Symbol.match](target) {
    return target.includes(this.str);
  }
}
console.log('foobar'.match(new StringMatcher('bar'))); // true 
```

## Symbol.replace
> 一个正则表达式方法，该方法替换一个字符串中匹配的子串。由 String.prototype.replace()方法使用
```javascript
class StringReplacer {
  constructor(str) {
    this.str = str;
  }

  /**
   * 重新定义 Symbol.replace 函数，从而让 replace() 方法可以使用非正则表达式实例
   * @param {String} target 调用 replace() 方法的字符串
   * @param {String} replacement 被替换后的值
   */
  [Symbol.replace](target, replacement) {
    return target.split(this.str).join(replacement);
  }
}
console.log('barfoobaz'.replace(new StringReplacer('foo'), 'qux')); // "barquxbaz"
```

## Symbol.search
> 一个正则表达式方法，该方法返回字符串中匹配正则表达式的索引。由 String.prototype.search()方法使用
```javascript
class StringSearcher {
  constructor(str) {
    this.str = str;
  }

  /**
   * 重新定义 Symbol.search 函数，从而让 search() 方法可以使用非正则表达式实例
   * @param {String} target 调用 search() 方法的字符串
   */
  [Symbol.search](target) {
    return target.indexOf(this.str);
  }
}
console.log('foobar'.search(new StringSearcher('foo'))); // 0
```

## Symbol.species
> 一个函数值，该函数作为创建派生对象的构造函数

它的意义在于，子类返回基类的实例，而非子类的实例
```javascript
class Foo extends Promise {}

class Bar extends Promise {
  // 指向基类 Promise 的实例
  static get [Symbol.species]() {
    return Promise;
  }
}

console.log(new Foo(r => r()).then(v => v) instanceof Foo) // true
console.log(new Bar(r => r()).then(v => v) instanceof Bar) // false
```

## Symbol.split
> 一个正则表达式方法，该方法在匹配正则表达式的索引位置拆分字符串。由 String.prototype.split()方法使用
```javascript
class StringSplitter {
  constructor(str) {
    this.str = str;
  }

  /**
   * 重新定义 Symbol.split 函数，从而让 split() 方法可以使用非正则表达式实例
   * @param {String} target 调用 split() 方法的字符串
   */
  [Symbol.split](target) {
    return target.split(this.str);
  }
}
console.log('barfoobaz'.split(new StringSplitter('foo'))); // ["bar", "baz"]
```

## Symbol.toPrimitive
> 一个方法，该方法将对象转换为相应的原始值。由 ToPrimitive 抽象操作使用

很多内置操作的场景下，会将对象强制转换为原始值，这时候我们可以使用该内置符号改变默认行为
```javascript
class Bar { 
  constructor() {
    // 支持三种模式（string、number、default）
    this[Symbol.toPrimitive] = function(hint) {
      switch (hint) {
        // 该场合需转换成数值
        case 'number':
          return 3;
        // 该场合需转换成字符串
        case 'string':
          return 'string bar';
        // 该场合可以转成数值，也可以转成字符串
        case 'default':
        default:
          return 'default bar';
      }
    }
  }
}

let bar = new Bar();
console.log(bar * 2); // 6
console.log(bar + 2); // default bar2
console.log(String(bar)); // string bar
console.log(bar == 'default bar'); // true
```

## Symbol.toStringTag
> 一个字符串，该字符串用于创建对象的默认字符串描述。由内置方法 Object.prototype.toString()使用

```javascript
class Foo {}
const foo = new Foo();
console.log(foo.toString()); // [object Object]

class Bar {
  constructor() {
    this[Symbol.toStringTag] = 'Bar';
  }
}

const bar = new Bar();
console.log(bar.toString()); // [object Bar]
```

## Symbol.unscopables
> 一个对象，该对象所有的以及继承的属性，都会从关联对象的 with 环境绑定中排除

不推荐使用 with，因此也不推荐使用 Symbol.unscopables