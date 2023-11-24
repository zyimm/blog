---
title: 这是一段来源于 `carbon.now.sh` 的代码
title: Git 免密登录
date: 2022-09-30
tags: 
  - javascript
---

carbon.now.sh 可以让你的文本代码转换成图片，并可以随时分享出去！

```js
const pluckDeep = key => obj => key.split('.').reduce((accum, key) => accum[key], obj)

const compose = (...fns) => res => fns.reduce((accum, next) => next(accum), res)

const unfold = (f, seed) => {
  const go = (f, seed, acc) => {
    const res = f(seed)
    return res ? go(f, res[1], acc.concat([res[0]])) : acc
  }

```
