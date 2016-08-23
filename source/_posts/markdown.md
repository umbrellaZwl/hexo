---
title: markdown 语法简介(学习使用hexo)
date: 2016-08-19 22:11:24
tags:
- markdown
- hexo开始
categories: markdown
---

# *标题*
在文本前面加上 # 就可以设置标题格式，也可以增加一级标题，二级标题~~~~六级标题
{% codeblock %}
# 标题1
## 标题2
## 标题3
{% endcodeblock %}

<pre>
var data = [1,2,3];
data.forEach(function(){
  alert(1);
})
</pre>

{% codeblock lang:js %}
  var data = [1,2,3];
  data.forEach(function(){
    alert(1);
  })
{% endcodeblock %}

{% pullquote %}
content
{% endpullquote %}

# *列表*

在文本前面加上 <code>-</code> 即可设置列表格式，如果是有序列表直接加上1. 2. 就好

{% codeblock %}
- 列表1
- 列表2
- 列表3
1. 列表4
2. 列表4
3. 列表4
{% endcodeblock %}

# *插入链接和图片*

*插入链接* 只需要 [显示文本](链接地址) 这样的语法就好
*插入图片* 只需要 [显示文本](链接地址) 这样的语法就好,比插入链接多一个!

这是一个[hexo](https://hexo.io/zh-cn)的尝试-----[umbrella](http://localhost:4000/)

{% img /images/test.jpg  200 200 This is an example image %}


# *引用*
> 万丈红尘，纷扰不休
> 初会便已许平生


# *表格*
**相关代码：**

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

