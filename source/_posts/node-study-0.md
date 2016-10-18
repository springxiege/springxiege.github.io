---
title: NodeJS学习笔记(一)从零开始
date: 2016-04-25 21:55:51
categories: 笔记
tags: [JS,NodeJS]
keywords: NodeJs,JavaScript
description: Node.js学习笔记。
---
从零起点学习NodeJS。学习资源来源于一本电子档的《NodeJS开发指南》，知道有Node存在不是一天两天了，可是从来没有系统去学习过这门语言，虽然它也属于JS，但是区别还是挺大的，想要学好Node,首先就要清楚Node是什么，它的能做些什么？
<!--more-->
    
JS是为客户端而生的，Node.js是为网络而生。Node.js能做的远不止开发一个网站那么简单，使用Node.js，你可以轻松开发：
+ 具有复杂逻辑的网站；
+ 基于社交网络的大规模Web应用；
+ Web Socket服务器；
+ TCP/UDP套接字应用程序；
+ 命令行工具；
+ 交互式终端程序；
+ 带有图形用户界面的本地应用程序；
+ 单元测试工具；
+ 客户端JavaScript编译器。

Node.js内建的HTTP服务器支持
---------------------------
当你使用 Node.js 时，不用额外搭建一个 HTTP 服务器，因为 Node.js本身就内建了一个。这个服务器不仅可以用来调试代码，而且它本身就可以部署到产品环境，它的性能足以满足要求。

>Node.js 最大的特点就是采用异步式 I/O 与事件驱动的架构设计。

``` js
    db.query('SELECT * from some_table', function(res) {
        res.output();
    });
```
这段代码中 db.query 的第二个参数是一个函数，我们称为回调函数。进程在执行到db.query 的时候，不会等待结果返回，而是直接继续执行后面的语句，直到进入事件循环。当数据库查询结果返回时，会将事件发送到事件队列，等到线程进入事件循环以后，才会调用之前的回调函数继续执行后面的逻辑。
Node.js 的异步机制是基于事件的，所有的磁盘 I/O、网络通信、数据库查询都以非阻塞的方式请求，返回的结果由事件循环来处理。图1-1 描述了这个机制。 Node.js 进程在同一时刻只会处理一个事件，完成后立即进入事件循环检查并处理后面的事件。这样做的好处是，CPU 和内存在同一时间集中处理一件事，同时尽可能让耗时的 I/O 操作并行执行。对于低速连接攻击， Node.js 只是在事件队列中增加请求，等待操作系统的回应，因而不会有任何多线程开销，很大程度上可以提高 Web 应用的健壮性，防止恶意攻击。

Node.js快速安装
---------------
可以在官方网站下载对应系统最新的node版本进行安装，安装程序这里省略(大家都懂的)。。。

安装 Node 包管理器
------------------
Node 包管理器（npm）是一个由 Node.js 官方提供的第三方包管理工具，就像 PHP 的Pear、 Python 的 PyPI 一样。 npm 是一个完全由 JavaScript 实现的命令行工具，通过 Node.js 执行，因此严格来讲它不属于 Node.js 的一部分。在最初的版本中，我们需要在安装完 Node.js以后手动安装npm。

>但从 Node.js 0.6 开始， npm 已包含在发行包中了，我们在 Windows、Mac 上安装包和源代码包时会自动同时安装 npm。

如果你是在 Windows 下手动编译的，或是在 POSIX 系统中编译时指定了 --without-npm参数，那就需要手动安装 npm 了。 http://npmjs.org/ 提供了 npm 几种不同的安装方法，通常你只需要执行以下命令：
```
    curl http://npmjs.org/install.sh | sh
```
如果安装过程中出现了权限问题，那么需要在 root 权限下执行上面的语句，或者使用sudo。
```
    curl http://npmjs.org/install.sh | sudo sh
```
其他安装方法，譬如从 git 中获取 npm 的最新分支，可以参考 http://npmjs.org/doc/README.html 上的说明。