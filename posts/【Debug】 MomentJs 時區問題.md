---
title: 【Debug】 Moment.js 時區問題
description: Moment.js 時區問題
date: 2022-10-21
tags:
    - Debug
    - MomentJs
layout: layouts/post.njk
image: /img/posts/黑人問號.png
---

### 前言
![MomentJs時區問題-1](/img/posts/MomentJs時區問題-1.png)
怪了，日期區間沒道理會有小數點阿...

### Moment.js
在前端開發時，常會遇到需要處理日期或是時間的問題，而 [MomentJs] 是與日期和時間有關的的解析、轉換、設置、格式化日期的 JavaScript 函式庫，
透過 [MomentJs] 就可以簡化日期或是時間的處理。

### 用 DevTools 模擬不同國家時區
1. DevTools(F12) 中右上角的三個小點點 > More tools > Sensors  
![DevTools-1](/img/posts/DevTools-1.png)

2. Sensors 頁籤就可以看到 Location 有一個下拉可以選擇
![DevTools-2](/img/posts/DevTools-2.png)

### Debug
![MomentJs時區問題-2](/img/posts/MomentJs時區問題-2.png)
![2022夏令時間](/img/posts/2022夏令時間.png)

一查才發現，原來 [MomentJs] 在解析日期時，會使用到時區，
然後又因為日期區間剛好跨到日光節約時間與正常時間，所以在做計算時，才會有小數點產生。


[MomentJs]: https://momentjs.com/  "Moment.js"