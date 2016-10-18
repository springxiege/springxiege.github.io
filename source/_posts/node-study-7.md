---
title: NodeJS核心模块笔记(八)事件驱动 events
date: 2016-05-02 22:14:13
categories: 笔记
tags: [JS,NodeJS]
keywords: NodeJs,JavaScript
description: Node.js事件驱动 events
---
Node.js的事件驱动 events
------------------------
events 是 Node.js 最重要的模块，没有“之一”，原因是 Node.js 本身架构就是事件式的，而它提供了唯一的接口，所以堪称 Node.js 事件编程的基石。 events 模块不仅用于用户代码与 Node.js 下层事件循环的交互，还几乎被所有的模块依赖。
<!--more-->
事件发射器
----------
events 模块只提供了一个对象： events.EventEmitter。 EventEmitter 的核心就是事件发射与事件监听器功能的封装。EventEmitter 的每个事件由一个事件名和若干个参数组成，事件名是一个字符串，通常表达一定的语义。对于每个事件， EventEmitter 支持若干个事件监听器。当事件发射时，注册到这个事件的事件监听器被依次调用，事件参数作为回调函数参数传递。
先来看个例子：
``` js
var events = require('events');
var emitter = new events.EventEmitter();
emitter.on('someEvent',function(arg1,arg2){
    console.log('linstener1',arg1,arg2);
});
emitter.on('someEvent',function(arg1,arg2){
    console.log('listener2', arg1, arg2);
});
emitter.emit('someEvent', 'xiege', 1989);
```

运行结果：
``` js
listener1 xiege 1989
listener2 xiege 1989
```

分析下以上js代码：
>emitter 为事件 someEvent 注册了两个事件监听器，然后发射了someEvent事件。运行结果中可以看到两个事件监听器回调函数被先后调用。

上面就是EventEmitter最简单的用法，接下来，列出EventEmitter常用的API：
+ EventEmitter.on(event, listener) 为指定事件注册一个监听器，接受一个字符串 event 和一个回调函数 listener。
+ EventEmitter.emit(event, [arg1], [arg2], [...])发射event事件，传递若干可选参数到事件监听器的参数表。
+ EventEmitter.once(event, listener)为指定事件注册一个单次监听器，即监听器最多只会触发一次，触发后立刻解除该监听器。
+ EventEmitter.removeListener(event, listener)移除指定事件的某个监听器，listener必须是该事件已经注册过的监听器。
+ EventEmitter.removeAllListeners([event])移除所有事件的所有监听器，如果指定event，则移除指定事件的所有监听器。

error 事件
----------
EventEmitter 定义了一个特殊的事件 error，它包含了“错误”的语义，我们在遇到异常的时候通常会发射 error 事件。当 error 被发射时， EventEmitter 规定如果没有响应的监听器， Node.js 会把它当作异常，退出程序并打印调用栈。我们一般要为会发射 error事件的对象设置监听器，避免遇到错误后整个程序崩溃。

继承 EventEmitter
-----------------
大多数时候我们不会直接使用 EventEmitter，而是在对象中继承它。包括 fs、 net、http 在内的，只要是支持事件响应的核心模块都是 EventEmitter 的子类。
为什么要这样做呢？原因有两点。首先，具有某个实体功能的对象实现事件符合语义，事件的监听和发射应该是一个对象的方法。其次 JavaScript 的对象机制是基于原型的，支持部分多重继承，继承 EventEmitter 不会打乱对象原有的继承关系。