---
title: JS实现倒计时
date: 2016-04-22 22:11:24
categories: 随笔
tags: [JS]
keywords: JavaScript
---
很久之前就写了一个时间插件，该插件是根据工作开发过程中的需要写的，关于时间插件，应该有不少人开发过，这里我写了一个个人觉得还算可以的方法吧，不过代码有待优化，一旦功能写好了，除非有bug，不然我也就懒得去优化了。我写了一个倒计时的对象setCountDown.
<!--more-->
首先，我定义了一个倒计时对象，如下：
``` js
    var setCountDown = {};
```
或者定义一个倒计时函数，如下：
``` js
    function setCountDown(){};
```
上面两种方法都可以，也就是使用的方法不一样而已，我这里使用的是第一种方法，如果使用第二种方法，可以使用如下方法：
``` js
    var times = new setCountDown();
    times.prototype.init = function(){
        // code here....
    };
    times.prototype.getSecond = function(){
        // code here....
    };
```
第二种方式我就不再多说了，主要说说我的方法。
首先，我给该对象添加了一个获取时间(单位为秒)的方法getSecond,如下：
``` js
    getSecond:function:(times){
        if(times){
            var year = parseInt(times.slice(0,4)),
                month = parseInt(times.match(/-\d*/gi)[0].replace('-','')-1),
                day = parseInt(times.match(/-\d*/gi)[1].replace('-','')),
                hour = parseInt(times.match(/\d*:/)[0].replace(':','')),
                minute = parseInt(times.match(/:\d*/)[0].replace(':',''));
            return (new Date(year,month,day,hour,minute,0)).getTime()/1000;
        }
        return (new Date()).getTime()/1000;
    }
```
然后我根据获取的的时间秒，另外给对象添加了个获取天数、小时，分钟，秒的方法，如下：
``` js
    getCountdown:function(time){
        var curShowTimeSecondsVal = this.getSecond(time) - this.getSecond();
        if(curShowTimeSecondsVal<0) return [0,'00','00','00'];
        // console.log(curShowTimeSecondsVal)
        // 剩余秒数
        var curShowTimeSeconds = parseInt(curShowTimeSecondsVal%60);
        // 计算剩余天数
        var curShowTimeDays = parseInt(curShowTimeSecondsVal/3600/24);
        // 计算剩余小时
        var curShowTimeHours = parseInt((curShowTimeSecondsVal/3600)) - curShowTimeDays*24;
        // 计算剩余分钟
        var curShowTimeMinutes = parseInt((curShowTimeSecondsVal - parseInt((curShowTimeSecondsVal/3600))*3600)/60);
        curShowTimeHours = curShowTimeHours > 9 ? curShowTimeHours : '0' + curShowTimeHours;
        curShowTimeSeconds = curShowTimeSeconds > 9 ? curShowTimeSeconds : '0' + curShowTimeSeconds;
        curShowTimeMinutes = curShowTimeMinutes > 9 ? curShowTimeMinutes : '0' + curShowTimeMinutes;
        return [curShowTimeDays,curShowTimeHours,curShowTimeMinutes,curShowTimeSeconds];
    }
```
在根据上面的getCountdown获取时间后，根据返回的时间数组显示，接下来写了另外一个显示的方法，如下：
``` js
    setShowTime:function(endtime,done,callback){
        var _this = this;
        // var oSetTime = document.getElementById('time');
        var day = this.getCountdown(endtime)[0],
            hour = this.getCountdown(endtime)[1],
            minute = this.getCountdown(endtime)[2],
            second = this.getCountdown(endtime)[3];
        done([day,hour,minute,second])
        // oSetTime.innerHTML = '剩余时间：'+day+'天'+hour+'小时'+minute+'分'+second+'秒';
        if(day==0&&hour=='00'&&minute=='00'&&second=='00'){
            clearInterval(_this.timer);
            _this.timer =null;
            if(callback) callback();
        }
    }
```
接下来添加一个入口函数，在入口函数设置相应的参数和相应的回调函数，还有一个操作函数，具体代码如下：
``` js
    init:function(opt){
        var _this = this;
        this.setShowTime(opt.endtime,opt.done);
        this.timer = setInterval(function(){
            _this.setShowTime(opt.endtime,opt.done,opt.callback)
        },1000)
    },
```
init函数中的opt的配置如下：
+ endtime -->结束时间
+ done --> 操作函数，用于操作dom，动态显示时间
+ callback --> 回调函数，用于当倒计时结束时的回调，作相应的操作
具体使用方法如下：
``` js
    setCountDown.init({
        endtime:'2016-7-8 00:00',
        done:function(data){
            // console.log(data)
            document.getElementById('time').innerHTML = '剩余时间：'+data[0]+'天'+data[1]+'小时'+data[2]+'分'+data[3]+'秒';
        },
        callback:function(){
            // window.location.reload()
        }
    });
```


