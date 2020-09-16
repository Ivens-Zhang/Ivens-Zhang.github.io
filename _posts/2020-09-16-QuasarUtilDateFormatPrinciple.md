---
layout: post
title: "Quasar中日期工具库中日期格式化处理的原理"
subtitle: ""
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-11-30/th.jpg"
tags:
  - Vue.js
---

之前在项目中使用 `moment.js` 的 `format` 来做日期格式化，效果是一样的，缺点是多引入了一个库徒增体积。昨天突然发现 `Quasar` 的日期工具类中可以实现相同的功能，所以做了一个替换，并且研究了一下它是如何实现这个功能的。附 `Quasar` 官方文档：[日期实用程序(Date Utils)](http://www.quasarchs.com/quasar-utils/date-utils#显示格式)

这里是我们项目中使用日期工具库的代码片段：

```js
import { date } from 'quasar'
const { formatDate } = date

/**
 * Reference：http://www.quasarchs.com/quasar-utils/date-utils#%E6%98%BE%E7%A4%BA%E6%A0%BC%E5%BC%8F
 * 传入时间戳，例如：1600054260793
 * 转换为：yyyy-MM-DD-HH:mm:ss 格式，如：2020-09-14 11:53:13
 */
export function transferTimestampToDate (timestamp) {
  let date = formatDate(timestamp, 'YYYY-MM-DD-HH:mm:ss')
  return date
}
```

这是 `node_modules/quasar/src/utils/date.js` 文件内复制处理日期格式化的代码：

```js
export function formatDate (val, mask, dateLocale, __forcedYear) {
  // 兼容性处理
  if (
    (val !== 0 && !val) ||
    val === Infinity ||
    val === -Infinity
  ) {
    return
  }

  // 1、init
  let date = new Date(val)

  if (isNaN(date)) {
    return
  }

  if (mask === void 0) {
    mask = defaultMask
  }

  // 负责第三个参数，主要作用于 i18n 处理
  const locale = dateLocale !== void 0
    ? dateLocale
    : lang.props.date

  // 2、对 mask 替换
  // 该方法核心思想是利用 String.Property.replace 中可以循环匹配替换的特性，将我们传入的 format 例如：YYYY-MM-DD-HH:mm:ss，逐个替换为具体数据。
  // 这里的 token = /\[((?:[^\]\\]|\\]|\\)*)\]|d{1,4}|M{1,4}|m{1,2}|w{1,2}|Qo|Do|D{1,4}|YY(?:YY)?|H{1,2}|h{1,2}|s{1,2}|S{1,3}|Z{1,2}|a{1,2}|[AQExX]/g
  return mask.replace(
    token,
    (match, text) => match in formatter		// 这里用的 in 的语法返回布尔类型，判断这个 value 或 index 是否存在于这个对象或数组中
      ? formatter[match](date, locale, __forcedYear)	// 下文附 formatter 对象
      : (text === void 0 ? match : text.split('\\]').join(']'))
  )
}
```

`formatter` 对象：

```js
const formatter = {
  // Year: 00, 01, ..., 99
  YY (date, _, forcedYear) {
    // workaround for < 1900 with new Date()
    const y = this.YYYY(date, _, forcedYear) % 100
    return y > 0
      ? pad(y)
      : '-' + pad(Math.abs(y))
  },

  // Year: 1900, 1901, ..., 2099
  YYYY (date, _, forcedYear) {
    // workaround for < 1900 with new Date()
    return forcedYear !== void 0 && forcedYear !== null
      ? forcedYear
      : date.getFullYear()
  },

  // Month: 1, 2, ..., 12
  M (date) {
    return date.getMonth() + 1
  },

  // Month: 01, 02, ..., 12
  MM (date) {
    return pad(date.getMonth() + 1)
  },

  // Month Short Name: Jan, Feb, ...
  MMM (date, dateLocale) {
    return dateLocale.monthsShort[date.getMonth()]
  },

  // Month Name: January, February, ...
  MMMM (date, dateLocale) {
    return dateLocale.months[date.getMonth()]
  },

  // Quarter: 1, 2, 3, 4
  Q (date) {
    return Math.ceil((date.getMonth() + 1) / 3)
  },

  // Quarter: 1st, 2nd, 3rd, 4th
  Qo (date) {
    return getOrdinal(this.Q(date))
  },

  // Day of month: 1, 2, ..., 31
  D (date) {
    return date.getDate()
  },

  // Day of month: 1st, 2nd, ..., 31st
  Do (date) {
    return getOrdinal(date.getDate())
  },

  // Day of month: 01, 02, ..., 31
  DD (date) {
    return pad(date.getDate())
  },

  // Day of year: 1, 2, ..., 366
  DDD (date) {
    return getDayOfYear(date)
  },

  // Day of year: 001, 002, ..., 366
  DDDD (date) {
    return pad(getDayOfYear(date), 3)
  },

  // Day of week: 0, 1, ..., 6
  d (date) {
    return date.getDay()
  },

  // Day of week: Su, Mo, ...
  dd (date, dateLocale) {
    return this.dddd(date, dateLocale).slice(0, 2)
  },

  // Day of week: Sun, Mon, ...
  ddd (date, dateLocale) {
    return dateLocale.daysShort[date.getDay()]
  },

  // Day of week: Sunday, Monday, ...
  dddd (date, dateLocale) {
    return dateLocale.days[date.getDay()]
  },

  // Day of ISO week: 1, 2, ..., 7
  E (date) {
    return date.getDay() || 7
  },

  // Week of Year: 1 2 ... 52 53
  w (date) {
    return getWeekOfYear(date)
  },

  // Week of Year: 01 02 ... 52 53
  ww (date) {
    return pad(getWeekOfYear(date))
  },

  // Hour: 0, 1, ... 23
  H (date) {
    return date.getHours()
  },

  // Hour: 00, 01, ..., 23
  HH (date) {
    return pad(date.getHours())
  },

  // Hour: 1, 2, ..., 12
  h (date) {
    const hours = date.getHours()
    if (hours === 0) {
      return 12
    }
    if (hours > 12) {
      return hours % 12
    }
    return hours
  },

  // Hour: 01, 02, ..., 12
  hh (date) {
    return pad(this.h(date))
  },

  // Minute: 0, 1, ..., 59
  m (date) {
    return date.getMinutes()
  },

  // Minute: 00, 01, ..., 59
  mm (date) {
    return pad(date.getMinutes())
  },

  // Second: 0, 1, ..., 59
  s (date) {
    return date.getSeconds()
  },

  // Second: 00, 01, ..., 59
  ss (date) {
    return pad(date.getSeconds())
  },

  // 1/10 of second: 0, 1, ..., 9
  S (date) {
    return Math.floor(date.getMilliseconds() / 100)
  },

  // 1/100 of second: 00, 01, ..., 99
  SS (date) {
    return pad(Math.floor(date.getMilliseconds() / 10))
  },

  // Millisecond: 000, 001, ..., 999
  SSS (date) {
    return pad(date.getMilliseconds(), 3)
  },

  // Meridiem: AM, PM
  A (date) {
    return this.H(date) < 12 ? 'AM' : 'PM'
  },

  // Meridiem: am, pm
  a (date) {
    return this.H(date) < 12 ? 'am' : 'pm'
  },

  // Meridiem: a.m., p.m.
  aa (date) {
    return this.H(date) < 12 ? 'a.m.' : 'p.m.'
  },

  // Timezone: -01:00, +00:00, ... +12:00
  Z (date) {
    return formatTimezone(date.getTimezoneOffset(), ':')
  },

  // Timezone: -0100, +0000, ... +1200
  ZZ (date) {
    return formatTimezone(date.getTimezoneOffset())
  },

  // Seconds timestamp: 512969520
  X (date) {
    return Math.floor(date.getTime() / 1000)
  },

  // Milliseconds timestamp: 512969520900
  x (date) {
    return date.getTime()
  }
}
```

看到这里原理你应该就懂了，就是如上文我说的，利用 `replace` 方法特性进行循环替换，最后返回一个用具体数据替代 `mask` 后的日期字符串。

