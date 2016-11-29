---
title: vue2.0变化
date: 2016-11-29
tags:
- vue2.0变化
categories: vue
---

### 组件定义
可以直接使用对象来代替Vue.extend
```javascript
var Component = {
  template: '#aaa',
  data(){
    return {
      msg: '我是组件'
    }
  }
}
```
然后就可以注册使用了

### 组件模板解析
在vue2.0中组件模板不能写片段代码，所有的代码必须包含在一个根元素当中

### 生命周期
```text
vue1.0生命周期
  init  
  created
  beforeCompile
  compiled
  ready   
  beforeDestroy 
  destroyed
```

```text
vue2.0生命周期
  beforeCreate  组件实例刚刚被创建,属性都没有
  created 实例已经创建完成，属性已经绑定
  beforeMount 模板编译之前
  mounted 模板编译之后，代替之前ready  *
  beforeUpdate  组件更新之前
  updated 组件更新完毕  *
  beforeDestroy 组件销毁前
  destroyed 组件销毁后
```

### v_for循环
1.在2.0当中默认可以展示重复数据，无需使用track-by="$index"
2.语法更贴近原生js，类似于<code>arr.forEach</code>,如：
```html
<!-- 在1.0中 -->
<ul>
  <li v-for="(index,val) in json"></li>
</ul>
<!-- 在2.0中 -->
<ul>
  <li v-for="(val,index) in json"></li>
</ul>
```
3.在2.0中使用<code>:key="index"</code>来代替<code>track-by</code>提升性能
```html
<ul>
  <li v-for="(val,index) in json" :key="index"></li>
</ul>
```

### 自定义键盘信息
```javascript
Vue.config.keyCodes.ctrl = 17
```
### 过滤器
1.在2.0中系统自带的过滤器全部废弃
2.自定义过滤器传参的方式有修改，类似于函数传参，如：
```html
<div>{{msg | toDou(12)}}</div>
```
### 单一事件管理组件通信(包含非父子组件通信)
首先新建一个空的vue实例
```javascript
var Event = new Vue()
```
在需要向外传播数据的地方利用<code>Event.$emit</code>来触发自定义事件
```javascript
Event.$emit('a-msg',this.msg);
```
在需要接收数据的组件中利用mouted钩子函数，在组件编译完成之后
利用<code>Event.$on</code>来监听自定义事件
```javascript
mounted(){
  //接收A组件的数据
  Event.$on('a-msg',function(a){
      this.a=a;
  }.bind(this));

  //接收B组件的数据
  Event.$on('b-msg',function(a){
      this.b=a;
  }.bind(this));
}
```
<script async src="//jsfiddle.net/umbrellazwl/50wL7mdz/3586/embed/js,html,result/dark/"></script>
