---
layout: post
title: "面向 2020 春招面试题(四)"
subtitle: "———— 数据结构 & 算法部分"
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-12-4/th.jpg"
hidden: true
catalog: true
tags:
  - 春招面试题
---

## 栈结构

### 什么是栈结构?

栈 ( Stack ), 是一种运算受限的线性表, 遵循后进先出 `LIFO(Last In First Out)` 

- 其限制是仅允许在表的一端进行插入和删除运算。这一端被称为栈顶，相对地，把另一端称为栈底。

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200303214041.png)

<center>栈的图示</center>
### 栈结构在编程中的使用

- 函数调用栈

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200303214724.png)

### 模拟栈的实现

我们使用`数组`模拟栈的实现, 实现以下几点功能 :

- `push(element)`:  添加一个新元素到栈顶位置.

- `pop()`：移除栈顶的元素，同时返回被移除的元素。

- `peek()`：返回栈顶的元素，不对栈做任何修改。

- `isEmpty()`：如果栈里没有任何元素就返回`true`，否则返回`false`。

- `clear()`：移除栈里的所有元素。

- `size()`：返回栈里的元素个数。这个方法和数组的`length`属性很类似。  

**完整代码:**

```js
/*
 * @Author: Ivens
 * @Date: 2020-02-29 21:37:41
 * @LastEditTime: 2020-03-03 21:49:42
 * @LastEditors: Please set LastEditors
 * @Description: 用数组实现栈结构
 * @FilePath: \imooc-algorithm-master\code\coderwhy\stack\stackImplement.js
 */
class Stack {
  item = []

  push(ele) {
    this.item.push(ele)
  }
  pop() {
    this.item.pop()
  }
  peek() {
    return this.item[this.item.length - 1]
  }
  isEmpty() {
    return this.item.length === 0 ? true : false
  }
  size() {
    return this.item.length
  }
  toString() {
    return this.item.reverse().join(' ')
  }
}

let s1 = new Stack()
s1.push('a')
s1.push('b')
s1.push('c')
console.log(s1.toString());		// a b c
```

### 栈的应用

下面我们来学习下如何使用`Stack`类，解决一些计算机科学中的问题 :

- **十进制转二进制**

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200303222252.png)

<center>转换原理</center>
**完整代码:**

```js
// 接上文实现栈结构的代码
let reslut = new Stack()
function transform (num) {
  // 如果传入的是正数, 进入这里
    if (num <= 1) {
      reslut.push(num)
      return reslut.toString()
    }
    reslut.push(num % 2)
    return transform(Math.floor(num / 2))
} 
console.log(transform(100));		// 1 1 0 0 1 0 0
// !现在只能转换正整数, 没法转换负数和小数, 希望以后解决
```



## 队列结构

### 什么是队列结构?

队列( Queue ), 他是一种受限的线性表, 遵循先进先出`FIFO(First In First Out)`的原则.

- 受限之处在于它只允许在表的前端（front）进行删除操作，而在表的后端（rear）进行插入操作

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200303223353.png)

### 队列的实现

我们使用`数组`来模拟实现队列结构, 实现以下功能 :

- `enqueue(element)`：向队列尾部添加一个（或多个）新的项。

- `dequeue()`：移除队列的第一（即排在队列最前面的）项，并返回被移除的元素。

- `front()`：返回队列中第一个元素——即最先被添加的那个元素（与`Stack`类的`peek`方法非常类似）

- `isEmpty()`：如果队列中不包含任何元素，返回`true`，否则返回`false`。

- `size()`：返回队列包含的元素个数，与数组的`length`属性类似。 

**完整代码:**

```js
/*
 * @Author: Ivens
 * @Date: 2020-03-02 11:06:50
 * @LastEditTime: 2020-03-02 12:29:44
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \imooc-algorithm-master\code\coderwhy\queue\queueImplement.js
 */

class Queue {
  item = []

	// 进入队列
  enqueue(ele) {
    this.item.push(ele)
  }
	// 弹出队列中第一个元素
  dequeue() {
    this.item.shift()
  }
  front() {
    return this.item[0]
  }
  isEmpty() {
    return this.item.length === 0
  }
  size() {
    return this.item.length
  }
  toString() {
    return this.item.join(' ')
  }
}

let q1 = new Queue()
q1.enqueue('a')
q1.enqueue('b')
q1.enqueue('c')
console.log(q1.toString());		// a b c
```

