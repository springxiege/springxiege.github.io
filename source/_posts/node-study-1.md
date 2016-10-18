---
title: NodeJS学习笔记(二)快速入门
date: 2016-04-26 22:27:56
categories: 笔记
tags: [JS,NodeJS]
keywords: NodeJs,JavaScript
description: Node.js—创建http服务器。
---
快速入门，利用Node.js建立http服务器。
------------------------------------
运行 Node.js 程序的基本方法就是执行 node app.js，其中 app.js是脚本的文件名。Node.js 提供的http 模块，可简单快速的创建http服务器。
<!--more-->
首先，建立一个名为app.js的文件，内容如下：
``` js
var http = require('http');
http.createServer(function(req,res){
    res.writeHead(200,{'Content-Type':'text/html'});
    res.write('<h1>Node.js</h1>');
    res.end('<p>Hello World</p>');
}).listen(3000);
console.log('HTTP server is listening at port 3000');
```
然后，打开cmd命令面板，定位到项目根目录下，我的项目目录是G:GitDateBase/NodeJS,直接输入命令：
```
node app.js
```
命令成功后，cmd命令面板上为打印出“HTTP server is listening at port 3000”，打开浏览器输入http://127.0.0.1:3000 ，即可看到页面的输出任务。

>小技巧——使用 supervisor

在开发 Node.js 实现的 HTTP 应用时会发现，无论你修改了代码的哪一部份，都必须终止Node.js 再重新运行才会奏效。这是因为 Node.js 只有在第一次引用到某部份时才会去解析脚本文件，以后都会直接访问内存，避免重复载入，Node.js的这种设计虽然有利于提高性能，却不利于开发调试，因为我们在开发过程中总是希望修改后立即看到效果，而不是每次都要终止进程并重启。
supervisor 可以帮助你实现这个功能，它会监视你对代码的改动，并自动重启 Node.js。使用方法很简单，首先使用 npm 安装 supervisor：
```
$ npm install -g supervisor
```
安装完成后，可输入以下命令启动应用程序：
``` js
$ supervisor app.js
```

回调函数
--------
首先，我们创建一个读取文件的js文件，这里命名为readfile.js（根据个人爱好自取），内容如下：
``` js
// readfile.js
var fs = require('fs');
fs.readFile('file.json','utf-8',function(err,data){
    if(err){
        console.error(err); //如果获取文件出错，则抛出错误
    }else{
        console.log(data); //如果获取文件成功，则打印文件数据
    }
})
console.log('end');
```
以上代码正确执行后的结果就是file.json里面的内容。

总结：
-----
Node.js创建服务器是使用内建http模块，还有其他的内建模块等着继续去摸索学习！