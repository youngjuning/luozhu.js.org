---
title: '「已解决」TypeError: moment().tz is not a function'
date: 2023-03-21 12:41:19
description: 本文解决了迁移项目时下面的代码编译报错 TypeError moment().tz is not a function
categories:
  - [issues, moment]
  - [前端, JavaScript]
tags:
  - moment
  - moment-timezone
  - TypeError
  - moment().tz
---

## 问题

迁移项目时下面的代码编译报错 `TypeError: moment().tz is not a function`：


```js
let myDate = moment().tz(undefined, vm.timeZone).format('YYYY-MM-DD');
```

## 解决办法 1

使用 `moment-timezone` 代替：

```js
import moment from 'moment-timezone';
```

## 解决办法 2

```js
import moment from 'moment';
import 'moment-timezone';
```