### 优先级队列的实现

我们在原本每个只携带数据的元素上, **增加一个优先级属性**, 当元素进入队列中, 按照优先级顺序排列.

```js
item(data, priority)
```

**完整代码:**

```js
/*
 * @Author: ivens
 * @Date: 2020-03-02 18:53:50
 * @LastEditTime: 2020-03-02 20:58:24
 * @LastEditors: Please set LastEditors
 * @Description: 优先级队列
 * @FilePath: \imooc-algorithm-master\code\coderwhy\queue\priorityQueue.js
 */
class PriorityQueue {
  item = []
  
  /**
   * 根据传入优先级遍历找到插入的位置, 利用数组的 splice() 方法将元素插入
   * @param {String} ele 传入的对象
   * @param {Number} prior 优先级
   */
  enqueue(ele, prior) {
    var obj = {}
    obj.ele = ele
    obj.prior = prior
    if(this.item.length === 0) {
      this.item.push(obj)
    }
    for(let i = 0; i < this.item.length; i ++) {
      if(this.item[i].prior < obj.prior) {
        this.item.splice(i+1,0,obj)
      }
    }
  }
  isEmpty() {
    return this.item.length === 0
  }
  toString() {
    var str = ''
    this.item.forEach(item => {
      str += item.ele + '..' + item.prior + '  '
    })
    return str
  }
}

var pq1 = new PriorityQueue()
pq1.enqueue('a',1)
pq1.enqueue('b',10)
pq1.enqueue('c',2)
console.log(pq1.toString());	// a..1  c..2  b..10
```

### 队列的应用 —— 击鼓传花

<img src="https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200304182321.png" style="zoom: 67%;" />

<center>击鼓传花图解</center>

**解决思路：**

击鼓传花，事先约定每次击多少下鼓，利用这个特性我们可以使用队列结构来解决问题。

1. **将所有成员依次压入队列**

<img src="https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/IMG_20200304_183746.jpg" style="zoom:25%;" />

2. **依次从头拿出成员再在尾端压入队列，轮到被淘汰的成员从首部弹出不再压入队列即可。**

<img src="https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/IMG_20200304_184052.jpg" style="zoom:25%;" />

**完整代码:**

```js
// 接上文普通队列实现代码

/**
 * 
 * @param {Array} nameList 击鼓传花名单列表
 * @param {Number} num 每次击鼓次数
 * @return {Object} 返回队列中唯一剩余的对象
 */
function passGame(nameList, num) {
  let nameQueue = new Queue()
  nameList.forEach(item => {
    nameQueue.enqueue(item)
  })

  // *思路: 从队列头弹出 num-1 个对象从队列尾部压入, 弹出第 num 个对象, 循环这个动作直至队列的 size() 等于 1. 返回该对象即可.
  while (nameQueue.size() > 1) {
    for (let i = 0; i < num - 1; i++) {
      nameQueue.enqueue(nameQueue.dequeue())
    }
    nameQueue.dequeue()
  }

  return nameQueue.front()	// 返回唯一剩余的对象
}

let stus = ['tom', 'jack', 'david', 'lili', 'nancy', 'john']
let result = passGame(stus, 3)
console.log(result);	// tom
```

## 单向链表

### 什么是单向链表?

链表的数据结构:

<img src="https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200304215105.png" style="zoom: 67%;" />

形象的表示:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200304215130.png)

### 封装一个单向链表

```js
// 链节点类
class Node {
  constructor(item) {
    this.item = item
    this.next = null
  }
}

// 链表类
class LinkedList {
  header = new Node('header')	// 实例化 header 为一个链节点
  length = 0	// 记录链表的长度
}
```

**实现以下功能 :**

- `append(element)`：向列表尾部添加一个新的项

- `insert(element, position)`：向列表的特定位置插入一个新的项。

- `remove(element)`：从列表中移除一项。

- `indexOf(element)`：返回元素在列表中的索引。如果列表中没有该元素则返回`-1`。

- `removeAt(position)`：从列表的特定位置移除一项。

