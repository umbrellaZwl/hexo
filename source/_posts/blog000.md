---
title: 配置多个git账号ssh
date: 2016-08-25
tags:
- git
categories: git
---

# 单个账号的ssh配置

### 第一步：检查是否已有SSH密钥
一般SSH密钥会在<code>c:/Users/you/.ssh</code>目录下，查看此目录下是否有类似以下文件:
- id_dsa.pub
- id_ecdsa.pub
- id_ed25519.pub
- id_rsa.pub

### 第二步：生成SSH密钥
{% codeblock lang:js %}
  var data = [1,2,3];
  data.forEach(function(){
    alert(1);
  })
{% endcodeblock %}