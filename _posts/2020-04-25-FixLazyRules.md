---
layout: post
title: "解决 Quasar 重置校验后 lazy-rules 一起被重置的问题"
subtitle: ""
author: "Ivens"
header-style: text
tags:
  - Vue.js
---

最近在项目中遇到了一个问题:

- 事先绑定了 `lazy-rules` 属性的输入框在重置校验后, 再次触发 `lazy-rules`, 需要输入两次才会触发校验.

此前我的做法是给组件的 `lazy-rules` 属性绑定了一个变量, 通过控制变量来阻止 `lazy-rules` 再次触发. 这种做法一是增加了变量徒增消耗, 第二不利于代码的可读性.

首先, `Quasar` 给我们的 `q-input` 组件提供了一个属性: `lazy-rules`

![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200425125915.png)

添加这个属性可以做到, 不会让你刚一进入页面时, 就立刻触发校验, 搞得一个表单到处都是错误提示.

我一开始的思路是先去 `Quasar` 官网找 `resetValidation()` 相关的文档, 希望能有哪个选项不需要连同 `lazy-rules` 一起重置, 可惜并没有发现.

所以我只好寄希望于源码, 打开 webstorm, 无法直接在 `<q-input>` 中直接 `ctrl + 左键`进入方法定义, 所以我全局搜索 Qinput 类:

进入 `node_modules/quasar/dist/types/index.d.ts` 文件, 搜索 `rule`, 可以看到:

```typescript
export interface QInput extends Vue {
  ...
  
  /**
   \* Array of Functions/Strings; If String, then it must be a name of one of the embedded validation rules
   */
  rules? : any[]

  /**
   \* Check validation status against the 'rules' only after field loses focus for first time
   */
  lazyRules? : boolean
  
  ...
  
   /**
   * Reset validation status
   */
  resetValidation (): void
  /**
   * Trigger a validation
   * @param value Optional value to validate against
   * @returns True/false if no async rules, otherwise a Promise with the outcome (true -> validation was a success, false -> invalid models detected)
   */
  validate (value? : any): boolean | Promise<boolean>
}
```

这是 `QInput` 的接口定义, 我又找到了他实现接口的地方: `node_modules/quasar/src/mixins/validate.js`

![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/f5c0b9b6fbfbc3a6ed59c7da760b617.png)

文件中设置了侦听 `q-input` 值, 如果 `lazyRules` 值为 `true` && `isDirty` 的值为 `false` 时,  会直接弹出不会进行验证.

![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/07b8fd0a271b66059456e969719f879.png)

而每次重置校验的时候, 又都把 `isDirty` 赋了个 `false`, 所以就导致下次 `q-input` 发生改变时, 中途就弹出了, 执行不到校验的一步.

那么, 问题的解决方法找到了:

- 我们在每次 `resetValidation()` 后, 设置 `isDirty` 的值为 `true` 即可.



之前的代码:

```js
firstInput () {
  this.$refs.secondPwd.resetValidation()
  if (this.newPwd.first === this.newPwd.second) {
    this.lazyFlag1 = false  // FIXME 
    this.$refs.firstPwd.validate()
  } else {
    this.$refs.firstPwd.validate()
  }
}
```

修改后的代码: 

```js
/**
 * 在 新密码栏 输入会触发当前函数
 */
firstInput () {
  this.$refs.secondPwd.resetValidation()
  this.$refs.secondPwd.isDirty = true
  this.$refs.firstPwd.validate()
}
```





