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
</code>
</pre>

## 事件循环
js通过事件循环来实现异步

1.归类
遇到同步认识直接执行，遇到异步任务分类为宏任务(macro-task)和微任务(micro-task)。

宏任务:整体的script setTimeout setInterval
微任务：promise proceed nextTick MutationObserver(html5新特性)

![](screenshot/1.webp)

注意进入到任务队列的是具体的执行任务的函数。
比如上述例子setTimeout()中的timer函数。另外不同类型的任务会分别进入到他们所属类型的任务队列，比如所有setTimeout()的回调都会进入setTimeout任务队列，所有then()回调都会进入到thin队列。当前的整体代码我们可以认为是宏任务。事件循环从当前整体代码开始第一次事件循环，然后再执行队列中的所有微任务，当微任务执行完毕之后，事件循环再找到其中一个宏任务队列并执行其中的所有任务，然后再找到一个微任务队列并执行里面的所有任务，就这样一直循环下去。

<pre><code class="language-javascript">
    console.log('global')

    setTimeout(function () {
    console.log('timeout1')
    new Promise(function (resolve) {
        console.log('timeout1_promise')
        resolve()
    }).then(function () {
        console.log('timeout1_then')
    })
    },2000)

    for (var i = 1;i <= 5;i ++) {
    setTimeout(function() {
        console.log(i)
    },i*1000)
    console.log(i)
    }

    new Promise(function (resolve) {
    console.log('promise1')
    resolve()
    }).then(function () {
    console.log('then1')
    })

    setTimeout(function () {
    console.log('timeout2')
    new Promise(function (resolve) {
        console.log('timeout2_promise')
        resolve()
    }).then(function () {
        console.log('timeout2_then')
    })
    }, 1000)

    new Promise(function (resolve) {
    console.log('promise2')
    resolve()
    }).then(function () {
    console.log('then2')
    })
    </code>
</pre>

1.首先执行整体代码，“global”会被第一个打印出来。这是第一个输出。

2.执行到第一个setTimeout时，发现它是宏任务，此时会新建一个setTimeout类型的宏任务队列并派发当前这个setTimeout的回调函数到刚建好的这个宏任务队列中去，并且轮到它执行时要延迟2秒后再执行。

3.代码继续执行走到for循环，发现是循环5次setTimeout()，那就把这5个setTimeout中的回调函数依次派发到上面新建的setTimeout类型的宏任务队列中去，注意，这5个setTimeout的延迟分别是1到5秒。此时这个setTimeout类型的宏任务队列中应该有6个任务了。再执行for循环里的console.log(i)，，直接输出1,2,3,4,5，这是第二个输出。

4.再执行到new Promise，Promise构造函数中的第一个参数在new的时候会直接执行，因此不会进入任何队列，所以第三个输出是"promise1"，上面有说到Promise.then是微任务，那么这里会生成一个Promise.then类型的微任务队列，这里的then回调会被push进这个队列中。

5.再继续走，执行到第二个setTimeout，发现是宏任务，派发它的回调到上面setTimeout类型的宏任务队列中去。

6.再走到最后一个new Promise，很明显，这里会有第四个输出："promise2"，然后它的then中的回调也会被派发到上面的Promise.then类型的微任务队列中去。

7.第一轮事件循环的宏任务执行完成（整体代码可以看做宏任务）。此时微任务队列中只有一个Promise.then类型微任务队列，它里面有两个任务。宏任务队列中也只有一个setTimeout类型的宏任务队列。

8.下面执行第一轮事件循环的微任务，很明显，会分别打印出"then1"，和"then2"。分别是第五和第六个输出。此时第一轮事件循环完成。

9.开始第二轮事件循环：执行setTimeout类型队列（宏任务队列）中的所有任务。发现都有延时，但延时最短的是for循环中第一次循环push进来的那个setTimeout和上面第5个步骤中的第二个setTimeout，它们都只延时1s。它们会被同时执行，但前者先被push进来，所以先执行它！它的作用就是打印变量i，在当前作用域找变量i，木有！去它上层作用域（这里是全局作用域）找，找到了，但此时的i早已是6了。所以这里第七个输出是延时1s后打印出6。

10.紧接着执行第二个setTimeout，它会先后打印出"timeout2"和"timeout2_promise"，这分别是第八和第九个输出。但这里发现了then，又把它push到上面已经被执行完的then队列中去。

11.这里要注意，因为出现了微任务then队列，所以这里会执行该队列中的所有任务（此时只有一个任务），即打印出"timeout2_then"。这是第十个输出。

12.继续回过头来执行宏任务队列，此时是执行延时为2s的第一个setTimeout和for循环中第二次循环的那个setTimeout，跟上面一样，前者是第一个被push进来的，所以它先执行。这里会延时1秒（原因下面会解释）分别输出“timeout1”和“timeout1_promise”，但发现了里面也有一个then，于是push到then微任务队列并立即执行，输出了"timeout1_then"。紧接着执行for中第二次循环的setTimeout，输出6。注意这三个几乎是同时被打印出来的。他们分别是第十一到十三个输出。

13.把省下的for循环中后面三次循环被push进来的setTimeout依次执行，于是每隔1s输出一个6，连续输出3次。

所以上代码的执行结果为：
<pre><code class="language-javascript">
    global
    1
    2
    3
    4
    5
    promise1
    promise2
    then1
    then2
    //延迟1s
    6
    timeout2
    timeout2_promise
    timeout2_then
    //延迟1s
    timeout1
    17 timeout1_promise
    20 timeout1_then
    6
    //每隔1s输出3个6
    </code>
</pre>

这里解释下为什么上面第12步不是延迟2秒再输出“timeout1”和“timeout1_promise”，这时需要理解setTimeout()延时参数的意思，这个延迟时间始终是相对主程序执行完毕的那个时间算的 ,并且多个setTimeout执行的先后顺序也是由这个延迟时间决定的。

再回过头来看上面那个问题，理解了事件循环的机制，问题就很简单了。for循环时setTimeout()不是立即执行的，它们的回调被push到了宏任务队列当中，而在执行任务队列里的回调函数时，变量i早已变成了6。那如何得到想要的结果呢？很简单，原理就是需要给循环中的setTimeout()创建一个闭包作用域，让它执行的时候找到的变量i是正确的。

### 解决方案
1.引入IIFE
<pre><code class="language-javascript">
    for(var i = 0;i < 5 ;i ++) {
    (function(i){
        setTimeout(function timer() {
        console.log(i)
        }, i * 1000);
    })(i);
    }
    </code>
</pre>