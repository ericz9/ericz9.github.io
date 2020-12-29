---
title: JavaScript高级程序设计学习笔记 - 集合引用类型(Array)
date: 2020-12-29 08:34:50
tags: [红宝书, 红宝书第6章（集合引用类）]
---
尽量使用字面量的方式创建数组

# 创建数组
```js
let arr1 = new Array(3); // [undefined, undefined, undefined]
let arr2 = [3]; // [3]

const obj = {
  '0': 'a',
  '1': 'b',
  length: 2
}
// 将类数组结构转换为数组实例
// 它也可以转换只有 length 属性但不具备 iterator的结构，而 ... 拓展运算符做不到这一点
let arr3 = Array.from(obj); // ["a", "b"]
let arr4 = [...obj]; // obj is not iterable

// 把一组参数转换为数组
let arr5 = Array.of('a', 'b'); // ["a", "b"]
```

# 检测数组
```js
Array.isArray([]); // true
```

# 迭代器方法
```js
const arr = ["foo", "bar", "baz", "qux"];
console.log([...arr.keys()]); // [0, 1, 2, 3]
console.log([...arr.values()]); // ["foo", "bar", "baz", "qux"]
console.log([...arr.entries()]); // [Array(2), Array(2), Array(2), Array(2)]
for (const [idx, element] of arr.entries()) { 
  console.log(idx); 
  console.log(element); 
}
// 0
// foo
// 1
// bar
// 2
// baz
// 3
// qux
```

# 复制与填充
fill 向数组中插入指定值
```js
const zeroes = [0, 0, 0, 0, 0];

// 用 5 填充整个数组
console.log(zeroes.fill(5)); // [5, 5, 5, 5, 5]
zeroes.fill(0); // 重置

// 用 6 填充索引大于等于 3 的元素
console.log(zeroes.fill(6, 3)); // [0, 0, 0, 6, 6]
zeroes.fill(0); // 重置

// 用 7 填充索引大于等于 1 且小于 3 的元素
console.log(zeroes.fill(7, 1, 3)); // [0, 7, 7, 0, 0]
```

copyWithin 按照指定范围浅复制数组中的部分内容，然后将它们插入到指定索引开始的位置

# 栈方法与队列方法
栈是 LIFO 结构（后进先出）
* push：添加到数组末尾，返回最新长度
* pop：移除数组最后一项，返回被移除项

队列是 FIFO 结构（先进先出）
* unshift：添加到数组开头，返回最新长度
* shift：移除数组第一项，返回被移除项

# 排序
* reverse()：降序（用的很少）
* sort()：默认升序。排序时，对每一项使用 String() 转型函数，然后比较字符串来决定顺序
  * sort((a, b) => a - b)：升序
  * sort((a, b) => b - a)：降序

# 操作方法
* concat：合并数组，返回新数组
* slice：切分数组，返回新数组。不包含结束索引的元素
* splice：插入、删除或替换数组，返回删除的元素组成的数组，会修改原数组

# 搜素和位置方法
* indexOf：从前向后搜索，返回元素在数组中的索引，未找到返回 -1
* lastIndexOf：从后向前搜索，返回元素在数组中的索引，未找到返回 -1
* includes：是否包含至少一个匹配项，返回布尔值
* find：返回第一个匹配的项
* findIndex：返回第一个匹配项的索引

# 迭代方法
对数组的每一项，都运行传入的函数
* every：如果都返回 true，则返回 true
* filter：返回结果为 true 的数组
* forEach：没有返回值
* map：返回每次函数调用所返回的结果的数组
* some：某一项返回 true，则返回 true

# 归并方法
* reduce：从第一项开始，遍历到最后一项
* reduceRight：从最后一项开始，遍历到第一项
归并方法接收两个参数：对每一项都会运行的归并函数，以及可选的以之为归并起点的初始值
```js
let values = [1, 2, 3, 4, 5];

// prev：上一个归并值
// cur：当前项
// index：当前项的索引
// array：和数组本身
let sum = values.reduce(function(prev, cur, index, array) {
  console.log(prev, cur);
  return prev + cur;
}, 100);
// 100 1
// 101 2
// 103 3
// 106 4
// 110 5

console.log(sum); // 115
```