---
layout: post
title: "Typescript 学习笔记————进阶"
subtitle: ""
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-11-30/th.jpg"
hidden: true
catalog: true
tags:
  - 前端学习笔记
---


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

### 类型断言

当 `TypeScript` 不确定一个`联合类型`的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里`共有`的属性或方法.

```ts
function getLength(something: string | number): number {
    return something.length;
}
​
// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```

而有时候，我们确实需要在还不确定类型的时候就访问其中一个类型的属性或方法, 此时可以使用`类型断言`，在需要断言的变量前加上 `<Type>` 即可。

**类型断言不是类型转换，断言成一个联合类型中不存在的类型是不允许的.**

### 声明语句
使用第三方库 jQuery，一种常见的方式是在 html 中通过 `<script>` 标签引入 jQuery，然后就可以使用全局变量 $ 或 jQuery 了。

**但是在 ts 中，编译器并不知道 $ 或 jQuery 是什么东西**, 需要使用 `declare var` 来定义它的类型.

```ts
declare const jQuery: (selector: string) => any;
jQuery('#foo');
```

通常我们会把声明语句放到一个单独的文件中，这就是声明文件, 声明文件必需以 `.d.ts` 为后缀, 如: `jQuery.d.ts`.

或直接使用第三方声明文件, 推荐使用 `@types` 统一管理第三方库的声明文件。

```cmd
npm install @types/jquery --save-dev
```

### 内置对象
![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191210155653.png)

我们可以在 TypeScript 中将变量定义为这些类型：
```ts
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;
```


## 类

JavaScript 在 ES6 中，添加了 `class` .

TypeScript 除了实现了所有 ES6 中的类的功能以外，还添加了一些新的用法。

使用 `class` 定义类，使用 `constructor` 定义构造函数。

```ts
class Animal {
  name;
  constructor(name: string) {
    this.name = name;
  }
}
```

或可以直接在构造函数中使用修饰符, 等同于类中定义该属性，使代码更简洁。

```ts
class Animal {
    constructor(public name: string) {
        this.name = name;
    }
}
```

### 存取器
```ts
class Animal {
    constructor(name) {
        this.name = name;
    }
    get name() {
        return 'Jack';
    }
    set name(value) {
        console.log('setter: ' + value);
    }
}

let a = new Animal('Kitty'); // setter: Kitty
a.name = 'Tom'; // setter: Tom
console.log(a.name); // Jack
```

改变 `name` 就会触发 `set` 方法, 获取 `name` 就会触发 `get` 方法.

### 静态方法
使用 static 修饰符修饰的方法称为静态方法，它们不需要实例化，而是直接通过类来调用
```ts
class Animal {
    static isAnimal(a) {
        return a instanceof Animal;
    }
}

let a = new Animal('Jack');
Animal.isAnimal(a); // true
a.isAnimal(a); // TypeError: a.isAnimal is not a function
```

### TypeScript 中的三种访问修饰符
- `public`
- `private`
- `protected`: 在子类中允许被访问

<br>

- 需要注意的是，TypeScript `编译之后`的代码中，`并没有`限制 private 属性在外部的可访问性。
- 当`构造函数`修饰为 `private` 时，该类不允许被继承或者实例化.
- 当构造函数修饰为 `protected` 时，该类只允许`被继承`
- 如果 `readonly` 和`其他访问修饰符`同时存在的话，需要写在其后面
  
### 抽象类

**什么是抽象类？**
- 抽象类是不允许被实例化的
- 抽象类中的抽象方法必须被子类实现

### 接口
接口与接口之间可以是继承关系, 但是实现的子类需要把之前接口中`所有的方法`重写.

**一个类可以实现多个接口**.
```ts
interface Alarm {
}

interface Light {
}

class Car implements Alarm, Light {
}
```
**接口也可以继承类**：
```ts
class Point {
    x: number;
    y: number;
}

interface Point3d extends Point {
    z: number;
}
```