---
layout: post
title: '前端学习——JavaScript相关问题'
subtitle: '面试准备'
date: 2020-04-07
categories: 技术
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-theme-h2o-postcover.jpg'
tags: 网络 前端开发 
---

## javascript数据基本类型有哪些？
null、boolean、string、undefined、number、symbol(ES6新加)

### JavaScript3种对象类型
Object、Date、Array

### Number()转换为数字，String()转化为字符串，Boolean()转化为布尔值

## const var let

const定义的变量不可以修稿，而且必须初始化

var定义的变量可以修改，如果不初始化会输出undefined，不会报错

let是快级作用域

## JS执行三部曲：语法分析 预编译 解释执行

函数声明整体提升指的是当你使用了函数声明，那么函数声明默认会提升至script顶部

imply global 暗示全局变量：即任何变量未经声明就赋值，那么这个变量就为全局对象所有。全局变量指的是window

### 预编译
1.创建AO对象 Activation Object（执行期上下文）
2.找形参和变量声明，将变量和形参名作为AO对象的属性名，值为undefined
3.将实参形参相统一
4.在函数体里找到函数声明，把函数声明当做值赋予函数体

function fn(){}这种形式才叫做函数声明，而var fn = function(){}j叫做函数表达式
## 箭头函数与function函数的区别
1.function函数与箭头函数的定义写法不同

    function fn(a,b){
    return a + b;
    }

    var foo = (a,b)=>{
    return a + b;
    };

2.this的指向不同

使用function定义的函数，this的指向随着调用环境的变化而变化的，而箭头函数中的this指向是固定不变的，一直指向的是定义函数的环境。

3.变量提升

由于js的内存机制，function的级别最高，而用箭头函数定义函数的时候，需要var(let const定义的时候更不必说)关键词，而var所定义的变量不能得到变量提升，故箭头函数一定要定义于调用之前！

## new一个对象过程发生了什么

1.创建一个新对象，如：var person = {};

2.新对象的_proto_属性指向构造函数的原型对象。

3.将构造函数的作用域赋值给新对象。（也所以this对象指向新对象）

4.执行构造函数内部的代码，将属性添加给person中的this对象。

5.返回新对象person。

## 函数传参数是按值还是引用?数据类型或者对象类型都一样吗？

是按值传递的！！！数据类型或者对象类型都是。

## 闭包是什么

闭包就是能够读取其他函数内部变量的函数。

闭包的用途：可以读取函数内部的变量，并且让这些变量的值始终保持在内存中。



### babel可以实现将ES6编译为ES5代码

### 用JavaScript判断一个变量是否为整数

实现思路：先判断该变量是否为Number类型，以此来缩小范围，再判断该变量除以1后是否与原值全等，若全等则返回true，若不全等则返回false

## 进程和线程是什么

线程是最小的执行单元，进程由至少一个线程组成。

如何调度进程和线程，完全由操作系统决定，程序自己不能决定什么时候执行，执行多长时间。

进程指计算机中已运行的程序，线程指错做系统能够进行运算的最小单位

## 死锁是什么

当两个以上的运算单元，双方都在等待对方停止运行，以获取系统资源，但是没有一方提前退出时，就成为死锁

## this
### this的指向是什么？
定义：this的指向是包含它的函数作为方法被调用时所属的对象。

### 如何修改this的指向

(1)使用局部变量来代替this指针
(2)使用call或apply方法
call 普通传参
    <pre><code class="language-javascript">function say(arg1,arg2){
        console.log(this.name,arg1,arg2);
    };
    var obj = {
        name : 'tom',
        say : function(){
        console.log(this.name);
         }
    }
    say.call(obj,'one','two');//tom one two
    apply 以数组的形式传参
    function say(arg1,arg2){
    console.log(this.name,arg1,arg2);
    };
    var obj = {
    name : 'tom',
    say : function(){
     console.log(this.name);
    }
    }
    say.apply(obj,['one','two']);//tom one two
    </code></pre>

## promise

Promise最大的好处是在异步执行的流程中，把执行代码和处理结果的代码清晰地分离了

有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，Promise对象提供统一的接口，使得控制异步操作更加容易。

Promise有三种状态：pending(进行中)、resolved(成功)、rejected(失败)

