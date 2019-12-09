---
layout: post
title: "Typescript 学习笔记 (一)"
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

总结: ES6 的 let 让 js 真正拥有了块级作用域，也是向这更安全更规范的路走，虽然加了很多约束，但是都是为了更安全的使用。

详细可参考 --- [<ES6-let和const命令>](http://es6.ruanyifeng.com/#docs/let#let-%E5%91%BD%E4%BB%A4).

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

### 接口
`接口（Interfaces）` 是对行为的抽象，而具体如何行动需要由`类（classes）` 去`实现（implement）`。

`TypeScript` 中的接口是一个非常灵活的概念，除了可用于对类的`一部分行为`进行抽象以外，也常用于对`「对象的形状（Shape）」`进行描述。

接口一般`首字母大写`, 建议接口的名称加上 `I` 前缀。

```ts
interface IPerson {
    name: string;
    age: number;
}

let tom: IPerson = {
    name: 'Tom',
    age: 25
};
```

赋值的时候，变量的形状必须和接口的`形状`保持一致, 少一些属性或多一些属性都是不允许的.

如果不需要完全匹配一个形状，那么可以用可选属性, 但任然不能多一些属性.
```ts
interface Person {
    name: string;
    age?: number;
}
```

如果我们希望一个接口允许有任意的属性，可以使用`任意属性`:
```ts
interface Person {
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male'
};
```
注意: 一旦定义了任意属性，那么确定属性和可选属性的类型`都必须是它的类型的子集`.

如果希望对象中的一些字段只能在创建的时候被赋值，那么可以用 `readonly` 定义只读属性.

```ts
readonly id: number;
```

### 数组
```ts
let list: any[] = ['xcatliu', 25, { website: 'http://xcatliu.com' }];
```

注意:[类数组](https://ts.xcatliu.com/basics/type-of-array#lei-shu-zu)

### 函数

```ts
function sum(x: number, y: number): number {
    return x + y;
}
// 注意，输入多余的（或者少于要求的）参数，是不被允许的：
```

使用接口的方式来定义一个函数需要符合的形状
```ts
interface SearchFunc {
    (source: string, subString: string): boolean;
}
​
let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

注意: 可选参数必须接在必需参数后面。

使用 `...rest` 的方式获取函数中的剩余参数
```ts
function push(array, ...items) {
    items.forEach(function(item) {
        array.push(item);
    });
}
​
let a = [];
push(a, 1, 2, 3);
```

使用重载定义多个 reverse 的函数类型
```ts
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```