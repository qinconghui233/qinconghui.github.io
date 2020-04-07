---
layout: post
title: 'JavaScript相关问题'
subtitle: '面试准备'
date: 2020-04-07
categories: 技术
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-theme-h2o-postcover.jpg'
tags: 网络 前端开发 
---

## javascript数据基本类型有哪些？
null、boolean、string、undefined、number、symbol(ES6新加)

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

## 跨域问题如何解决

同源策略：浏览器安全策略，同协议、ip、端口的脚本才会执行。只要协议、域名、端口有任何一个不同，都被当作是不同的域。

js跨域是指通过js在不同的域之间进行数据传输或通信

    1. 通过jsonp跨域
    jsonp在页面上引入不同域上的js脚本文件实现请求不同域上的数据
    (1) 通过script标签引入一个js文件
    (2) js文件载入成功后会执行我们在url参数中指定的函数，并且会把我们需要的json数据作为参数传入
    注：需要服务器端的页面进行相应的配合
    2. 通过修改document.domain来跨子域
    3. 使用window.name来进行跨域
    window对象有个name属性，该属性有个特征：即在一个窗口(window)的生命周期内,窗口载入的所有的页面都是共享一个window.name的，每个页面对window.name都有读写的权限，window.name是持久存在一个窗口载入过的所有页面中的，并不会因新页面的载入而进行重置。

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
function say(arg1,arg2){
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