Promise对象的缺点：

1、无法取消Promise，一旦新建它就会立即执行，无法中途取消。

2、如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。

3、当处于Pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

4、Promise 真正执行回调的时候，定义 Promise 那部分实际上已经走完了，所以 Promise 的报错堆栈上下文不太友好。



<pre><code class="language-javascript">const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});</code></pre>

### async promise generator

async/await是基于promise实现的，他不能用于普通的回调函数

async/await使得异步代码看起来像同步代码

async/await与Promise一样，是非阻塞的。

Generator 是ES6引入的新语法，Generator是一个可以暂停和继续执行的函数。

简单的用法，可以当做一个Iterator来用，进行一些遍历操作。复杂一些的用法，他可以在内部保存一些状态，成为一个状态机。

Generator 基本语法包含两部分：函数名前要加一个星号；函数内部用 yield 关键字返回值。

yield表达式本身没有返回值，或者说总是返回undefined。

next方法可以带一个参数，该参数就会被当作上一个yield表达式的返回值。


<pre><code class="language-javascript">function * foo(x) {

    var y = 2 * (yield (x + 1));

    var z = yield (y / 3);

    return (x + y + z);

}

var b = foo(5); 

b.next() // { value:6, done:false }

b.next(12) // { value:8, done:false } 

b.next(13) // { value:42, done:true }</code></pre>

## 如何阻止冒泡
w3c的方法是e.stopPropagation()，

IE则是使用e.cancelBubble = true；

### 什么是冒泡事件？
如在一个按钮是绑定一个”click”事件，那么”click”事件会依次在它的父级元素中被触发。

stopPropagation就是阻止目标元素的事件冒泡到父级元素。

兼容写法：
<pre><code class="language-javascript">window.event? window.event.cancelBubble = true : e.stopPropagation();
</code></pre>

<!-- <pre><code class="language-javascript"></code></pre> -->

## 如何改变默认行为
preventDefault它是事件对象(Event)的一个方法，作用是取消一个目标元素的默认行为。

元素必须有默认行为才能被取消，如果元素本身就没有默认行为，调用当然就无效了。

## 链表反转
1.就地反转发
2.头节点插入法

## 会改变作用域链的三种方法
1.eval() 函数
2.with()
3.try-catch


[Array.prototype.flat()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)

### css选择器及权重
1、ID　　#id

2、class　　.class

3、标签　　p

4、通用　　*

5、属性　　[type="text"]

6、伪类　　：hover

7、伪元素　　::first-line

8、子选择器、相邻选择器

1. 第一等：代表内联样式，如: style=””，权值为1000。

2. 第二等：代表ID选择器，如：#content，权值为0100。

3. 第三等：代表类，伪类和属性选择器，如.content，权值为0010。

4. 第四等：代表类型选择器和伪元素选择器，如div p，权值为0001。

5. 通配符、子选择器、相邻选择器等的。如*、>、+,权值为0000。

6. 继承的样式没有权值。

