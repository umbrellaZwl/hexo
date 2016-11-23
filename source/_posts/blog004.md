---
title: 自定义指令
date: 2016-11-23
tags:
- angular
categories: angular
---

## 基本用法
angular中可以通过
```javascript
m1.directive('aaa', function(){
  return {
    restrict: 'AE',
    replace: true,
    templateUrl: ''
  }
})
```
来创建自定义指令
用法如上，对于return出来的对象中的参数，这里不做过多讲解。
主要讲一下指令和指令的交互，以及和dom的交互。

## link
link也是return中的一个key，值是一个函数，如：
```javascript
m1.directive('aaa', function(){
  return {
    restrict: 'AE',
    replace: true,
    templateUrl: ''，
    link: function(scope, element, attrs, reController){
      console.log(attrs)
      element.bind('mouseenter', function(){
        console.log('鼠标进入了...')
      })
      element.bind('mouseleave', function(){
        console.log('鼠标离开了...')
      })
    }
  }
})
```
如上所示，link对应一个函数，函数一共有4个参数：
<code>scope</code> ----- 该指令的作用域
<code>element</code> ----- 该指令解析后的顶层元素
<code>attrs</code> ----- 该指令接收到的属性
<code>reController</code> ---- 父控制器
这样我们就可以在中通过
```html
<aaa from='a' to='b'>hello custom directive</aaa>
```
使用这个指令了。其中的from和to会在attrs里面。

## scope及其绑定策略
自定义指令中还有个属性叫做scope，默认值为false，表示继承父作用域
为<code>true</code> ---- 表示继承父作用域，并创建自己的作用域(子作用域)
为<code>{}</code>   ---- 表示创建一个全新的隔离作用域

很多时候我们都会取值为隔离作用域
顺之，在隔离作用域中会有三种绑定策略
@ ---- 使用@，表示字符串绑定
= ---- 使用=，进行变量双向绑定
& ---- 使用&，调用父作用域中的函数

例如：
1.使用@绑定字符串
```javascript
m1.directive('hello',function(){
  return {
    restrict: 'AE',
    scope: {
      from: '@'
    },
    template: '<div>{{from}}</div>'
  }
})
```
```html
<hello from='aaa'></hello>
```
如此就可以在页面中看到一个div里面内容是aaa了。

2.使用=绑定变量
```javascript
m1.controller('MyCtrl', ['$scope', function($scope){
  $scope.parentfrom="aaa";
}])
m1.directive("hello", function() {
    return {
      restrict:'AE',
      scope:{
        from:'='
      },
      template:'<input type="text" ng-model="from"/>'
    }
});
```
```html
<div ng-controller="MyCtrl">
  Ctrl:
  <br>
  <input type="text" ng-model="parentfrom">
  <br>
  Directive:
  <br>
  <hello from="parentfrom"></hello>
</div>
```
如此就对from这个变量绑定了

3.使用&来调用父作用域的函数
```javascript
m1.controller('MyCtrl', ['$scope', function($scope){
  $scope.sayHello=function(name,age){
    alert("Hello "+name + age);
  }
}])
m1.directive("greeting", function() {
  return {
    restrict:'AE',
      scope:{
        greet:'&'
      },
      template:'<input type="text" ng-model="userName" /><br/>'+
               '<input type="text" ng-model="age" /><br/>'+
           '<button class="btn btn-default" ng-click="greet({name:userName,age:age})">Greeting</button><br/>'
  }
});
```
```html
<div ng-controller="MyCtrl">
  <greeting greet="sayHello(name,age)"></greeting>
  <greeting greet="sayHello(name,age)"></greeting>
  <greeting greet="sayHello(name,age)"></greeting>
</div>
```
如此便可以调用父作用略的函数了

## 在link中使用第4个参数parentControllerInstance
```javascript
m1.directive('outerDirective', function() {
  return {
    scope: {},
    restrict: 'AE',
    controller: function($scope) {      
      this.say = function(someDirective) { 
         console.log('Got:' + someDirective.message);
      };
    }
   };
});
m1.directive('innerDirective', function() {
  return {
    scope: {},
    restrict: 'AE',
    require: '^outerDirective',
    link: function(scope, elem, attrs, controllerInstance) {
      scope.message = "Hi,leifeng";
      controllerInstance.say(scope);
   }
  };
});
```
如上在内部指令中link的第4个参数就是父作用域中controller的实例。
同时注意require属性的值。
<code>^</code>表示如果果在当前的指令没有找到所需的控制器，则会查找父元素的控制器
<code>?</code>如果在当前的指令没有找到所需的控制器，则会将undefined传给link连接函数的第四个参数
没有前缀，指令会在自身提供的控制器中进行查找，如果找不到任何控制器，则会抛出一个error

