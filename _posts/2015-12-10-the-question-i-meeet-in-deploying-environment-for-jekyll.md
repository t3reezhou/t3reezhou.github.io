---
layout: post
title: "
The Question I Meeet In Deploying Environment for jekyll
"
description: "not a hard work"
category: install
tags: [question,deloy,jekyll]
---
{% include JB/setup %}

# 在部署jekyll环境遇到的问题

## 前言

## git
***

- ###提交问题

因为之前一直是用https的形式来clone和push的，发现老是得输入username和password，简直就是烦人，去网上搜下，有一种是在环境中写人username和password，但总是觉得有点别扭，最后才发现原来用ssh来设置url就只会在第一次验证私钥，之后都不会要求输入了，这就好办了，有两种方式可以解决

1 在clone的时候用的是ssh而不是https，即

```
$ git clone git@github.com:username/projectname.git
```

2 打开.git/config将url设置如下

```
url = git@github.com:username/projectname.git
```

- ###关于jekyll的git操作

实际上并不像网上说的一定得放置到gh-pages分支上，那只是针对project来说的，如果project的name为username.github.io的话，那么直接提交的master分支就可以了，这里有一点要注意，如果你在github的帐号没有设置邮箱验证的话，github将发送邮件提示无法编译，需通过验证才能使用。

## rvm
***

这次的jekyll的环境是搭建在ubuntu中的，因为windows下的ruby为1.9.3，无法支持现有的jekyll，而我又不想把自己好不容易搭建好的rails环境给破坏掉，所以才将环境部署到ubuntu中，顺便吸取下windows下的多版本教训，所以就在试着在ubuntu中利用rvm来管理ruby多版本，但用起来问题还是有一些的

- ####找不到ruby？

rvm和ruby都安装正常，但是重启终端问题就来了，竟然说找不到ruby！！！最后才知道原来ubuntu的终端在启动的时候并不login，而rvm是在bash登录的时候才加载，所以治标的方法如下

```
$ bash --login
```
还有另外的方式，我嫌麻烦，就用治标的方法就行吧，可以参考以下方法

**[手把手安装RVM以及为什么RVM is not a function](https://ruby-china.org/topics/3705)**

- ###gem install安装失败

具体报什么错我忘记记录了，就是关于libxml2的库找不到，安装了就ok（事实是你就是不知道怎么缺的东西的全名是什么，怎么安装...）

```
$ sudo apt-get install libxsl-dev libxml2-dev
```

未完待续