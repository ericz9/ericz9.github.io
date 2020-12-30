---
title: JavaScript高级程序设计学习笔记-集合引用类型(Map与WeakMap)
date: 2020-12-30 09:20:36
tags: [红宝书, 红宝书第6章（集合引用类）]
---
Map 和 WeakMap 是真正的键值机制，它们的大多数特性也可以通过 Object 来实现。

# Map
```js
// 创建空映射
const m1 = new Map();

// 使用二维数组初始化
const m2 = new Map([
  ['key1', 'val1'],
  ['key2', 'val2']
]);

// 使用自定义迭代器初始化
const m3 = new Map({
  *[Symbol.iterator]() {
    yield ['key1', 'val1'];
    yield ['key2', 'val2'];
  }
});
```

## 常用属性及方法
* `size`：返回键值对的总数量
* `set()`：添加一组新的键值对。返回映射实例，因此可以连接多个操作
* `get()`：通过键名，获取对应的值
* `has()`：通过键名查询，存在时返回 true
* `delete()`：通过键名，删除一个键值对
* `clear()`：清空所有键值对

## 顺序与迭代
与 Object 不同的是，Map 会维护键值对的插入顺序
```js
const foo = new Map([
  ['key1', 'val1'],
  ['key2', 'val2']
]);
console.log(foo.keys()); // MapIterator {"key1", "key2"}
console.log(foo.values()); // MapIterator {"val1", "val2"}
console.log(foo.entries()); // MapIterator {"key1" => "val1", "key2" => "val2"}
```

## 选择 Object 还是 Map
* 内存占用：给定固定大小的内存，Map 大约可以比 Object 多存储50%的键值对
* 插入性能：Map 一般会稍微快一点
* 查找速度：差异极小，如果有大量查找操作，某些情况下 Object 更好一些
* 删除性能：一般来说 Map 更快，特别是涉及到大量删除操作的情况下

# WeakMap
WeakMap（弱映射）是 Map 的“兄弟”类型，“weak”表示它的键是“弱弱地拿着”。意思就是，这些键不属于正式的引用，不会阻止垃圾回收。不过，值的引用可不是“弱弱地拿着”，只要键存在，它的值就不会被垃圾回收。

我们知道，只要对象还存在引用，就不会被垃圾回收，不过 WeakMap “打破了这个惯例”。

我们假设在一个 WeakMap 中，有一对键值对的键为 key，值为 val。当 key 的引用只指向该键，没有被其他任何对象所引用时，因为键不属于正式的引用，所以它会被垃圾回收。而当 val 的引用只指向该值，没有被其他任何对象所引用时，因为值的引用是正式的引用，所以它就不会被垃圾回收。

**基本上，如果你要往对象上添加数据，又不想干扰垃圾回收机制，就可以使用 WeakMap。**

## 与 Map 的不同点
* 键只能是 Object 或继承在 Object 的类型，使用非对象键会抛出 TypeError。值的类型没有限制
* 没有 size 属性
* 没有 clear() 方法
* 不可迭代

## 应用场景
```js
// 私有变量
const wm = new WeakMap();
class Foo {
  constructor() {
    wm.set(this, {
      name: 'ericz9'
    });
  }

  getName() {
    return wm.get(this).name;
  }
}

const foo = new Foo();
console.log(foo.getName()); // ericz9
```