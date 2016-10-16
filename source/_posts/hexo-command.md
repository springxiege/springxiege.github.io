---
title: hexo常用命令
date: 2016-07-14 20:21:45
categories: 随笔
tags: [hexo]
keywords: hexo,hexo常用命令
description: hexo常用命令，帮助使用hexo搭建博客。
---
很久没来写博客了，感觉一下子对hexo的命令生疏了不少，所以才有了这篇文章，下面罗列一下关于hexo的一些常用的命令
<!-- more -->
>hexo的安装、升级以及初始化

``` js
npm install hexo -g  #安装
npm update hexo -g  #升级
hexo init  #初始化
```

>hexo命令的简写方式，使用简写可以省下不少事

``` js
hexo n "博客名称"  => hexo new "博客名称"   #这两个都是创建新文章，前者是简写模式
hexo p  => hexo publish 
hexo g  => hexo generate  #生成
hexo s  => hexo server  #启动服务预览
hexo d  => hexo deploy  #部署
```

>关于hexo的服务器命令

``` js
hexo server   #Hexo 会监视文件变动并自动更新，无须重启服务器。
hexo server -s   #静态模式
hexo server -p 5000   #更改端口
hexo server -i 192.168.1.1   #自定义IP
hexo clean   #清除缓存，网页正常情况下可以忽略此条命令
hexo g   #生成静态网页
hexo d   #开始部署
```

>监视文件变动

``` js
hexo generate   #使用Hexo生成静态文件
hexo generate --watch   #监视文件变动
```

>模版

``` js
title: 使用Hexo搭建个人博客
layout: post
date: 2014-03-03 19:07:43
comments: true
categories: Blog
tags: [Hexo]
keywords: Hexo, Blog
description: 生命在于折腾，又把博客折腾到Hexo了。给Hexo点赞。
```

>设置文章摘要，即更多按钮

``` js
以上是文章摘要<!-- more -->以下是余下全文
```

>关于模版里的一些变量

| 变量        |描述                             |
|:------------|:--------------------------------|
| :title      | 标题                            |
| :year       | 建立的年份（4 位数）            |
| :month      | 建立的月份（2 位数）            |
| :i_month    | 建立的月份（去掉开头的零）      |
| :day        | 建立的日期（2 位数）            |
| :i_day      | 建立的日期（去掉开头的零）      |

>关于搭建hexo博客中的一些报错的解决方法

1. 找不到git部署
``` js
ERROR Deployer not found : git
```
解决方案
``` js
npm install hexo-deployer-git --save
```

2. 部署类型设置git

hexo 3.0 部署类型不再是github,而是修改_config.yml文件，如下：
``` js
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: git@***.github.com:***/***.github.io.git
  branch: master
```