[http缓存相关](https://www.jianshu.com/p/227cee9c8d15)

## 递归
递归：直接或间接调用自身算法的过程

满足使用递归的条件：

子问题为同类事物，且更简单
必须有个出口

优点：

代码简洁
符合思维习惯，容易理解
缺点：

效率较低
递归层次太深，耗内存且容易栈溢出一定要使用的话，最好使用缓存避免相同的计算，限制递归调用的次数

## 优化

### function tick() 一直调用可以刷新内部内容


### setInterval 定时运行

### 封装

### setState可以合并成一个调用来提高性能

不需要保存状态定义function 需要保存state状态定义成class类
props外部流入  state引用内部


const count = 1
APP
    ---> <Clock couter={count}>
            +--->props.count
            +--->state{
                couner:props.count
                second:15
            }
            ----->render()
                    +---<Second second={second}/>
                            +--->props.second
                            +--->state{
                                second:props.second
                            }
                            +--->render()

## JavaScript 常见的设计模式
1.工厂模式
简单的工厂模式可以理解为解决多个相似的问题；
2.单例模式
只能被实例化(构造函数给实例添加属性与方法)一次
3.沙箱模式
将一些函数放到自执行函数里面，但要用闭包暴露接口，用变量接收暴露的接口，再调用里面的值，否则无法使用里面的值
<pre><code class="language-javascript">
let sanboxMode=(function(){
  function sayName();
  function sayAge();
  return{
    sayName:sayName,
    sayAge:sayAge
  }
})()
</code></pre>
4.发布者订阅模式
<pre><code class="language-javascript">
//发布者与订阅模式
    var shoeObj = {}; // 定义发布者
    shoeObj.list = []; // 缓存列表 存放订阅者回调函数

    // 增加订阅者
    shoeObj.listen = function(fn) {
        shoeObj.list.push(fn); // 订阅消息添加到缓存列表
    }

    // 发布消息
    shoeObj.trigger = function() {
            for (var i = 0, fn; fn = this.list[i++];) {
                fn.apply(this, arguments);//第一个参数只是改变fn的this,
            }
        }
     // 小红订阅如下消息
    shoeObj.listen(function(color, size) {
        console.log("颜色是：" + color);
        console.log("尺码是：" + size);
    });

    // 小花订阅如下消息
    shoeObj.listen(function(color, size) {
        console.log("再次打印颜色是：" + color);
        console.log("再次打印尺码是：" + size);
    });
    shoeObj.trigger("红色", 40);
    shoeObj.trigger("黑色", 42);  
</code></pre>


## JavaScript 的原型和原型链
[内容地址](https://www.jianshu.com/p/be7c95714586)

### 构造函数的继承
第一步是在子类的构造函数中，调用父类的构造函数。
<pre><code class="language-javascript">
    function Sub(value) {
        Super.call(this);
        this.prop = value;
    }
</code></pre>
上面代码中，Sub是子类的构造函数，this是子类的实例。在实例上调用父类的构造函数Super，就会让子类实例具有父类实例的属性。

第二步，是让子类的原型指向父类的原型，这样子类就可以继承父类原型。

    Sub.prototype = Object.create(Super.prototype);
    Sub.prototype.constructor = Sub;
    Sub.prototype.method = '...';

上面代码中，Sub.prototype是子类的原型，要将它赋值为Object.create(Super.prototype)，而不是直接等于Super.prototype。否则后面两行对Sub.prototype的操作，会连父类的原型Super.prototype一起修改掉。

另外一种写法是Sub.prototype等于一个父类实例。

    Sub.prototype = new Super();

举例来说，下面是一个Shape构造函数。
<pre><code class="language-javascript">
    function Shape() {
        this.x = 0;
        this.y = 0;
    }

    Shape.prototype.move = function (x, y) {
        this.x += x;
        this.y += y;
        console.info('Shape moved.');
    };
</code></pre>
我们需要让Rectangle构造函数继承Shape。
<pre><code class="language-javascript">
    // 第一步，子类继承父类的实例
    function Rectangle() {
        Shape.call(this); // 调用父类构造函数
    }
    // 另一种写法
    function Rectangle() {
        this.base = Shape;
        this.base();
    }

    // 第二步，子类继承父类的原型
    Rectangle.prototype = Object.create(Shape.prototype);
    Rectangle.prototype.constructor = Rectangle;
</code></pre>

采用这样的写法以后，instanceof运算符会对子类和父类的构造函数，都返回true。
<pre><code class="language-javascript">
    var rect = new Rectangle();
    rect instanceof Rectangle  // true
    rect instanceof Shape  // true
</code></pre>
上面代码中，子类是整体继承父类。有时只需要单个方法的继承，这时可以采用下面的写法。
<pre><code class="language-javascript">
    ClassB.prototype.print = function() {
        ClassA.prototype.print.call(this);
    // some code
    }
</code></pre>

### 多重继承
<pre><code class="language-javascript">
    function M1() {
        this.hello = 'hello';
    }

    function M2() {
        this.world = 'world';
    }

    function S() {
        M1.call(this);
        M2.call(this);
    }

    // 继承 M1
    S.prototype = Object.create(M1.prototype);
    // 继承链上加入 M2
    Object.assign(S.prototype, M2.prototype);

    // 指定构造函数
    S.prototype.constructor = S;

    var s = new S();
    s.hello // 'hello'
    s.world // 'world'
</code></pre>





### [JavaScript执行上下文栈/作用域链]()

### Object.prototype.toString   typeof  Array.isArray()