---
title: 事件绑定
date: 2016-08-07 15:06:51
categories: 笔记
tags: [JS,React.js]
keywords: React.js,JavaScript
description: 事件绑定
---
在文章开头先提一下React的事件绑定，React 里只需把事件处理器（event handler）以骆峰命名（camelCased）形式当作组件的 props 传入即可，就像使用普通 HTML 那样。React 内部创建一套合成事件系统来使所有事件在 IE8 和以上浏览器表现一致。也就是说，React 知道如何冒泡和捕获事件，而且你的事件处理器接收到的 events 参数与 W3C 规范 一致，无论你使用哪种浏览器。
<!-- more -->

> 如果需要在手机或平板等触摸设备上使用 React，需要调用 React.initializeTouchEvents(true); 启用触摸事件处理。
> 在后续的一篇文章中，我将详细说明一下关于React事件的绑定及最大化公用事件

自动绑定和事件代理
------------------
实际上，React 做了一些操作来让代码高效运行且易于理解。

* Autobinding: 在 JavaScript 里创建回调的时候，为了保证 this 的正确性，一般都需要显式地绑定方法到它的实例上。有了 React，所有方法被自动绑定到了它的组件实例上。React 还缓存这些绑定方法，所以 CPU 和内存都是非常高效。而且还能减少打字！
* 事件代理 ： React 实际并没有把事件处理器绑定到节点本身。当 React 启动的时候，它在最外层使用唯一一个事件监听器处理所有事件。当组件被加载和卸载时，只是在内部映射里添加或删除事件处理器。当事件触发，React 根据映射来决定如何分发。当映射里处理器时，会当作空操作处理。参考 [David Walsh](https://davidwalsh.name/event-delegate) 很棒的文章 了解这样做高效的原因。

在JavaScript中，事件委托是一种很常用也很合理的方法。事件委托可以让你避免添加事件监听器监听特定的节点，相反，事件委托是将事件紧监听放在父级节点上。该事件监听冒泡事件，找到子元素进行匹配。下面举个例子来说明一下事件委托的工作原理：

首先，我们有一些子元素的你UL元素：
``` html
<ul id="parent-list">
    <li id="post-1">Item 1</li>
    <li id="post-2">Item 2</li>
    <li id="post-3">Item 3</li>
    <li id="post-4">Item 4</li>
    <li id="post-5">Item 5</li>
    <li id="post-6">Item 6</li>
</ul>
```


我们也说，有什么需要点击每个子元素时发生。你可以将单独的事件侦听器添加到每个LI元素，但如果LI元素被频繁的从列表中添加和删除呢？添加和删除事件侦听器将是一场噩梦，尤其是如果添加和移除代码在Li的应用程序中不同的地方。更好的解决方案是一个事件监听器添加到父UL元素。但是，如果你添加事件监听到父UL，你将如何知道被点击哪些元素？

```js
// Get the element, add a click listener...
document.getElementById("parent-list").addEventListener("click", function(e) {
    // e.target is the clicked element!
    // If it was a list item
    if(e.target && e.target.nodeName == "LI") {
        // List item found!  Output the ID!
        console.log("List item ", e.target.id.replace("post-", ""), " was clicked!");
    }
});
```



通过增加一个click事件侦听父元素开始。当事件听者被触发，检查事件元件，以确保它的元素反应以类型。如果它是一个LI元素，但是，如果这不是我们想要的元素，该事件可以被忽略。这个例子是非常简单 -  UL和LI是一个直接的比较。让我们尝试一些更加困难。让我们多子元素的父级DIV ，但我们关心的是与ClassA的CSS类的标记

```js
// Get the parent DIV, add click listener...
document.getElementById("myDiv").addEventListener("click",function(e) {
    // e.target was the clicked element
  if (e.target && e.target.matches("a.classA")) {
    console.log("Anchor element clicked!");
    }
});
```

使用[Element.matches](https://davidwalsh.name/element-matches-selector) API ，我们可以看到，如果匹配元素我们所期望的目标。

总结：
-----

由于大多数开发者使用JavaScript库为他们的DOM元素和事件处理，我建议使用事件委托的方法，因为它们能够进行高效的事件委托和元素监听。
此外，希望这篇文章能够帮助你直观的理解事件委托的概念，并能让你知道事件委托的高效。

