---
title: vue-router
date: 2016-11-30
tags:
- vue-router
categories: vue
---

### vue-router的基本使用步骤

1.在html中引入vue和vue-router，router-link来导航，router-view用来渲染
```html
<script src="vue.js"></script>
<script src="bower_components/vue-router/dist/vue-router.min.js"></script>

<div id="app">
  <p>hello vue router</p>
  <div>
    <!-- 使用 router-link 组件来导航. -->
    <!-- 通过传入 `to` 属性指定链接. -->
    <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
    <router-link to="/home">Go to Home</router-link>
    <router-link to="/news">Go to News</router-link>
  </div>
  <div>
    <!-- 路由出口 -->
    <!-- 路由匹配到的组件将渲染在这里 -->
    <router-view></router-view> 
  </div>
</div>
```
2.通过4个步骤，定义组件-->配置路由-->生成路由实例-->挂载即可完成
```javascript
 //1.定义组件
  var Home = {
    template: '<h3>我是Home</h3>'
  }

  var News = {
    template: '<h3>我是News</h3>'
  }

  //2.定义配置路由，每个路由通过component属性映射一个组件
  const routes = [//注意这里是routes而不是routers!!!
    { path: '/home', component: Home },
    { path: '/news', component: News }
  ]

  //3.生成路由实例，并传入routers参数
  const router = new VueRouter({
    routes
  })

  //4.挂载到根实例
  new Vue({
    router,
    el: '#app'
  })
```
### 动态路由
有的时候需要不同的路径参数来映射到同一路由，路径参数使用冒号:标记，
匹配到路由时，参数值会被设置到this.$route.params,可以在组件内部使用，如：
```javascript
const User = {
  template: '<div>User {{ $route.params.id }} </div>'
}

const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ]
})
```
动态路由从/user/foo导航到user/bar的时候，组件是被复用的。
这意味组件的生命周期钩子不会被调用。如果要对路由参数变化做出响应，需要使用到watch
```javascript
const User = {
  template: '<div>User {{ $route.params.id }} </div>',
  watch: {
    '$route' (to, from){
      //对路由变化做出响应
    }
  }
}
```
### 嵌套路由
顾名思义，一个路由里面包含着子路由
在路由配置的时候通过children属性指定子路由
```javascript
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id', component: User,
      children: [
        // 当 /user/:id 匹配成功，
        // UserHome 会被渲染在 User 的 <router-view> 中
        { path: '', component: UserHome },

        // ...其他子路由
      ]
    }
  ]
})
```
<script async src="//jsfiddle.net/yyx990803/L7hscd8h/embed/js,html,result/dark/"></script>
