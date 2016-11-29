---
title: vue1.0组件
date: 2016-11-28
tags:
- vue1.0组件
categories: vue
---

## vue1.0组件基本步骤：
### 1. 创建组件构造器：
```javascript
var Aaa = Vue.extend({
  template: '<h3>我是组件</h3>'
})
```
如上首先利用<code>Vue.extend</code>来创建组件构造器，相当于扩展了Vue这个构造函数(类)

### 2.注册组件
```javascript
//全局注册组件
Vue.component('aaa',Aaa)
```
这样的话，我们就可以直接使用在html里面直接使用aaa这个组件了
<script async src="//jsfiddle.net/umbrellazwl/kfr4xbef/embed/js,html,result/dark/"></script>

### 局部注册
```javascript
//局部注册组件
var vm=new Vue({
  el:'#box',
  components:{ //局部组件
    aaa:Aaa
  }
});
```
<script async src="//jsfiddle.net/umbrellazwl/kfr4xbef/1/embed/js,html,result/dark/"></script>
当然我们也可以把定义和注册一次写好，而不用分开写，如：
```javascript
Vue.component('aaa',{
  template: '<h3>我是组件</h3>',
  data(){
    return {
      msg: '定义和注册'
    }
  }
})
```
### 模板解析
除了使用Vue.extend来定义，还可以使用template来解析，如下
<script async src="//jsfiddle.net/umbrellazwl/kfr4xbef/4/embed/js,html,result/dark/"></script>

### 父子组件通信之子组件使用父组件数据---props
“prop” 是组件数据的一个字段，期望从父组件传下来。子组件需要显式地用 props 选项 声明 props：
在父组件aaa中传递一个mmm的prop，并且这个mmm的值是aaa数据中的msg，如：
```html
<template id="aaa">
  <div>
    <h3>{{msg}}</h3>
    <bbb :mmm="msg"></bbb>
  </div>
</template>
<template id="bbb">
  <h3>我是子组件-->{{mmm}}</h3>
</template>
```
在javascript中需要显示声明
```javascript
var vm=new Vue({
  el:'#box',
  components: {
    'aaa': {
      template: '#aaa',
      data(){
        return {
          msg: '我是父组件'
        }
      },
      components: {
        'bbb': {
          template: '#bbb',
          props: ['mmm'],
          data(){
            return {
            
            }
          }
        }
      }
    }
  }
});

```
<script async src="//jsfiddle.net/umbrellazwl/kfr4xbef/9/embed/js,html,result/dark/"></script>

### 父子组件通信之父组件获取子组件数据---通过自定义事件
<script async src="//jsfiddle.net/umbrellazwl/kfr4xbef/12/embed/js,html,result/dark/"></script>





