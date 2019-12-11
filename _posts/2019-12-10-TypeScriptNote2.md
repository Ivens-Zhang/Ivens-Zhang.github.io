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