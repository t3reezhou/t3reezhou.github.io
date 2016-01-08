---
layout: post
title: "thinkphp for wechat.1"
description: ""
category: wechat
tags: [thinkphp,php]
---
{% include JB/setup %}

# 微信支付的实现V3.3.7

# thinkphp+微信开发----小结(1)

## 前言

虽然之前有用过thinkphp的1.*版本，想不到才1年多没关注就出到3.2.3去了，总体看了一下，变化挺大，不过很明显就是逐渐向zend靠拢的趋势，用起来问题还是蛮多的，现在论坛的功能已经走出一大步了，做下小结，以备需要。

### 在url中不写入入口文件index.php就无法访问到相应的页面？

其实这是个问题并不普遍，大多数开发者选择用windows开发时用的是集成环境xampp，里面集成的apache已经默认配置了好多模块了，而由于我是在ubuntu下开发，显然这些东西都得自己手工配置，我下面要配置的是为了解决
像

	http://serverName/index.php/Home/Index/hello

这样是可以访问
而像

	http://serverName/Home/Index/hello

却没法访问
究其原因是apache没有配置mod-rewirte模块
解决方式操作如下：
1.

	$ sudo a2enmod rewrite
2.

    $ sudo vim /etc/apache2/site-enable/000-default

修改AllowOverride None改为All
3.重启apache2

	$/etc/init.d/apache2 restart

### thinkphp的redirect无法跳出框架

也就是只能在contorl之间进行跳转，我试过用

	$this->redirect('http://www.taobao.com', array('cate_id' => 2), 5, '页面跳转中...');

这是按他官方文档来写的，但无论如何都跳不出去，目前没收到bug修复或者解答，最后还是用php原生的header轻松解决

	header('location:url');

### 没有后缀的如“.php”之类的无法在“网页授权获取用户基本信息”里进行设置

话说这个问题应该挺久了，不知道微信修正了没，反正我自己的折中解决方案是做出来了，也不在乎它怎么改了。其实调用个.php对于原生来的没什么问题，但框架的话，如果跳到框架里的话并没法通过路径来访问.php的内容，既然如此，那解决方案就是不跳入框架，也就是不调用index.php，在根目录建立自己需要用到的php即可，这样就可以让微信能访问到该文件
