---
layout: post
title: "Typescript 学习笔记————基础"
subtitle: ""
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-11-30/th.jpg"
hidden: true
catalog: true
tags:
  - 前端学习笔记
---
## 前言
`let` 与 `var` 的区别?
- 不存在变量提升
- 不允许重复声明
- 为 JS 新增了块级作用域

**总结: ES6 的 let 让 js 真正拥有了块级作用域，也是向这更安全更规范的路走，虽然加了很多约束，但是都是为了更安全的使用。**

详细可参考 --- [《ES6-let和const命令》](http://es6.ruanyifeng.com/#docs/let#let-%E5%91%BD%E4%BB%A4).

## 简介
### 什么是 TypeScript ?
TypeScript 是 JavaScript 的一个超集，主要提供了类型系统和对 ES6 的支持.

### TypeScript 的优点

1. TypeScript 增加了代码的可读性和可维护性
2. TypeScript 非常包容，`.js `文件可以直接重命名为 `.ts `即可
3. TypeScript 拥有活跃的社区

### TypeScript 的安装
1. `Ctrl + R` 打开 `cmd` 输入:

```cmd
npm install -g typescript
```

2. 编译 TypeScript 文件

```cmd
tsc hello.ts
```

## 基础
![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191210161237.png)

### 原始数据类型
JavaScript 的类型分为两种：`原始数据类型（Primitive data types）`和`对象类型（Object types）`。

原始数据类型包括：`布尔值`、`数值`、`字符串`、`null`、`undefined` 以及 ES6 中的新类型 `Symbol`。

#### 布尔值
```ts
let isDone: boolean = false;
```
注意，使用构造函数 Boolean 创造的对象不是布尔值, 返回的是一个 Boolean 对象.

#### 数值
```ts
let a: number = 6;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```

#### 字符串
```ts
let myName: string = 'Tom';
```

模板字符串:
```ts
let sentence: string = `Hello, my name is ${myName}.`;
```

编译结果:
```js
var sentence = "Hello, my name is " + myName + ".";
```

#### 空值
在 TypeScript 中，可以用 void 表示没有任何返回值的函数, JavaScript 没有空值（Void）的概念.

```ts
function alertName(): void {
    alert('My name is Tom');
    // 使用 void 则函数内就不可以有 return.
}

// 声明一个 void 类型的变量没有什么用，因为你只能将它赋值为 undefined 和 null
let unusable: void = undefined;
```

#### Null 和 Undefined
可以使用 null 和 undefined 来定义这两个原始数据类型
```ts
let u: undefined = undefined;
let n: null = null;
```

`undefined` 和 `null` 是所有类型的子类型.
```ts
let a: undefined;
let b: string = a;

// 而 void 类型的变量不能赋值给 number 类型的变量
let u: void;
let num: number = u;

// Type 'void' is not assignable to type 'number'.
```

***

### 任意值
在 TypeScript 中, 是不允许在赋值过程中改变类型的.

```ts
let a: string;
a = 2;
// Type '2' is not assignable to type 'string'.
```

但如果是 `any` 类型，则允许被赋值为任意类型。

任意值上访问`任何属性`都是允许的, 也允许调用`任何方法`. 可以认为，声明一个变量为任意值之后，对它的任何操作，返回的内容的类型都是任意值.

注意: 变量如果在声明的时候，`未指定`其类型，那么它会被识别为任意值类型.

### 类型推论

`TypeScript` 会在没有明确的指定类型的时候推测出一个类型，这就是类型推论。
```ts
let a = 'a';
a = 2;
// Type '2' is not assignable to type 'string'.
```

如果`定义的时候`没有赋值，不管之后有没有赋值，都会被推断成 `any` 类型而完全不被类型检查.

### 联合类型
```ts
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型.

