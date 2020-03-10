---
layout: post
title: "面向 2020 春招面试题(五)"
subtitle: "———— 算法部分"
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-12-4/th.jpg"
hidden: true
tags:
  - 春招面试题
---

## [String] 557. 反转字符串中的单词 III

### 题目

给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

**示例 1:**

```
输入: "Let's take LeetCode contest"
输出: "s'teL ekat edoCteeL tsetnoc" 
```

**注意 : ** 在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格。

### 思路

1. 将句子中每个单词拆分
2. 逐词反转
3. 最后拼接起来

### 完整代码

```js
var reverseWords = str => { // 接收传入的字符串
  // 1. 将接收的字符串按照空格拆分, "asd zxc qwe" => ["asd", "zxc", "qwe"]
  var arr = str.split(' ')
  var res = []
  // 2. 遍历拆分后的每个单词
  arr.forEach(item => {
    var reverseW = item.split('').reverse().join('')	// 将 item 先拆分, 翻转, 合并.
    res.push(reverseW)	// 存入预先创建的数组中
  })
  return res.join(' ')
}

reverseWords('asd zxc qwe')		//dsa cxz ewq
```

其实功能已经实现了, 我们应该考虑下代码美观的问题了.

优化后的代码:

```js
var reverseWords = str => {
  // 这里用 map 是为了他可以将得到的结果直接返回原先的数组中, 不需要再创建一个数组保存结果
  return str.split(' ').map(item => {	
    return item.split('').reverse().join('')	// 将 item 先拆分, 翻转, 合并.
  }).join(' ')	// 返回字符串
}
```

### 提交结果

优化前的代码:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200307162530.png)

优化后的代码:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200307162940.png)

可能少一个接收结果的数组, 可以节省一些运行的时间.

## [Array] 17. 电话号码的字母组合

### 题目

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

<img src="https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200307115109.png" style="zoom: 50%;" />

**示例:**

```
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

### 思路:

将输入的数字拆分, 关联对应的字母, 利用数组的循环逐次拼接, 最终生成完整的数组.

*![](https://pic.leetcode-cn.com/50eb7e8eb7b293f70f5c8dab515710a02e5fb0bfc6985dd4de75a19167de291f-file_1583552558917)*



### 具体步骤

**1.** 定义数组 **`map`** 存放数字对应的字母

**2.** 接收传入字符串, 利用 **`split()`** 函数拆分

**3.** 利用 **`forEach()`** 函数将字符串数组与 **`map`** 数组关联如: **`["2", "3", "4"] => ["abc", "def", 'ghi', 'jkl']`** 

**4.** 使用上一步生的数组, 每次利用这个数组的第0位与第1位进行组合, 组合生成的结果 **`splice()`** 返回数组, 放在第0位上, 再与后面一位进行组合, 直到该数组的长度为 1 时, 返回第 0 位即可.

**5.** 对输入 "" 或 个位数(如"1", "2", "3"), 进行兼容.

**6.** 提交代码

### 完整代码

```js
/**
 * @param {string} digits 输入的字符串
 * @return {string[]}
 */
var letterCombinations = function(digits) {
  // 定义数组 `map` 存放数字对应的字母
  let map = ['', 1, 'abc', 'def', 'ghi', 'jkl', 'mno', 'pqrs', 'tuv', 'wxyz']
  // 兼容输入的是 ""
  if(digits === '') {
    return []
  }
  // 将输入的号码进行分隔, 234 => [2,3,4]
  let num = digits.split('')
  // 保存输入号码对应的字母, ["2", "3", "4"] => ["abc", "def", 'ghi', 'jkl']
  let code = []
  num.forEach(item => {
    if(map[item]) {
      code.push(map[item])
    }
  });
  // 兼容输入的是个位数(如"1", "2", "3")
  if(code.length === 1) {
    var temp = []
    for(let i = 0; i < code[0].length; i ++) {
      temp.push(code[0][i])
    }
    return temp
  }
  let comb = arr => {
    var temp = []
    for(let i = 0; i < arr[0].length; i ++) {
      for(let k = 0; k < arr[1].length; k ++) {
        temp.push(`${arr[0][i]}${arr[1][k]}`) // 这里使用了模板字符串
      }
    }
    arr.splice(0, 2, temp)
    if(arr.length > 1) {
      comb(arr)
    }
    return arr[0]
  }
  return comb(code)
};
```



### 提交结果

*![](https://pic.leetcode-cn.com/0f04efbf5d61b769e8814d49863692e9dea5be73b3db44e73fbd67d9cd28d3bc-file_1583552558911)*

