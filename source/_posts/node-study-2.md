---
title: NodeJS学习笔记(三)模块
date: 2016-04-27 23:08:03
categories: 笔记
tags: [JS,NodeJS]
keywords: NodeJs,JavaScript
description: Node.js模块和包
---
模块（Module）和包（Package）是 Node.js最重要的支柱。开发一个具有一定规模的程序不可能只用一个文件，通常需要把各个功能拆分、封装，然后组合起来，模块正是为了实现这种方式而诞生的。在浏览器 JavaScript 中，脚本模块的拆分和组合通常使用 HTML 的script 标签来实现。 Node.js 提供了 require 函数来调用其他模块，而且模块都是基于文件的，机制十分简单。
<!--more-->
什么是模块？
-----------
模块是 Node.js 应用程序的基本组成部分，文件和模块是一一对应的。换言之，一个Node.js 文件就是一个模块，这个文件可能是 JavaScript 代码、 JSON 或者编译过的 C/C++ 扩展。下面就是我们调用一个模块的例子：
``` js
var http = require('http');
```
其中 http是 Node.js的一个核心模块，其内部是用C++实现的，外部用JavaScript封装。只是需要通过require函数获取这个模块，然后才有使用其中的对象。

创建模块
--------
在 Node.js 中，创建一个模块非常简单，因为一个文件就是一个模块，我们要关注的问题仅仅在于如何在其他文件中获取这个模块。 Node.js 提供了 exports 和 require 两个对象，其中 exports 是模块公开的接口， require 用于从外部获取一个模块的接口，即所获取模块的 exports 对象。
首先，创建一个module.js文件，内容如下：
``` js
//module.js
var name;
exports.setName = function(thyName){
    name = thyName;
};
exports.sayHello = function(){
    console.log('hello' + name);
};
```
然后在同一目录下创建一个getmodule.js文件，内容如下：
``` js
// getmodule.js
var myModules = require('./module');
myModules.setName('心情');
myModules.sayHello();
```
最后运行的结果是
```
hello心情
```
在以上示例中， module.js 通过 exports 对象把 setName 和 sayHello 作为模块的访问接口，在 getmodule.js 中通过 require('./module') 加载这个模块，然后就可以直接访问 module.js 中 exports 对象的成员函数了。
现在，我将上面的module.js文件内容修改一下，内容如下：
``` js
// module.js
function hello(){
    var name;
    this.setName = function(thyName){
        name = thyName;
    };
    this.sayHello = function(){
        console.log('hello' + name);
    };
};
module.exports = hello;

// getmodule.js
var hello = require('./module.js');
_hello = new hello();
_hello.setName('心情');
_hello.sayHello();
```
最后输出的结果是一样的。事实上， exports 本身仅仅是一个普通的空对象，即 {}，它专门用来声明接口，本质上是通过它为模块闭包①的内部建立了一个有限的访问接口。因为它没有任何特殊的地方，所以可以用其他东西来代替，譬如我们上面例子中的 Hello 对象。

警告：
-----
不可以通过对 exports 直接赋值代替对 module.exports 赋值。exports 实际上只是一个和 module.exports 指向同一个对象的变量，它本身会在模块执行结束后释放，但 module 不会，因此只能通过指定module.exports 来改变访问接口。