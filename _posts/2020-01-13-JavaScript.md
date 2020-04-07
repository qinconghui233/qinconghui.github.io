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

