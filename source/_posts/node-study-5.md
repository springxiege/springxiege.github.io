---
title: NodeJS核心模块笔记(六)全局对象
date: 2016-04-30 23:03:30
categories: 笔记
tags: [JS,NodeJS]
keywords: NodeJs,JavaScript
description: Node.js调试
---
今天开始，才是真正的接触到了node.js的核心部分，那就是node.js的核心模块。核心模块是 Node.js 的心脏，它由一些精简而高效的库组成，为 Node.js提供了基本的API。接下来，将选出最常用的核心模块加以详细介绍，主要内容包括：
<!--more-->
+ 全局对象，
+ 常用工具，
+ 事件机制，
+ 文件系统访问，
+ HTTP服务器与客户端。

全局对象
--------
JavaScript 中有一个特殊的对象，称为全局对象（Global Object），它及其所有属性都可以在程序的任何地方访问，即全局变量。在浏览器 JavaScript 中，通常 window 是全局对象，而 Node.js 中的全局对象是 global，所有全局变量（除了global本身以外）都是global对象的属性。
我们在 Node.js 中能够直接访问到对象通常都是 global 的属性，如 console、process等，下面逐一介绍。

全局对象与全局变量
------------------
global 最根本的作用是作为全局变量的宿主。按照 ECMAScript 的定义，满足以下条件的变量是全局变量：
+ 在最外层定义的变量，
+ 全局对象的属性，
+ 隐式定义的变量(未定义直接赋值的变量)。

当你定义一个全局变量时，这个变量同时也会成为全局对象的属性，反之亦然。需要注意的是，在 Node.js中你不可能在最外层定义变量，因为所有用户代码都是属于当前模块的，而模块本身不是最外层上下文。

>永远使用 var 定义变量以避免引入全局变量，因为全局变量会污染命名空间，提高代码的耦合风险。

process
-------
process 是一个全局变量，即 global 对象的属性。它用于描述当前 Node.js进程状态的对象，提供了一个与操作系统的简单接口。通常在你写本地命令行程序的时候，少不了要和它打交道。下面将会介绍 process 对象的一些最常用的成员方法。
1. process.argv是命令行参数数组，第一个元素是 node， 第二个元素是脚本文件名，从第三个元素开始每个元素是一个运行参数。
``` js
    console.log(process.argv);
```
将以上代码存储为argv.js,通过以下命令运行：
``` 
$ node argv.js 1991 name=byviod --v "Carbo kuo"
```

2. process.stdout是标准输出流，通常我们是使用console.log()向标准输出打印字符，process.stdout.write()函数提供了更底层的接口。
3. process.stdin是标准输入流，初始时它的是被暂停的，要想从标准输入读取数据，你必须恢复流，并手动编写流的事件响应函数。

``` js
process.stdin.resume();
process.stdin.on('data',function(){
    process.stdout.write('read from console:' + data.toString());
});
```

4. process.nextTick(callback)的功能是为事件循环设置一项任务，Node.js会在下次事件循环调响应时调用callback.

console
-------
console 用于提供控制台标准输出，它是由 Internet Explorer 的 JScript 引擎提供的调试工具，后来逐渐成为浏览器的事实标准。 Node.js 沿用了这个标准，提供与习惯行为一致的console 对象，用于向标准输出流（stdout）或标准错误流（stderr）输出字符。
+ console.log()：向标准输出流打印字符并以换行符结束。 console.log 接受若干个参数，如果只有一个参数，则输出这个参数的字符串形式。如果有多个参数，则以类似于 C 语言 printf() 命令的格式输出。第一个参数是一个字符串，如果没有参数，只打印一个换行。
+ console.error()：与 console.log() 用法相同，只是向标准错误流输出。
+ console.trace()：向标准错误流输出当前的调用栈。
