---
title: 媒体查询表达式详解
date: 2016-08-25
tags:
- 媒体查询
categories: css3
---

## 媒体查询

媒体查询在响应式网站中必不可少，直接来看一个媒体查询表达式：

{% codeblock lang:css %}
@media all and (min-width: 800px) and (orientation: landscape) {
  backgroud-color: #fff;
}
{% endcodeblock %}

这是一条常见的css3媒体查询的表达式，{}里面是需要执行的css样式。
当表达式的最终结果为真的时候，执行{}里面的样式，否则不执行。

{% blockquote %}
<code>@media</code> - - - -  这个标识表示该表达式是媒体查询
<code>all</code> - - - -  表示所有媒体类型
<code>and</code> - - - -  连接操作符，表示与，即所有条件都要为真
<code>orientation: landscape</code> - - - -  表示是否横屏
{% endblockquote %}

当然除了and连接操作符，还有or，not，only等操作符

{% blockquote %}
<code>or</code> - - - -  表示或，即只要有一个条件为真即可。
<code>,</code> - - - -  逗号跟or一样，也是表示或
{% endblockquote %}

<code>and</code>和<code>or</code>操作符都是比较好理解的，那么<code>not</code>呢
请看下面这串表达式：

{% codeblock lang:css %}
@media not all and (min-width: 800px) {
  backgroud-color: #fff;
}
/* 等价于 */
@media not (all and (min-width: 800px)) {
  backgroud-color: #fff;
}
{% endcodeblock %}

从上面的例子看出来<code>not</code>操作符是对后面整体的一个否定
后面整体为真，表达式结果即为假；后面整体为假，表达式结果即为真！

但是当遇到<code>or</code>或者<code>,</code>的时候就不一样了
这种情况下not的范围只管到or或者,之前。例如：

{% codeblock lang:css %}
@media not all and (min-width: 800px),print and (color) {
  backgroud-color: #fff;
}
/* 等价于 */
@media not all and (min-width: 800px) or print and (color) {
  backgroud-color: #fff;
}
/* 等价于 */
@media (not (all and (min-width: 800px))) or print and (color) {
  backgroud-color: #fff;
}
{% endcodeblock %}

<code>only</code>---这个操作符表示仅仅，用于针对某一种特定的媒体类型
主要是为了防止一些不支持媒体查询的浏览器而引入了样式。

## 兼容移动端

在移动设备中，我们一般都会增加以下代码来兼容：

{% codeblock lang:html %}
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
{% endcodeblock %}

通过以上这段代码来设置移动端视口

## 低版本浏览器

在IE8等低版本浏览器中不支持HTML5和CSS3 media，我们需要加载两个js来兼容

{% codeblock lang:html %}
<!--[if lt IE 9]>
  <script src="https://oss.maxcdn.com/libs/html5shiv/3.7.0/html5shiv.js"></script>
  <script src="https://oss.maxcdn.com/libs/respond.js/1.3.0/respond.min.js"></script>
<![endif]-->
{% endcodeblock %}

## 设置IE渲染方式默认最高

有些人使用的是IE9浏览器，但是浏览器的文档模式确实IE8：
为了防止这种情况，加入下面这段代码来保证IE的文档模式是最新的：

{% codeblock lang:html %}
<meta http-equiv="X-UA-Compatible" content="IE=edge, chrome=1">
{% endcodeblock %}

其中<code>chrome=1</code>表示**Google Chrome Frame（谷歌内嵌浏览器框架GCF）**如果有的用户电脑里面装了这个chrome的插件，就可以让电脑里面的IE不管是哪个版本的都可以使用Webkit引擎及V8引擎进行排版及运算，无比给力，不过如果用户没装这个插件，那这段代码就会让IE以最高的文档模式展现效果。

参考链接：[css3 media用法总结](http://www.360doc.com/content/16/0826/09/36048877_586003161.shtml)