最后，下面贴上倒计时的整个代码：
``` js
    /**
     * [setCountDown description]
     * @type {Object}
     * @Author xiege
     * @param init  入口函数
     * @param opt{
     *              endtime:'',
     *              done:function(){},
     *              callback:function(){}
     *           }[endtime:结束时间,done:操作函数,callback:倒计时归零时回调函数]
     */
    var setCountDown = {
            timer:null,
            init:function(opt){
                var _this = this;
                this.setShowTime(opt.endtime,opt.done);
                this.timer = setInterval(function(){
                    _this.setShowTime(opt.endtime,opt.done,opt.callback)
                },1000)
            },
            getCountdown:function(time){
                var curShowTimeSecondsVal = this.getSecond(time) - this.getSecond();
                if(curShowTimeSecondsVal<0) return [0,'00','00','00'];
                // console.log(curShowTimeSecondsVal)
                // 剩余秒数
                var curShowTimeSeconds = parseInt(curShowTimeSecondsVal%60);
                // 计算剩余天数
                var curShowTimeDays = parseInt(curShowTimeSecondsVal/3600/24);
                // 计算剩余小时
                var curShowTimeHours = parseInt((curShowTimeSecondsVal/3600)) - curShowTimeDays*24;
                // 计算剩余分钟
                var curShowTimeMinutes = parseInt((curShowTimeSecondsVal - parseInt((curShowTimeSecondsVal/3600))*3600)/60);
                curShowTimeHours = curShowTimeHours > 9 ? curShowTimeHours : '0' + curShowTimeHours;
                curShowTimeSeconds = curShowTimeSeconds > 9 ? curShowTimeSeconds : '0' + curShowTimeSeconds;
                curShowTimeMinutes = curShowTimeMinutes > 9 ? curShowTimeMinutes : '0' + curShowTimeMinutes;
                return [curShowTimeDays,curShowTimeHours,curShowTimeMinutes,curShowTimeSeconds];
            },
            getSecond:function(times){
                if(times){
                    var year = parseInt(times.slice(0,4)),
                        month = parseInt(times.match(/-\d*/gi)[0].replace('-','')-1),
                        day = parseInt(times.match(/-\d*/gi)[1].replace('-','')),
                        hour = parseInt(times.match(/\d*:/)[0].replace(':','')),
                        minute = parseInt(times.match(/:\d*/)[0].replace(':',''));
                    return (new Date(year,month,day,hour,minute,0)).getTime()/1000;
                }
                return (new Date()).getTime()/1000;
            },
            setShowTime:function(endtime,done,callback){
                var _this = this;
                // var oSetTime = document.getElementById('time');
                var day = this.getCountdown(endtime)[0],
                    hour = this.getCountdown(endtime)[1],
                    minute = this.getCountdown(endtime)[2],
                    second = this.getCountdown(endtime)[3];
                done([day,hour,minute,second])
                // oSetTime.innerHTML = '剩余时间：'+day+'天'+hour+'小时'+minute+'分'+second+'秒';
                if(day==0&&hour=='00'&&minute=='00'&&second=='00'){
                    clearInterval(_this.timer);
                    _this.timer = null;
                    if(callback) callback();
                }
            }
        };
    setCountDown.init({
        endtime:start,
        done:function(data){
            // console.log(data)
            document.getElementById('time').innerHTML = '剩余时间：'+data[0]+'天'+data[1]+'小时'+data[2]+'分'+data[3]+'秒';
        },
        callback:function(){
            // window.location.reload()
            
        }
    })
}
```
总结：写类似这种对象式的插件，一定要注意作用域。