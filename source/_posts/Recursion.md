---
title: 函数的递归
date: 2016-08-27 23:30:55
categories: 笔记
tags: [JS]
keywords: JS,JavaScript
description: 函数的递归
---
首先，解释一下什么是递归——程序调用自身的编程技巧称为递归（ recursion）。递归做为一种算法在程序设计语言中广泛应用。递归需要有边界条件、递归前进段和递归返回段。当边界条件不满足时，递归前进；当边界条件满足时，递归返回。
<!-- more -->

下面举一个经典的递归函数的例子：
``` js
    function f(n){
        if(n == 0 || n == 1){
            return 1;
        }else{
            return n*f(n-1);
        }
    }
```

那么，根据上面的例子，我们来具体分析一下递归程序的基本步骤：

1. 递归就是方法里调用自身。
2. 在使用递增归策略时，必须有一个明确的递归结束条件，称为递归出口。
3. 使用更简单的子问题重新定义答案。我们可以将答案定义为当前节点的内容加上列表中其余部分的和。
4. 合并结果。递归调用之后，我们将当前节点的值加到递归调用的结果上。

由于在新项目中的需求，我就想到使用递归函数来实现，问题是这样的：

>给你一个数组，数组的每个元素都是一个对象，需要比较每个元素对象里面的两个属性的值是否相同，如果相同，则合并该元素将里面的count计数属性值相加 

下面就贴上答案：

``` js
/**
 * uniqueData
 * arr -> 传入的数组，[注：此数组的每个元素都是一个对象，各元素的属性相同值不同]
 * field -> 传入比较的属性值，[field是一个数组]
 * count -> 传递需要处理的属性键，[注；count属性值必须为number，以作统计]
 * jsonArray -> 存入去重合并后的元素
 */

let jsonArray = [];
// 去重合并
let uniqueData = function(arr,field,count){
    let narr = arr
    if(narr.length > 1){
        let first = narr.shift();
        if(narr.length > 0){
            let next = narr.findIndex((value,index,array)=>{
                return field.map((val,index)=>{
                    return first[val] == value[val]
                }).reduce((prev,next) => prev&&next )
            })
            if(next >= 0){
                first[count] += parseInt(narr[next][count]);
                narr.splice(next,1);
                narr.splice(0,0,first);
                uniqueData(narr,field,count);
            }else{
                jsonArray.push(first);
                uniqueData(narr,field,count);
            }
        }else{
            jsonArray.push(first)
        }
        
    }else{
        jsonArray.push(narr[0])
    }
}

// 模拟数据
var data = [
    {
        count:1,
        selected:0,
        subselectd:1,
        stock:22
    },{
        count:2,
        selected:0,
        subselected:1,
        stock:22
    },{
        count:1,
        selected:1,
        subselected:2,
        stock:11
    },{
        count:3,
        selected:0,
        subselected:1,
        stock:22
    },{
        count:1,
        selected:1,
        subselected:2,
        stock:11
    }
];
uniqueData(data,['selected','subselected'],'count');
console.log(jsonArray);
```

可能以上的代码不是最好的方案，如果有比较好的方案，请多多指教，谢谢！

总结递归算法解决问题的特点：
-----
* 递归就是方法里调用自身。
* 在使用递增归策略时，必须有一个明确的递归结束条件，称为递归出口。
* 递归算法解题通常显得很简洁，但递归算法解题的运行效率较低。所以一般不提倡用递归算法设计程序。
* 在递归调用的过程当中系统为每一层的返回点、局部量等开辟了栈来存储。递归次数过多容易造成栈溢出等，所以一般不提倡用递归算法设计程序。
