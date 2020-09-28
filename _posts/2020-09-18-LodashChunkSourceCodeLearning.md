---
layout: post
title: "Lodash 中 chunk 函数的源码学习"
subtitle: ""
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-11-30/th.jpg"
tags:
  - Vue.js
---

首先看一下我们的 `_.chunk` 函数的实际效果：

```js
const _ = require('lodash')

let arr = [1, 2, 3, 4, 5, 6, 7]
let res = _.chunk(arr, 2)

console.log(res)	// [ [ 1, 2 ], [ 3, 4 ], [ 5, 6 ], [ 7 ] ]
```

### 官方文档的说明：

```js
_.chunk(array, [size=1])
```

将数组（array）拆分成多个 `size` 长度的区块，并将这些区块组成一个新数组。 如果`array` 无法被分割成全部等长的区块，那么最后剩余的元素将组成一个区块。

#### 参数

1. `array` *(Array)*: 需要处理的数组
2. `[size=1]` *(number)*: 每个数组区块的长度

#### 返回

*(Array)*: 返回一个包含拆分区块的新数组（注：相当于一个二维数组）。

### 正文

使用 `webstorm` `debug` 功能，断点打在 `res` 赋值的那行。

进入 `chunk` 函数内部，看一下它的实现步骤是什么样的。

```js
/**
 * Creates an array of elements split into groups the length of `size`.
 * If `array` can't be split evenly, the final chunk will be the remaining
 * elements.
 *
 * @static
 * @memberOf _
 * @since 3.0.0
 * @category Array
 * @param {Array} array The array to process.
 * @param {number} [size=1] The length of each chunk
 * @param- {Object} [guard] Enables use as an iteratee for methods like `_.map`.
 * @returns {Array} Returns the new array of chunks.
 * @example
 *
 * _.chunk(['a', 'b', 'c', 'd'], 2);
 * // => [['a', 'b'], ['c', 'd']]
 *
 * _.chunk(['a', 'b', 'c', 'd'], 3);
 * // => [['a', 'b', 'c'], ['d']]
 */
function chunk(array, size, guard) {	// arr = [1, 2, 3, 4, 5, 6, 7], size = 2
  // 1、参数兼容性检测，确定 size 值
  if ((guard ? isIterateeCall(array, size, guard) : size === undefined)) {
    size = 1;
  } else {
    size = nativeMax(toInteger(size), 0);
  }
    
  // 2、确定 length 值
  var length = array == null ? 0 : array.length;
    
  // 3、对 length 和 size 做兼容性处理
  if (!length || size < 1) {
    return [];
  }
    
  var index = 0,
      resIndex = 0,
      result = Array(nativeCeil(length / size));	// new 一个确定长度的数组，减小内存开销

  while (index < length) {
    // 利用 baseSlice 函数，为数组赋值
    // 这里关注 baseSlice 第三个参数的传值
    result[resIndex++] = baseSlice(array, index, (index += size));
  }
  return result;
}
```

> 为什么 `JavaScript` 原生 `API` 中已经有 `Array.slice()` 函数了 `lodash` 还自己实现一个 `_.slice` 和 `_.baseSlice` 呢？
>
> 原因：The perf wins of `Array#slice` vs. `baseSlice` depends on the size of the array. That is a minor point though as the perf of that method is not likely to be an issue. The reason we use `baseSlice` is because we treat arrays as dense while `Array#slice` will respect sparse arrays.
>
> 密集数组与稀疏数组的区别，例如：
>
> ```
> var sparse = new Array(10)
> var dense = new Array(10).fill(undefined)
> ```
>
> 其中 `sparse` 的 `length` 为10，但是 `sparse` 数组中没有元素，是稀疏数组；而 `dense` 每个位置都是有元素的，虽然每个元素都为`undefined`，为密集数组 。
>
> 那稀疏数组和密集数组有什么区别呢？在 lodash 中最主要考虑的是两者在迭代器中的表现。
>
> 稀疏数组在迭代的时候会跳过不存在的元素。
>
> ```
> sparse.forEach(function(item){
>   console.log(item)
> })
> dense.forEach(function(item){
>   console.log(item)
> })
> ```
>
> `sparse` 根本不会调用 `console.log` 打印任何东西，但是 `dense` 会打印出10个 `undefined` 。
>
> Reference：
>
> - [why not the 'baseslice' func use Array.slice(), loop faster than slice?](https://github.com/lodash/lodash/issues/2850)
> - [读lodash源码之从slice看稀疏数组与密集数组](https://github.com/yeyuqiudeng/pocket-lodash/issues/1)
> - [Lodash 学习笔记（二）：slice](https://juejin.im/post/6844904190926389261)

```js
/**
 * The base implementation of `_.slice` without an iteratee call guard.
 *
 * @private
 * @param {Array} array The array to slice.
 * @param {number} [start=0] The start position.
 * @param {number} [end=array.length] The end position.
 * @returns {Array} Returns the slice of `array`.
 */
function baseSlice(array, start, end) {
  var index = -1,
      length = array.length;

  // 参数兼容性处理
  if (start < 0) {
    start = -start > length ? 0 : (length + start);
  }
  // Tip：注意这里是如何处理数组剩余的值凑不满 length 的情况。
  end = end > length ? length : end;
  if (end < 0) {
    end += length;
  }
  length = start > end ? 0 : ((end - start) >>> 0);
  // Tips：《js >>> 0 谈谈 js 中的位运算》(https://www.jianshu.com/p/588eb74b5a03)
  // 在这里目前来看只有一个作用——自然数向下取整，类似 Math.floor()
  start >>>= 0;

  var result = Array(length);
  // 利用循环，将原数组做截取，将截取出来片段返回
  while (++index < length) {
    result[index] = array[index + start];
  }
  return result;
}
```

模仿 `_.chunk` 和 `_.baseSlice` 的实现思想，我自己写了个 `demo`，虽然**没做任何参数的兼容性处理**，但是功能是可以实现的：

```js
let arr = [1, 2, 3, 4, 5]

let res1 = chunk(arr, 2)
console.log(res)		// => [ [ 1, 2 ], [ 3, 4 ], [ 5, undefined ] ]
let res2 = chunk(arr, 3)
console.log(res)		// => [ [ 1, 2, 3 ], [ 4, 5, undefined ] ]

function chunk(arr, size) {
    let index = 0
    let resIndex = 0
    let res = []
    while (index < arr.length) {
        res[resIndex++] = baseSlice(arr, index, index += size)
    }
    return res
}

function baseSlice(arr, start, end) {
    let length = end - start
    let index = 0
    let res = []
    while (index < length) {
        res[index] = arr[start + index++]
    }
    return res
}
```

在这段时间的源码学习中，发现一个现象，其实很多函数主要的逻辑其实并不多，大部分的代码量都在提高函数的健壮性上了，如果说这些库函数和普通人写的函数有什么区别？大约是这样的：

1. 兼容性处理
2. 实现方法的性能优化
3. 合理使用语法糖