---
title: React.js学习笔记(一)初尝react.js魅力
date: 2016-05-04 22:01:51
categories: 笔记
tags: [JS,React.js]
keywords: React.js,JavaScript
description: 初尝react.js魅力
---
今天只是初尝react.js的魅力，先不作深究。
关于react.js，在前端界一直是一个热门话题。我也是一直想去尝试使用这一框架开发项目，只可惜工作的原因一直未得愿。由于，接下来的工作项目中，前端需要重新架构，所以，我就想用react.js这套框架来搭建前端框架用来快速开发。
<!--more-->
HTML 模板
---------
首先，需要建立好目录结构，然后在相应的目录下创建相应的html模板，使用React的网页源码，结构如下：
``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hello React!</title>
    <script src="build/react.js"></script>
    <script src="build/react-dom.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.23/browser.min.js"></script>
</head>
<body>
    <div id="example"></div>
    <script type="text/babel">
      ReactDOM.render(
        <div>
            <h1>Hello, world!</h1>
            <p>呵呵</p>
        </div>,
        document.getElementById('example')
      );
    </script>
</body>
</html>
```

>上面代码有两个地方需要注意。首先，最后一个 script 标签的 type 属性为 text/babel 。这是因为 React独有的 JSX 语法，跟 JavaScript 不兼容。凡是使用 JSX 的地方，都要加上 type='text/babel'。
>其次，上面代码一共用了三个库： react.js 、react-dom.js 和 Browser.js ，它们必须首先加载。其中，react.js 是 React 的核心库，react-dom.js 是提供与 DOM 相关的功能，Browser.js 的作用是将 JSX 语法转为 JavaScript 语法，这一步很消耗时间，实际上线的时候，应该将它放到服务器完成。

``` js
$ babel src --out-dir build
```

上面命令可以将 src 子目录的 js 文件进行语法转换，转码后的文件全部放在 build 子目录.

ReactDOM.render()
-----------------
ReactDOM.render 是 React 的最基本方法，用于将模板转为 HTML 语言，并插入指定的 DOM 节点。
``` jsx
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('example')
);
```

JSX 语法
--------
HTML 语言直接写在 JavaScript 语言之中，不加任何引号，这就是 JSX 的语法，它允许 HTML 与 JavaScript 的混写.
``` jsx
var names = ['Alice', 'Emily', 'Kate'];

ReactDOM.render(
  <div>
  {
    names.map(function (name) {
      return <div>Hello, {name}!</div>
    })
  }
  </div>,
  document.getElementById('example')
);
```

上面代码体现了 JSX 的基本语法规则：遇到 HTML 标签（以 < 开头），就用 HTML 规则解析；遇到代码块（以 { 开头），就用 JavaScript 规则解析。上面代码的运行结果如下。

>JSX 允许直接在模板插入 JavaScript 变量。如果这个变量是一个数组，则会展开这个数组的所有成员（查看

``` jsx
var arr = [
  <h1>Hello world!</h1>,
  <h2>React is awesome</h2>,
];
ReactDOM.render(
  <div>{arr}</div>,
  document.getElementById('example')
);
```

上面代码的arr变量是一个数组，结果 JSX 会把它的所有成员，添加到模板