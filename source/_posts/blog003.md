---
title: uiRoute,angular路由,state
date: 2016-11-18
tags:
- angular
categories: angular
---

由于时间紧迫这里我就不讲ngRouter了，项目中大多也会使用uiRouter，
而且uiRouter包含了ngRouter的所有功能，所以这里我就直接开讲uiRouter了

## 安装使用
```bash
npm install angular-ui-router #通过npm安装
```
安装好之后，在script里面引入即可
```html
<script src="/vendor/angular-ui-router/release/angular-ui-router.min.js"></script>
```

## 依赖和配置
首先定义模块的时候添加上对ui.router的依赖
```javascript
(function(){
  var app = angular.module('app',['ui.router'])
})
```
接着我们配置路由
```javascript
(function(){
  var app = angular.module('app',['ui.router'])
  app.config(['$stateProvider','$urlRouterProvider',function($stateProvider,$urlRouterProvider){
    $stateProvider
      .state('login', {
        url: '/login',
        templateUrl: 'static/partials/login.html',
        controller: 'LoginController',
        controllerAs: 'vm'
      })

  }])
})
```

如上图，首先注入<code>$stateProvider,$urlRouterProvider</code>
然后在config里面通过<code>$stateProvider.state</code>来配置不同的<code>stateName</code>对应的配置
<code>stateConfig</code>包含了很多字段：
```text
template, templateUrl, templateProvider, controller, controllerProvider, resolve, 
url, params, views, abstract, onEnter, onExit, reloadOnSearch, data
```
之后在对其中的一些字段讲解

## state的父子级
```javascript
// 个人中心
  .state('my', {
    url: '/my',
    templateUrl: 'static/partials/my.html'
  })

  // 我的资料
  .state('my.profile', {
    url: '/profile',
    templateUrl: 'static/partials/my.profile.html',
    controller: 'ProfileController',
    controllerAs: 'vm',
    authenticate: true
  })
```
$state.go('my.profile')
$state.go('^')到上一级,比如从my.profile到photo
$state.go('^.list')到相邻state,比如从my.profile到my.list
$state.go('^.detail.comment')到孙子级state，比如从my.detail到my.detial.comment

## 激活state
激活state有三种方式
```text
1.通过$state.go,例如：$state.go('my')
2.html中通过ui-sref，例如<a ui-sref="home" class="navbar-brand">Home</a>
3.通过href，例如<a href="#/home"></a>
```

## 路由接收参数
在<code>stateconfig</code>中通过params属性设置路由参数，
也可以在url中带上参数，例如：
```javascript
.state('my.buy', {
  url: '/buy/{id: [0-9]}/detail/{month}',
  params: {
    code: null
  },
  templateUrl: 'static/partials/my.buy.html',
  controller: 'BuyController',
  controllerAs: 'vm',
  belong: 'trade',
  authenticate: true
})
```
如上的配置，就可以在BuyController中通过$stateParams获取到id，month，以及params中的值了。
如：
```javascript
function BuyController($stateParams){
  console.log($stateParams.id)
  console.log($stateParams.month)
  console.log($stateParams.code)
}
```

## resolve属性
resolve属性的值是一个对象，
这个对象可以被注入到controller中去从而提供使用
如：
```javascript
.state('activities',{
    url: '/activities',
    controller: 'AlLActivitiesController',
    controllerAs: 'activities',
    templateUrl: '/app/templates/allActivities.html',
    resolve: {//这里设置resolve属性
        activities: function(dataService){
            return dataService.getAllActivites();
        }
    }
})


function AllActivitiesController(dataService, notifier, $state, activities){//这里注入resolve中设置的activities
  var vm = this;
  
  vm.selectedMonth = 1;
  vm.allActivities = activities;//使用
  
}
```
其他的属性例如data以及其他的属性，都可以在$state.current来获得当前状态的配置中看到
至此，uirouter大致用法就这么多
参考链接[http://www.cnblogs.com/darrenji/p/4982533.html](http://www.cnblogs.com/darrenji/p/4982533.html)



