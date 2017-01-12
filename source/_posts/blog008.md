---
title: react-lifecycle
date: 2017-1-12
tags:
- react-lifecycle
categories: react
---

好久没有写博客，最近一直学习react的东西，当自己入个小门吧。
今天就来分享一下react当中十分重要的，关于组件的生命周期吧！

### 组件生命周期
1.挂载组件
<code>componentWillMount</code> 组件即将挂载，会在render之前执行，这个阶段组件的dom结构还没有生成，无法获取DOMNode。
由于是在render之前执行，所以可以在这个周期函数里面进行一些初始的操作，比如获取数据了之后去更新state。
<code>componentDidMount</code> 组件已经挂载，会在render之后执行，此时dom结构已经生产，可以通过<code>this.getDOMNode</code>
获取原生dom节点，给dom绑定事件监听等。
```javascript
getInitialState: function(){
  console.log('getInitialState')
  return null
},
getDefaultProps: function(){
  console.log('getDefaultProps')
},
componentWillMount: function(){
  console.log('componentWillMount')
},
componentDidMount: function(){
  console.log('componentDidMount')
  console.log( this.getDOMNode() )
},
render: function(){
  console.log('render')
}
```
以上代码的执行结果：
```
getDefaultProps-->getInitialState-->componentWillMount-->render-->componentDidMount
```

2.卸载组件
<code>componentWillUnmount</code> 组件即将被卸载，在执行<code>React.unmountComponentAtNode( 原生dom节点 )</code>之前
会调用该周期函数,在该函数中，用来执行各种清除操作。比如组件当中定义的setInterval。
```javascript
componentWillUnmount: function(){
  console.log('kill me 0.0')
  clearInterval(this.timer)
}
```

3.更新组件
<code>componentWillUpdate</code> 组件即将更新时调用
<code>componentDidUpdate</code> 组件更新后调用,这两个周期都可以获取到dom节点
<code>shouldComponentUpdate</code> 在组件更新之前判断是否应该更新，最终需要返回一个boolean，例如：
```javascript
shouldComponentUpdate:function(nextProp,nextState){
  if(nextProp.count> this.props.count) return false;
  return true;
}
```
<code>componentWillReceiveProps</code> 子组件从父组件获得了props时会调用




