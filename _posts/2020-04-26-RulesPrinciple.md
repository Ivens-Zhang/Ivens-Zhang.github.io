---
layout: post
title: "Quasar 中 validate 校验原理"
subtitle: ''
author: "Ivens"
header-style: text
tags:
  - Vue.js
---

## 前言

最近在项目中一直在与校验作斗争, 今天中午午饭之前, 把 `Quasar` 中 `validate.js` 的源码拿出来看了下, 分析一下内部是如何实现的.

代码如下:


```js
/*
 * Return value
 *   - true (validation succeeded)
 *   - false (validation failed)
 *   - Promise (pending async validation)
 */
validate (val = this.value) {
  //  如果没有设置 rules 属性, 直接返回 true
  if (!this.rules || this.rules.length === 0) {
    return true
  }

  this.validateIndex++

  
  if (this.innerLoading !== true && this.lazyRules !== true) {
    this.isDirty = true
  }

  // 更新错误信息
  const update = (err, msg) => {
    if (this.innerError !== err) {
      this.innerError = err
    }

    const m = msg || void 0
    if (this.innerErrorMessage !== m) {
      this.innerErrorMessage = m
    }

    if (this.innerLoading !== false) {
      this.innerLoading = false
    }
  }

  const promises = []

  // 遍历 rules 数组
  for (let i = 0; i < this.rules.length; i++) {
    const rule = this.rules[i]
    let res
		
    // 如果 rule 是一个 function, 把 watch 中侦听的值作为参数传入, 进行校验
    if (typeof rule === 'function') {
      res = rule(val)
    }
    // 如果 rule 为一个字符串, 并且是在 testPattern 中已经存在的, 则调用 testPattern 中内置的方法进行校验
    else if (typeof rule === 'string' && testPattern[rule] !== void 0) {
      res = testPattern[rule](val)
    }

    // 在 Quasar 中规定, rules 内部校验的格式为: "value => condition || errorMessage"
    // 所以一旦前面的 condition 返回的是 false, 当前的 res 则会被赋值为后面的 errorMessage
    // 而 testPattern 中内置的校验则只会返回 true 或 false
    if (res === false || typeof res === 'string') {
      update(true, res)
      return false
    }
    else if (res !== true && res !== void 0) {
      promises.push(res)
    }
  }

  if (promises.length === 0) {
    update(false)
    return true
  }

  if (this.innerLoading !== true) {
    this.innerLoading = true
  }

  const index = this.validateIndex

  return Promise.all(promises).then(
    res => {
      if (index !== this.validateIndex) {
        return true
      }

      if (res === void 0 || Array.isArray(res) === false || res.length === 0) {
        update(false)
        return true
      }

      const msg = res.find(r => r === false || typeof r === 'string')
      update(msg !== void 0, msg)
      return msg === void 0
    },
    e => {
      if (index === this.validateIndex) {
        console.error(e)
        update(true)
        return false
      }

      return true
    }
  )
}
```

## 正文

先看返回值, 不难看出我们使用的 `validate()` 函数, 返回值有 3 种:

- true
- false
- promise (等待异步验证 => 执行下一轮校验)

