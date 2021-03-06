---
title: Javascript系统复习：函数的重要属性和arguments
date: 2016-08-25
tags: ["javascript"]
---
## 前言:
这篇博客概括起来比较简单粗暴，就是写函数的常用属性和arguments对象～～   
(写完之后的我再回过头来看也觉得它意外的短啊。。。😂)   

## 函数的属性
- foo.name 函数名
- foo.length 形参的个数
- arguments.length 实参的个数
🌰：    
   
```js
function foo(x,y,z) {
    arguments.length; //2
    arguments[0]; //1
    arguments[0] = 10;
    x; //10

    arguments[2] = 100; 
    z; //undefined
    arguments.callee === foo; true
}

foo(1,2);
foo.length;// 3
foo.name; //"foo"
```

<!-- more -->
## arguments
还是楼上那段代码，来聊聊arguments。   
arguments是一个***类数组***的对象。  
之所以说是类数组，因为它原型并不是Array.prototype。   
但是我们可以像访问数组一样访问它。   
我们可以看到，我们可以通过访问arguments来修改形参的值。   
但是楼上的代码也提示了一个坑:    
如果某个参数没有传进来，arguments和它就不具有绑定关系。因此当我们给arguments[2]赋值的时候，形参的值并没有改变。   
因此,我们把arguments理解为***和被传值的形参相绑定的类数组对象***
    
arguments.callee返回的是arguments对应的函数对象。   
在"use strict"模式下，arguments.callee是不能使用的，而且arguments和形参不再具有绑定关系，只是形参的一个副本～～   
   
## bind方法更进一步
bind方法上一篇博客说apply和call的时候提了一下，这一篇博客再深入介绍个别的用法：    
   
```js
function add(a,b,c) {
    return a + b + c;
} 
var func = add.bind(undefined, 100);
func(1,2); //103

var func2 = func.bind(undefined, 200);
func2(10); //310
```

它不仅可以用来修改this的指向，还可以把一个函数的参数分阶段传入～～   
只有当最后一个参数被传入的时候，函数才真正被调用了。   

## 构造函数和bind
先看一个🌰：   
   
```js
function foo() {
    this.b = 100;
    return this.a;
}

var func = foo.bind({a:1});

func(); //1
new func(); // {b:100}
```
当我们对一个构造函数用bind的时候，如果我们直接调用它，和普通函数的bind没差。    
如果用了new,return除非是对象，如果return不是对象，那么仍然让原有的this作为返回值。    
   

## 总结
啊，这次写的东西意想不到的短啊？？？？？   
嘿嘿，因为大多数基础的知识点都在上一篇覆盖过了，好像确实是这样😂
晚安！^_^


