---
title: NodeJS核心模块笔记(七)常用工具 util
date: 2016-05-01 21:48:30
categories: 笔记
tags: [JS,NodeJS]
keywords: NodeJs,JavaScript
description: Node.js常用工具之util
---
常用工具 util
-------------
util 是一个 Node.js 核心模块，提供常用函数的集合，用于弥补核心 JavaScript 的功能过于精简的不足。
<!--more-->

>util.inherits

util.inherits(constructor, superConstructor)是一个实现对象间原型继承的函数。 JavaScript 的面向对象特性是基于原型的，与常见的基于类的不同。 JavaScript 没有提供对象继承的语言级别特性，而是通过原型复制来实现的，
首先介绍一下util.inherits的用法
``` js
var util = require('util');
function Base(){
    this.name = 'base';
    this.base = 1989;
    this.sayHello = function(){
        console.log('Hello' + this.name);
    };
};
Base.prototype.showName = function(){
    console.log(this.name);
};
function Sub(){
    this.name = 'sub';
};

util.inherits(Sub,Base);

var objBase = new Base();
objBase.showName();
objBase.sayHello();
console.log(objBase);

var objSub = new Sub();
objSub.showName();
// objSub.sayHello();
console.log(objSub);
```

下面分析一下代码：

>首先定义了一个基础对象Base和一个继承自Base的Sub对象，Base有三个在构造函数内定义的属性和一个原型中定义的函数，通过util.inherits实现继承。

运行结果如下：
``` js
base
Hello base
{
    name:'base',
    base:1989,
    sayHello:[Function]
}
sub
{
    name:'sub'
}
```

>注意：Sub仅仅继承了Base在原型中定义的函数，而构造函数内部创造的base属性和sayHello函数都没有被Sub继承。同时，在原型中定义的属性不会被console.log作为对象的属性输出。这就是为为什么在代码中注释掉objSub.sayHello()这行的原因，不然会报错。

util.inspect
------------
util.inspect(object,[showHidden],[depth],[colors])是一个将任意对象转换为字符串的方法，通常用于调试和错误输出。
对它的的参数下面简要列出一下：
+ 接受一个参数object，即要转换的对象。
+ showHidden 是一个可选参数，如果值为 true，将会输出更多隐藏信息。
+ depth 表示最大递归的层数
    * 如果对象很复杂，你可以指定层数以控制输出信息的多少。
    * 如果不指定depth，默认会递归2层，
    * 指定为 null 表示将不限递归层数完整遍历对象。
+ 如果color 值为 true，输出格式将会以 ANSI 颜色编码，通常用于在终端显示更漂亮的效果。

>特别要指出的是， util.inspect 并不会简单地直接把对象转换为字符串，即使该对象定义了 toString 方法也不会调用。

看下面这个例子：
``` js
var util = require('util');
function Person(){
    this.name = 'xiege';
    this.toString = function(){
        return this.name
    };
};
var obj = new Person();
console.log(util.inspect(obj));
console.log(util.inspect(obj,true));
```

运行结果如下：
``` js
{
    name:'xiege',
    toString:[Function]
}
{
    toString:{
        [Function][prototype]:{[constructor]:[Circular]},
        [caller]:null,
        [length]:0,
        [name]:'',
        [arguments]:null
        }
    },
    name:'xiege'
}
```

总结：
-----
除了以上说到的几个函数外，util还提供了util.isArray()、util.isRegExp()、util.isDate()、util.isError()四个类型测试工具，以及util.format()、util.debug()等工具。