---
title: NodeJS核心模块笔记(九)文件系统 fs
date: 2016-05-03 22:52:03
categories: 笔记
tags: [JS,NodeJS]
keywords: NodeJs,JavaScript
description: Node.js文件系统 fs
---
fs 模块是文件操作的封装，它提供了文件的读取、写入、更名、删除、遍历目录、链接等 POSIX 文件系统操作。与其他模块不同的是， fs 模块中所有的操作都提供了异步的和同步的两个版本，例如读取文件内容的函数有异步的 fs.readFile() 和同步的fs.readFileSync()。我们以几个函数为代表，介绍 fs 常用的功能，并列出 fs 所有函数的定义和功能。
<!--more-->
fs.readFile
-----------
fs.readFile(filename,[encoding],[callback(err,data)])是最简单的读取文件的函数。它接受一个必选参数 filename，表示要读取的文件名。第二个参数 encoding是可选的，表示文件的字符编码。 callback 是回调函数，用于接收文件的内容。如果不指定 encoding，则 callback 就是第二个参数。回调函数提供两个参数 err 和 data， err 表示有没有错误发生， data 是文件内容。如果指定了 encoding， data 是一个解析后的字符串，否则 data 将会是以 Buffer 形式表示的二进制数据。
先来看一个例子：
``` js
var fs = require('fs');
fs.readFile('content.txt', function(err, data) {
    if(err){
        console.log(err);
    }else{
        console.log(data);
    };
});
```

首先，假设content.txt中的内容是UTF-8编码的Text文本文件，则运行结果如下：
```
<Buffer 54 65 78 74 20 e6 96 87 e6 9c ac e6 96 87 e4 bb b6 e7 a4 ba e4 be 8b>
```

上面的输出结果表示的是程序以二进制的模式读取了文件的内容，data的值是Buffer对象。
如果给fs.readFile的encoding指定编码，代码如下：
``` js
var fs = require('fs');
fs.readFile('content.txt','utf-8',function(err,data){
    if(err){
        console.log(err);
    }else{
        console.log(data);
    };
})
```

这样子，输出的结果会是：
```
Text 文本文件示例。
```

当读取文件出现错误时， err 将会是 Error 对象。如果 content.txt不存在，运行前面的代码则会出现以下结果：
``` js
{ [Error: ENOENT, no such file or directory 'content.txt'] errno: 34, code: 'ENOENT',path: 'content.txt' }
```

>Node.js 的异步编程接口习惯是以函数的最后一个参数为回调函数，通常一个函数只有一个回调函数。回调函数是实际参数中第一个是 err，其余的参数是其他返回的内容。如果没有发生错误， err 的值会是 null 或undefined。如果有错误发生， err 通常是 Error 对象的实例。

fs.readFileSync
---------------
fs.readFileSync(filename, [encoding])是 fs.readFile 同步的版本。它接受的参数和 fs.readFile 相同，而读取到的文件内容会以函数返回值的形式返回。如果有错误发生， fs 将会抛出异常，你需要使用 try 和 catch 捕捉并处理异常。

>与同步 I/O 函数不同， Node.js 中异步函数大多没有返回值。

fs.open
-------
fs.open(path, flags, [mode], [callback(err, fd)])是 POSIX open 函数的封装，与 C 语言标准库中的 fopen 函数类似。它接受两个必选参数， path 为文件的路径，flags 可以是以下值
+ r ：以读取模式打开文件。
+ r+ ：以读写模式打开文件。
+ w ：以写入模式打开文件，如果文件不存在则创建。
+ w+ ：以读写模式打开文件，如果文件不存在则创建。
+ a ：以追加模式打开文件，如果文件不存在则创建。
+ a+ ：以读取追加模式打开文件，如果文件不存在则创建。

mode 参数用于创建文件时给文件指定权限，默认是 0666①。回调函数将会传递一个文件描述符 fd②。

>① 文件权限指的是 POSIX 操作系统中对文件读取和访问权限的规范，通常用一个八进制数来表示。例如 0754 表示文件所有者的权限是 7 （读、写、执行），同组的用户权限是 5 （读、执行），其他用户的权限是 4 （读），写成字符表示就是 -rwxr-xr--。
>② 文件描述符是一个非负整数，表示操作系统内核为当前进程所维护的打开文件的记录表索引。

fs.read
-------
fs.read(fd, buffer, offset, length, position, [callback(err, bytesRead,buffer)])是 POSIX read 函数的封装，相比 fs.readFile 提供了更底层的接口。fs.read的功能是从指定的文件描述符 fd 中读取数据并写入 buffer 指向的缓冲区对象。 offset 是buffer 的写入偏移量。 length 是要从文件中读取的字节数。 position 是文件读取的起始位置，如果 position 的值为 null，则会从当前文件指针的位置读取。回调函数传递bytesRead 和 buffer，分别表示读取的字节数和缓冲区对象。

然后，看一个例子：
``` js
var fs = require('fs');
fs.open('content.txt','r',function(err,fd){
    if(err){
        console.error(err);
        return ;
    };
    var buf = new Buffer(8);
    sf.read(fd,buf,0,8,null,function(err,bytesRead,buffer){
        if(err){
            console.error(err);
            return ;
        };
        console.log('bytesRead: ' + bytesRead);
        console.log(buffer);
    })
})
```

上面的代码最终运行结果如下：
``` js
bytesRead: 8
<Buffer 54 65 78 74 20 e6 96 87>
```

注意：
-----
一般来说，除非必要，否则不要使用这种方式读取文件，因为它要求你手动管理缓冲区和文件指针，尤其是在你不知道文件大小的时候，这将会是一件很麻烦的事情。