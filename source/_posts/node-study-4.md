---
title: NodeJS学习笔记(五)调试
date: 2016-04-29 22:43:20
categories: 笔记
tags: [JS,NodeJS]
keywords: NodeJs,JavaScript
description: Node.js调试
---
写程序时免不了遇到 bug，Node.js的调试功能正是由V8提供的，保持了一贯的高效和方便的特性。尽管你也许已经对原始的调试方式十分适应，而且有了一套高效的调试技巧，但今天还是要介绍一下Node.js内置的工具和第三方模块来进行单步调试。
<!--more-->
命令行调试
----------

>Node.js支持命令行下的单步调试，下面来介绍一个简单的例子：

``` js
var a = 1;
var b = 'world';
var c = function(x){
    console.log('hello' + x + a);
};
c(b);
```
在命令行下执行 node debug debug.js,将会启动调试工具

远程调试
----------
>V8 提供的调试功能是基于 TCP 协议的，因此 Node.js 可以轻松地实现远程调试。在命令行下使用以下两个语句之一可以打开调试服务器：

```
node --debug[=port] script.js
node --debug-brk[=port] script.js
```

node --debug 命令选项可以启动调试服务器，默认情况下调试端口是 5858，也可以使用 --debug=1234 指定调试端口为 1234。使用 --debug 选项运行脚本时，脚本会正常执行，但不会暂停，在执行过程中调试客户端可以连接到调试服务器。如果要求脚本暂停执行等待客户端连接，则应该使用 --debug-brk 选项。这时调试服务器在启动后会立刻暂停执行脚本，等待调试客户端连接。

>事实上，当使用 node debug debug.js 命令调试时，只不过是用 Node.js 命令行工具将自动完成相关任务。

使用 node-inspector 调试 Node.js
--------------------------------
大部分基于 Node.js 的应用都是运行在浏览器中的，例如强大的调试工具 node-inspector。node-inspector 是一个完全基于 Node.js 的开源在线调试工具，提供了强大的调试功能和友好的用户界面，它的使用方法十分简便。
首先，使用以下命令安装node-inspector,
```
npm install -g node-inspector
```

然后在终端中通过以下命令连接需要除错的脚本调试服务器,
```
node --debug-bre=5858 debug.js
```
进而启动node-inspector:
```
$ node-inspector
```

在浏览器中打开 http://127.0.0.1:8080/debug?port=5858 ，即可显示出优雅的 Web 调试工具。

总结：
-----
node-inspector 的使用方法十分简单，和浏览器脚本调试工具一样，支持单步、断点、调用栈帧查看等功能。无论你以前有没有使用过调试工具，都可以在几分钟以内轻松掌握。
另外，需要我们注意的是：
>node-inspector 使用了 WebKit Web Inspector，因此只能在 Chrome、Safari等 WebKit 内核的浏览器中使用，而不支持 Firefox 或 Internet Explorer。