- `isEmpty()`：如果链表中不包含任何元素，返回`true`，如果链表长度大于0则返回`false`。

- `size()`：返回链表包含的元素个数。与数组的`length`属性类似。

- `toString()`：由于列表项使用了`Node`类，就需要重写继承自JavaScript对象默认的`toString`方法，让其只输出元素的值。

**完整代码 :**

```js
/*
 * @Author: ivens
 * @Date: 2020-03-02 21:26:27
 * @LastEditTime: 2020-03-03 15:13:19
 * @LastEditors: Please set LastEditors
 * @Description: 单向链表
 * @FilePath: \imooc-algorithm-master\code\coderwhy\linkedList\linkedListImp.js
 */

class Node {
  constructor(item) {
    this.item = item
    this.next = null
  }
}

class LinkedList {
  header = new Node('header')	// 实例化 header 为一个链节点
  length = 0

	// 在末尾新增一个节点
  append(item) {
    var newNode = new Node(item)	// 将传入的数据实例化为一个链节点
    // 利用只有链尾节点的 next 属性为 null 的特性, 放入 while 循环中直到 current 指向当前链尾节点
    var current = this.header
    while(current.next) {
      current = current.next
    }
    current.next = newNode		// 让当前链尾节点的 next 属性指向要新增的链节点
    this.length ++						// 链表长度 +1
  }

  insert(item, position) {
    var insertNode = new Node(item)		// 将传入的数据实例化为一个链节点
    
    if (position < 1) {		// 如果传入位置小于 1, 无法实现, 即数据不合法
      return console.log(`position is illegal`);
    }
    
    if (position > this.length + 1) {	// 传入位置超过合理范围
      return console.log(`position is to big`);
    }

    if (position === 1) {		// 如果传入位置为 1, 即 header 后第一个
      var temp = this.header.next			// 暂存之前 header.next 指向的节点
      this.header.next = insertNode		// 让 header.next 指向新增节点
      insertNode.next = temp					// 让新增节点的 next 指向暂存的节点
    }

    if(position > 1) {
      // 通过循环找到要插入位置的前一个节点, 重复上面的操作
      var current = this.header
      for(let i = 0; i < position - 1; i ++) {
        current = current.next
      }
      var temp = current.next
      current.next = insertNode
      insertNode.next = temp
    }

    this.length ++
  }

  toString() {
    var current = this.header.next
    var str = ''
    while(current) {
      str += `${current.item}  `
      current = current.next
    }
    return str
  }

  size() {
    return this.length
  }
}

var l1 = new LinkedList()
l1.append('a')
l1.append('b')
l1.append('c')
l1.insert('x',2)
console.log(l1.toString());		// res => a  x  b  c
console.log(l1.size()); 			// res => 4
```

## 双向链表

### 什么是双向链表?

**单向链表有一个比较明显的缺点 :**

- 我们可以轻松的到达下一个节点, 但是回到前一个节点是很难.  在实际开发中, 经常会遇到需要回到上一个节点的情况.
- 举个例子: 假设一个文本编辑用链表来存储文本. 每一行用一个 `String` 对象存储在链表的一个节点中. 当编辑器用户向下移动光标时, 链表直接操作到下一个节点即可. 但是当用于将光标向上移动呢? 这个时候为了回到上一个节点, 我们可能需要从first开始, 依次走到想要的节点上.

双向链表可以有效的解决单向链表中提到的问题. 只需要一个节点既有向前连接的引用, 也有一个向后连接的引用就行了.

**双向链表有什么缺点呢?**

1. 每次在插入或删除某个节点时, 需要处理四个节点的引用, 而不是两个. 也就是**实现起来要困难一些**

2. 并且相当于单向链表, 必然**占用内存空间更大一些**

但是这些缺点和我们使用起来的方便程度相比, 是微不足道的.

<img src="https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200304222333.png" style="zoom: 67%;" />

<center>双向链表图解</center>

### 封装双向链表

```js
// 双向链表中的节点类
class Node {
  constructor(item) {
    this.pre = null
    this.item = item
    this.next = null
  }
}
// 双向链表类
class TwodicLinkedList {
  header = new Node('header')		// 为 header 实例化一个节点
  length = 0
}
```

