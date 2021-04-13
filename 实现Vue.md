---
title: 实现Vue
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

这两天，抽时间学习了一下网上的Vue代码。同时巩固了一下知识点。

总结一下Vue用到的知识点。
1、fragment片段。将document存储在片段中，渲染完再appendChild；
2、消息发布与订阅。开发on(eventHandler,callback)及emit(eventHandler, param)方法；
3、get set方法劫持，用到了   

``` JavaScript

function hookdata(data,key,value){
      var v = value;
      var watch = this.watch;
      return (function(){
        Object.defineProperty(data,key,{
           enumerable: true,
           configurable: true,
            get(){
              //console.log(v)
              return v;
            },
            set(value){
              v = value;
              watch.emit(key,value);
            }
          });
      })();
    }

```

