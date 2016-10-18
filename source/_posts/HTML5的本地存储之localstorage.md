---
title: HTML5的本地存储之localstorage
date: 2016-04-22 20:47:45
categories: 随笔
tags: [HTML5]
keywords: HTML5
---

开门见山，今天来讲一讲HTML5中的存储方式。前端开发者应该都知道，对于html的本地存储是HTML5中出现的新特性，并不是所有浏览器都支持，在开始写本地存储的时候，必须得检测一下浏览器对本地存储特性的支持性是否良好，根据支持情况选择不同的方案，以免造成不必要的麻烦。<!--more-->
在Web Storage API中，特定域名下的Storage数据库可以直接通过window对象访问。因此首先确定用户的浏览器是不支持Web Storage非常重要。代码示例如下：

``` js
    function checkStorageSupport(){
        if(window.localStorage){
            alert('This browser supports localStorage');
        }else{
            alert('This browser does not supports localStorage');
        }
    }
```

上面只是说明了检测浏览是否支持localStorage，接下来，再详细扒一扒本地存储的API方法:
``` js 
    setItem(key,val);
    removeItem(key);
    getItem(key);
    clear();
    key(index);
```
上面的代码是通过setItem方法设置数据的，key为关键字类似字段名形式，val则表示相对应的值，不过在这里要提一下的是，这种方法设置的的数据都是以字符串的形式存储的。以上原生方法就介绍到这里。
下面我再说说一个非常实用的开源项目store.js，这个开源小库非常实用，那么我们开始吧

首先，先介绍一下关于store.js的简单API：（store.js exposes a simple API for cross browser local storage）
``` js

    // Store 'marcus' at 'username'
    store.set('username', 'marcus')

    // Get 'username'
    store.get('username')

    // Remove 'username'
    store.remove('username')

    // Clear all keys
    store.clear()

    // Store an object literal - store.js uses JSON.stringify under the hood
    store.set('user', { name: 'marcus', likes: 'javascript' })

    // Get the stored object - store.js uses JSON.parse under the hood
    var user = store.get('user')
    alert(user.name + ' likes ' + user.likes)

    // Get all stored values
    store.getAll().user.name == 'marcus'

    // Loop over all stored values
    store.forEach(function(key, val) {
        console.log(key, '==', val)
    })
```
基于这个开源库，我们检测浏览器是否支持本地存储的代码则更为简单了：
``` js
    // store.enabled 返回一个布尔值，如果浏览器支持本地存储则返回true,否则返回false
    if (!store.enabled) {
        alert('Local storage is not supported by your browser. Please disable "Private Mode", or upgrade to a modern browser.');
        return ;
    }

```
以上是store.js的主要API，这些都不是重点，重点是我要基于这个开源小库封装一下自己的一个小方法，对于大部分人来说，或许这步是多余的，如有多余，请勿喷。。。。

由于通过store.js保存的数据是字符串形式的，所以我存储数据时做了如下处理：

``` js
    /**
     * [setStorage 利用store.js改进存储方法，用来存储json对象字符串]
     * @param {[type]} objName [存储的key值名字]
     * @param {[type]} key     [存储json对象的key值]
     * @param {[type]} val     [存储json对象的对应value值]
     */
    var setStorage = function(objName,key,val){
        var _data = {};
        _data[objName] = {};
        _data[objName].data = store.get(objName)||{};
        _data[objName].data[key] = val;
        store.set(objName,_data[objName].data);
    };
```
估计有人看到上面的代码会有疑问，那就是store.js明明有自己的一套获取数据的方法，为什么还要重新写一个呢？
对于这个疑问，就让大家自己先思考一下吧，上面重新写的获取方法或许会对你有帮助哟！


总结：在我被要求优化一个组件的时候，里面有就一条，将数据存储到本地，我就用了store.js这个开源，挺方便的，不过随之在用的过程中遇到了不小的坑，可能是刚开始接触的时候对它的本质不太了解，所以才会掉进坑里去，上面自定义的存储数据方法是我解决坑的方案，用这个方法着实给我减轻了不小的负担。另外，希望这篇文章对前端的你有帮助！