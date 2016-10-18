---
title: NodeJS学习笔记(四)包
date: 2016-04-28 21:52:15
categories: 笔记
tags: [JS,NodeJS]
keywords: NodeJs,JavaScript
description: Node.js模块和包
---
对于Node.js的包的理解，是非常抽象的。对于这个，我刚开始接触的时候，也是一头雾水，那先来看看Node.js里面是怎么定义包的。
包是模块基础上更深一步的抽象，Node.js的包类似C/C++的函数库或者Java/.Net的类库。它的将某个独立的功能封装起来，用于发布、更新、依赖管理和版本控制。Node.js根据CommonJS规范实现了包机制，开发了npm来解决包的发布和获取需求。
<!--more-->
>Node.js的包是一个目录，其中包含一个JSON格式的包说明文件pagkage.json。严格符合CommonJS规范的包应该具备以下特征：

+ package.json必须在包的顶层目录下;
+ 二进制文件应该在bin目录下;
+ JavaScript代码应该在lib目录下;
+ 文档应该在doc目录下;
+ 单元测试应该在test目录下。

Node.js对包的要求并没有这么严格，只要顶层目录下有package.json，并符合一些规范即可。当然为了提高兼容性，还是建议在制作包的时候，严格遵守CommonJS规范。

作为文件夹的模块
----------------
模块与文件是一一对应的。文件不仅可以是JavaScript代码或二进制代码，还可以是一个文件夹。最简单的包，就是一个作为文件夹的模块。下面我们举个例子，创建一个somepackage的文件夹，然后在其中创建index.js文件，内容如下：

``` js
// somepackage/index.js
exports.hello = function(){
    console.log('Hello');
};
```
然后在somepackage文件夹之外创建一个getpackage.js文件，内容如下：
``` js
// getpackage.js
var somepackage = require('./somepackage');
somepackage.hello();
```
运行node getpackage.js,控制台将输出结果：hello.
我们使用这种方法可以把文件夹封装为一个模块，即所谓的包。包通常是一些模块的集合，在模块的基础上提供了更高层的抽象，相当于提供了一些固定接口的函数库。通过定制package.json,我们可以创建更复杂、更完善、更符合规范的包用于发布。

>细说package.json

在前面例子中的 somepackage 文件夹下，我们创建一个叫做 package.json 的文件，内容如下所示：
``` json
{
    "main":"./lib/interface.js"
}
```
然后将index.js重命名为interface.js并放入lib子文件夹下，然后我将interface.js里面的输出内容改为hello,mood!。以同样的方式再次调用这个包，依然可以正常使用。终端输出结果为：hello,mood!

> Node.js 在调用某个包时，会首先检查包中 package.json 文件的 main 字段，将其作为包的接口模块，如果 package.json 或 main 字段不存在，会尝试寻找 index.js 或 index.node 作为包的接口。

package.json 是 CommonJS 规定的用来描述包的文件，完全符合规范的 package.json 文件应该含有以下字段:
+ name:包的名称，必须是唯一的，由小写英文字母、数字和下划线组成，不能包含空格。
+ description：包的简要说明。
+ version：符合语义化版本识别规范的版本字符串。
+ keywords：关键字数组，通常用于搜索。
+ maintainers：维护者数组，每个元素要包含 name、 email （可选）、 web （可选）字段。
+ contributors：贡献者数组，格式与maintainers相同。包的作者应该是贡献者数组的第一个元素。
+ bugs:提交bug的地址，可以是网址或者电子邮件地址。
+ licenses：许可证数组，每个元素要包含 type （许可证的名称）和 url （链接到许可证文本的地址）字段。
+ repositories：仓库托管地址数组，每个元素要包含 type（仓库的类型，如 git ）、url （仓库的地址）和 path （相对于仓库的路径，可选）字段。
+ dependencies：包的依赖，一个关联数组，由包名称和版本号组成。

现在，给出一个完全符合CommonJS规范的package.json示例：
``` json
{
    "name":"mypackage",
    "description":"Sample package for CommonJS. This package demonstrates the required elements of a CommonJS package.",
    "version":"0.7.0",
    "keywords":[
        "package",
        "example"
    ],
    "maintainers":[
        {
            "name":"Springxiegel",
            "email":"springxiege@sina.cn"
        }
    ],
    "contributors":[
        {
            "name":"XieGe",
            "web":"http://springxiege.github.io"
        }
    ],
    "bugs":{
        "mail":"1953744343@qq.com",
        "web":"http://springxiege.github.io"
    },
    "licenses":[
        {
            "type":"GPLv2",
            "url":"http://springxiege.github.io"
        }
    ],
    "repositories":[
        {
            "type":"get",
            "url":"http://springxiege.github.io"
        }
    ],
    "dependencies":[
        {
            "webkit":"1.2",
            "ssl":{
                "gnutls":["1.0","2.0"],
                "openssl":"0.9.8"
            }
        }
    ]
}
```

？疑问：如果将package.json里面的main字段改成sub字段，但是我又想获取sub字段的模块，该怎么获取呢？
-----------------------------------------------------------------------------------------------
对于这个疑问，我也是一头雾水弄不明白，看来只能搞搞清楚后再作解答了。

