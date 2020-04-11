---
layout: post
title: 'setTimeout与事件循环'
subtitle: ''
date: 2020-04-07
categories: 技术
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-theme-h2o-postcover.jpg'
tags: 网络 前端开发 
---

首先常见面试题

<pre><code class="language-javascript">
   for (var i = 1;i <= 5;i ++) {

  setTimeout(function timer() {

      console.log(i)

  },i * 1000)
console.log(i);
}

</pre>

## 事件循环
js通过事件循环来实现异步

1.归类
遇到同步认识直接执行，遇到异步任务分类为宏任务(macro-task)和微任务(micro-task)。

宏任务:整体的script setTimeout setInterval
微任务：promise proceed nextTick

![](screenshot/1.webp)