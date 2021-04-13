---
title: 实现Vue
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

这两天，抽时间学习了一下网上的Vue代码。同时巩固了一下知识点。

Vue是一个MVVM模型，各种实现方法在网上也有很多，我就不说相同的了，说说个人看法，在我眼里，其实他只做了三件事。
1. 初次加载编译视图。
>比较麻烦，可能是使用正则或者替换，也需要考虑效率问题。
>
2. 视图改变影响数据。
> 最简单，用oninput()即可实现。
> 
3. 数据改变影响视图。
> hook 数据的set方法
> 

与网上的代码很相似，就不贴相同的了。说一下不同点。
```javascript
function updateNodeValue(_value){
				 //使用正则的方法替换{{ data }}
                  node.textContent=nodetxt.replace( /\{\{((?:.|\n)+?)\}\}/g,
                     function(match,key){
                       if(key.trim()==exp){
                         return _value;
                       } else {
                         return match;
                       }
                     }
                  );
            }
```
使用**正则**的方法替换{{ data }}，而不是全行替换。

```javascript
//事件订阅触发器
function Watch(){
  this.dep={};
}
Watch.prototype.addEventListener=function(eventHandLer,callback){
    if(!this.dep[eventHandLer]){
      this.dep[eventHandLer]=[];
    }
    this.dep[eventHandLer].push(callback);
}
Watch.prototype.on = Watch.prototype.addEventListener;
Watch.prototype.emit=function(eventHandLer,params){
    this.dep[eventHandLer].forEach(function(nodeUpdate){
      nodeUpdate.apply({},params.constructor == Array ?params:[params]);
    });
}
```
使用 **事件订阅触发器** 来传递消息


----------


总结一下自制Vue用到的知识点。
1、fragment片段。将document存储在片段中，渲染完再appendChild；
2、消息发布与订阅。开发on(eventHandler,callback)及emit(eventHandler, param)方法；
3、set方法劫持，用到了   
``` JavaScript
function hookProperties(data,key,value){
      var v = value;
      var watch = this.watch;
      return (function(){
        Object.defineProperty(data,key,{
           enumerable: true,
           configurable: true,
            get(){ 
              return v;
            },
            set(value){
              v = value;
              watch.emit(key,value); //触发更新事件
            }
          });
      })();
    }
```

感觉博主prebra的作图和讲解，所以才能理解vue是如何实现 MVVM 的。
我自己实现的代码，应该会有一些问题，欢迎指正。