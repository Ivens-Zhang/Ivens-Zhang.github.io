## aichef-web 项目测试报告

### 一. 概念介绍

**单元测试：**

单元测试是对软件中的基本组成单位进行的测试，如一个模块、一个过程等等。它是软件动态测试的最基本的部分，也是最重要的部分之一，其目的是`检验软件基本组成单位的正确性`。

一个软件单元的正确性是相对于该单元的规约而言的。因此，单元测试以被测试单位的规约为基准。

单元测试的主要方法有:

- 控制流测试
- 数据流测试
- 排错测试
- 分域测试

**集成测试：**

集成测试是在软件系统集成过程中所进行的测试，其主要目的是`检查软件单位之间的接口是否正确`。

它根据集成测试计划，一边将模块或其他软件单位组合成越来越大的系统，一边运行该系统，以分析所组成的系统是否正确，各组成部分是否合拍。集成测试的策略主要有两种:

- 自顶向下
- 自底向上

**系统测试：**

系统测试是对已经集成好的软件系统进行彻底的测试，`以验证软件系统的正确性和性能等满足其规约所指定的要求`，检查软件的行为和输出是否正确并非一项简单的任务，它被称为测试的 “ 先知者问题 ” 。因此，系统测试应该按照测试计划进行，其输入、输出和其他动态运行行为应该与软件规约进行对比。

软件系统测试方法很多，主要有:

- 功能测试
- 性能测试
- 随机测试

**验收测试：**

验收测试旨在向软件的购买者展示该软件系统满足其用户的需求。它的测试数据通常是系统测试的测试数据的子集。所不同的是，验收测试常常有软件系统的购买者代表在现场，甚至是在软件安装使用的现场。这是软件在投入使用之前的`最后测试`。

**回归测试：**

回归测试是在软件维护阶段，对软件进行`修改之后`进行的测试。其目的是检验对软件进行的修改是否正确。

这里，修改的正确性有两重含义：

1. 所作的修改`达到了预定目的`，如错误得到改正，能够适应新的运行环境等等
2. 不影响软件的`其他功能`的正确性



### 二. 单元测试常用技术

#### jest

> Delightful JavaScript Testing. Works out of the box for any React project. Capture snapshots of React trees.

`jest` 包含了 `karma + mocha + chai + sinon` 的所有常用功能，零配置开箱即用.

*facebook家的测试框架，与react打配合会更加得心应手一些。*

#### mocha

> Simple, flexible, fun JavaScript test framework for Node.js & The Browser.

强大的测试框架，中文名叫抹茶，常见的`describe，beforeEach`就来自这里.

#### karma

> A simple tool that allows you to execute JavaScript code in multiple real browsers. Karma is not a testing framework, nor an assertion library. Karma just launches an HTTP server, and generates the test runner HTML file you probably already know from your favourite testing framework.

不是测试框架，也不是断言库，可以做到`抹平浏览器障碍`式的生成测试结果.

#### chai

> BDD / TDD assertion framework for node.js and the browser that can be paired with any testing framework.

BDD/TDD断言库，三种常用风格：

- assert
- expect
- should

##### BDD/TDD是什么？

[《 What’s the difference between Unit Testing, TDD and BDD? 》](https://codeutopia.net/blog/2015/03/01/unit-testing-tdd-and-bdd/)

#### sinon

> Standalone and test framework agnostic JavaScript test spies, stubs and mocks.

`js mock`测试框架，"everything is fake".

##### 为什么以spec.js命名？

有这样一个问题：[What does “spec” mean in Javascript Testing](https://stackoverflow.com/questions/40222751/what-does-spec-mean-in-javascript-testing)

- `spec` 是 `sepcification` 的缩写。

就测试而言，Specification指的是给定特性或者必须满足的应用的技术细节。最好的理解这个问题的方式是：`让某一部分代码成功通过必须满足的规范。`



### 三. Vue 项目的单元测试

**我总结的 Vue 中单元测试的步骤:**

1. 安装依赖库
2. 配置jest (package.json)
3. 配置Babel (.babelrc)
4. 项目根目录新建__test__文件夹, 添加测试文件 [组件名]`.spec.js`
5. 运行测试

### 四. 可测性报告

#### 概要:

`src/` 的一级文件夹中, 明确可测试的有:

| 序号 | 文件夹名称  |
| ---- | ----------- |
| 1    | components/ |
| 2    | models/     |
| 3    | router/     |
| 4    | utils/      |

`src/` 的一级文件夹中, 部分可测试的有:

| 序号 | 文件夹名称 | 备注                                     |
| ---- | ---------- | ---------------------------------------- |
| 1    | pages      | Course  相关业务逻辑变化大, 等稳定后测试 |
| 2    | store      | 主要逻辑集中在 mutations/ 中             |

`src/` 的一级文件夹中, 无需测试的有:

| 序号 | 文件夹名称 | 备注                              |
| ---- | ---------- | --------------------------------- |
| 1    | assets/    | 里面存放的都是 icon、背景图等文件 |
| 2    | boot/      | `I18n` 配置相关代码               |
| 3    | layouts/   | 布局相关代码                      |
| 4    | locales/   | `I18n` 内容文件                   |
| 5    | styles/    | 样式文件                          |

#### 总览:

```cmd
aichef-web:
│
├─assets 				× 里面存放的都是 icon、背景图等文件
│
├─boot 					× I18n 配置相关代码
│
├─components 		√
│
├─layouts 			× 布局相关代码
│
├─locales 			× I18n 内容文件
│
├─models 				√
│
├─pages
│   ├─common 		√
│   │
│   ├─project0 	√
│   │
│   └─project1 	× Course 相关业务逻辑变化大, 等稳定后测试
│
├─router 				√
│
├─store 				? 主要逻辑集中在 mutations/ 中
│ actions.js
│ getters.js
│ index.js
│ mutations.js 	√
│ state.js
│
├─styles 				× 样式文件
│
└─utils 				√
```



 