<img src="https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200304223505.png" style="zoom:67%;" />

<center>双向链表的 append 方法图解</center>

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200304223617.png)

<center>双向链表的 insert 方法图解</center>

**完整代码 :**

```js
/*
 * @Author: your name
 * @Date: 2020-03-03 15:20:02
 * @LastEditTime : 2020-03-04 22:31:02
 * @LastEditors  : Ivens
 * @Description: 双向链表
 * @FilePath     : \imooc-algorithm-master\code\coderwhy\linkedList\twodicLinkedListImp.js
 */
// 双向链表中的节点类
class Node {
  constructor(item) {
    this.pre = null
    this.item = item
    this.next = null
  }
}
// 双向链表类
class TwodicLinkedList {
  header = new Node('header')
  length = 0
  /**
   * 在尾部新增节点
   * @param {Object / String} item 新增节点包含的数据
   */
  append(item) {
    var current = this.header
    var appendNode = new Node(item)
    while (current.next) {
      current = current.next
    }
    current.next = appendNode		// 让前一个节点的 next 指向新增节点
    appendNode.pre = current		// 让新增节点的 pre 指向前一个节点
    this.length ++
  }

  insert(item, position) {
    var insertNode = new Node(item)
    if (position < 1 || position > this.length + 1) {	// 位置输入不符合范围, 数据不合法
    return console.log(`position is illage`);
    }

    if (position >= 1 && position < this.length) {
      var current = this.header
      for (let i = 0; i < position - 1 ; i ++) {
        current = current.next  
      }
      var temp = current.next			// 暂存之前 next 指向的节点
      current.next = insertNode
      insertNode.pre = current
      insertNode.next = temp
      temp.pre = insertNode
    }

    if (position === this.length + 1) {
      return this.append(item)
    }
    
    this.length ++
  }

  toString () {
    var current = this.header.next
    var str = ''
    while (current) {
      str += `${current.item}  `
      current = current.next
    }
    return console.log(str);
  }

  size() {
    return console.log(this.length);
  }
}

var d1 = new TwodicLinkedList()
d1.append('a')
d1.append('b')
d1.append('c')
d1.insert('x', 2)
d1.toString()			// res => a  x  b  c
d1.size()					// 4
```

## 集合结构

集合通常是由一组`无序`的, `不能重复`的元素构成

*2015年6月份发布的ES6中包含了`Set`类, 所以其实我们可以不封装, 直接使用它.*

### 用对象封装一个简单的集合

```js
class ArrayList {
  items = {}
}
```

**要实现的方法 :**

- `add(value)`：向集合添加一个新的项。
- `remove(value)`：从集合移除一个值。
- `has(value)`：如果值在集合中，返回`true`，否则返回`false`。
- `clear()`：移除集合中的所有项。
- `size()`：返回集合所包含元素的数量。与数组的length属性类似。
- `values()`：返回一个包含集合中所有值的数组。

**完整代码 :**

```js
/*
 * @Author: your name
 * @Date: 2020-03-03 15:59:12
 * @LastEditTime: 2020-03-03 18:52:48
 * @LastEditors: Please set LastEditors
 * @Description: 集合实现
 * @FilePath: \imooc-algorithm-master\code\coderwhy\set\setImp.js
 */
class ArrayList {
  items = {}

	// 增
  add(value) {
    if (this.has(value) || value === '') {
      return console.log(`元素不合法 !`);
    }

    this.items[value] = value
    return true
  }

	// 删
  remove(value) {
    if (this.has(value)) {
      delete this.items[value]
      return true
    }
  }

  clear() {
    this.items = {}
  }

  has(value) {
    return this.items.hasOwnProperty(value)
  }

	// 转换为数组
  values() {
    var arr = []
    Object.keys(this.items).forEach(index => {
      arr.push(index)
    })
    return arr
  }

  toString() {
    var str = ''
    Object.keys(this.items).forEach(index => {
      str += `${index}  `
    })
    return console.log(str);
  }
}

var a1 = new ArrayList()
a1.add('a')
a1.add('b')
a1.add('c')
a1.toString()		// a  b  c
```

### 实现集合间的操作: 并集, 交集, 差集, 子集









































