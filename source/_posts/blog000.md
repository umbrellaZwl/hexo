---
title: 配置多个git账号ssh
date: 2016-08-25
tags:
- git
categories: git
---

## 单个账号的ssh配置

### 第一步：检查是否已有SSH密钥
一般SSH密钥会在<code>c:/Users/you/.ssh</code>目录下，查看此目录下是否有类似以下文件:
- id_dsa.pub
- id_ecdsa.pub
- id_ed25519.pub
- id_rsa.pub

### 第二步：生成SSH密钥
要生成新的密匙，请在命令终端中执行以下操作。
{% codeblock lang:bash %}
#新建SSH key：
$ cd ~/.ssh     # 切换到C:\Users\Administrator\.ssh
ssh-keygen -t rsa -C "mywork@email.com"  # 新建工作的SSH key
# 设置保存自己的密钥的文件，默认为id_rsa
Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa): [Press enter]
{% endcodeblock %}
接下来，你会被要求键入密码（密码可以不填写，直接回车）：
{% codeblock lang:bash %}
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
{% endcodeblock %}
当输完密码后，你会看到如下提示：
{% codeblock lang:bash %}
Your identification has been saved in /c/Users/you/.ssh/id_rsa.
Your public key has been saved in /c/Users/you/.ssh/id_rsa.pub.
The key fingerprint is:
01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
{% endcodeblock %}

### 第三步：新密钥添加到SSH agent中
{% codeblock lang:bash %}
ssh-agent -s
# Agent pid 59566
ssh-add /c/Users/you/.ssh/id_rsa.pub
{% endcodeblock %}

### 第四步：将你的密匙加入到你的 Github(Gitlab) 帐号中
{% blockquote %}
打开<code>/c/Users/you/.ssh/id_rsa.pub</code>，复制里面的内容
登录你要用的账户的github账号，在导航中选择“Settings”
在二级导航处，选择 "SSH Keys" 选项，点击右上角的 "Add SSH Key" 按钮。
先将你的拷贝内容粘贴到 "Key" 一栏，再填写 "Title" 一栏。
最后别忘记点击 "Add key" 按钮确认。
{% endblockquote %}

### 第五步：设置SSH config
在 <code>C:\用户\你的用户目录\.ssh</code> 目录下新建 config 文件, 文件里录写入下述内容
{% codeblock lang:bash %}
Host github.com
HostName github.com
User umbrellaZwl #你的账户名
PreferredAuthentications publickey
IdentityFile C:\Users\umbrella\.ssh\id_rsa
{% endcodeblock %}

### 第六步：测试生效
在终端中输入：

{% codeblock lang:bash %}
ssh -T git@github.com
{% endcodeblock %}

之后应该会看到以下之类的提示：

{% codeblock lang:bash %}
The authenticity of host 'github.com' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
{% endcodeblock %}

看到这个后输入<code>yes</code>，回车继续即可，
然后就会在<code>C:\用户\你的用户目录\.ssh\konwn_hosts</code>文件里面生成信息。
*注：第一次看到这个，总是以为默认<code>yes</code>，直接回车，然后使用总是出错，默哀！*

以上便是配置ssh的过程，如果要使用多个账号那么就需要按照以上过程再为另一个账号配置一个ssh
不嫌麻烦，咱们在重头来一遍：

## 配置第二个账号SSH

### 第一步：新建user2的SSH Key

{% codeblock lang:bash %}
#新建SSH key：
$ cd ~/.ssh     # 切换到C:\Users\Administrator\.ssh
ssh-keygen -t rsa -C "mywork@email.com"  # 新建工作的SSH key
# 设置保存文件的名称，默认id_rsa，为了区分开，我设置为id_rsa_person
Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa): id_rsa_person
{% endcodeblock %}

### 第二步：新密钥添加到SSH agent中

{% codeblock lang:bash %}
ssh-add ~/.ssh/id_rsa_person
{% endcodeblock %}

如果出现Could not open a connection to your authentication agent的错误，就试着用以下命令：

{% codeblock lang:bash %}
ssh-agent bash
ssh-add ~/.ssh/id_rsa_person
{% endcodeblock %}

### 第三步：将你的密匙加入到你的 Github(Gitlab) 帐号中

参照第一大节中的第四步，很简单，此处偷点懒。。。

### 第四步：修改config文件

针对这个账户添加config里面的配置

{% codeblock lang:bash %}
# user2
Host github.com
HostName github.com
User umbrellaZwl #你的账户名
PreferredAuthentications publickey
IdentityFile C:\Users\umbrella\.ssh\id_rsa
{% endcodeblock %}

### 第五步：测试生效

过程同第一大节中一样，省略。。。

## 具体怎么操作多个账号

### 第一步：生成.git

git 会把所有文件以及历史记录保存在你的项目中，创建一个新的仓库，首先要去到项目路径，执行 git init。然后git会创建一个隐藏的文件夹.git，所有的信息都储存在其中。

### 第二步：配置当前项目的username 和 email

如果不需要切换提交的用户，这一步可以省略，直接使用全局的配置

{% codeblock lang:bash %}
git config --global user.name "My Name"
git config --global user.email "myEmail@example.com"
{% endcodeblock %}

### 第三步：链接远端仓库 - git remote add

{% codeblock lang:bash %}
git remote add origin git@github.com:umbrellaZwl/hexo.git
{% endcodeblock %}

之后就可以使用该账号向该远程仓库做git操作了




