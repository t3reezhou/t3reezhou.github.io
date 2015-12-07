---
layout: post
title: "Install Haroopad In Ubuntu"
description: "how to install haroopad in ubuntu"
category: lesson
tags: [haroopad,install ubuntu]
---
{% include JB/setup %}

# Ubntu下的Haroopad安装


## 前言
一直就很不喜欢word和wps的使用方式，至少我就没习惯过，还是markdown的使用方式比较适合于不爱鼠标点击操作的我，可能是程序员的观点在作祟，之前一直在windows下使用haroopad还是很便捷的，最近由于特殊原因得在Ubuntu下工作，但发现这玩意安装起来还真没window下省心，baidu下就只有单纯的编译器介绍而已，官网提供的tar.gz真心不知道怎么操作，并不是简单的解压就可以运行，最后还是得通过google才算是找到比较有价值的安装过程，用haroopad提供的专为debian使用的deb会比较方便安装，不过就得做下配置

[参考链接](http://linuxg.net/how-to-install-haroopad-0-12-2-on-ubuntu-debian-and-derivative-systems/)

## 下载deb包

[下载链接](https://bitbucket.org/rhiokim/haroopad-download/downloads)

![下载页面](/public/1.png)

进入页面看看自己想要的版本，注意这里我们选择以.deb的文件，可下载在主文件夹下，即home/user/目录下，当然也可以用下面的命令来下载，例如我想下载图中橙色选中的文件，就可以这么操作

```
$ wget https://bitbucket.org/rhiokim/haroopad-download/downloads/haroopad-v0.13.1-x64.deb
```

就会下载到主文件夹中


## 配置安装deb包所需要的组件

接着我们需要能运行deb包的环境

```
$ sudo apt-get install gdebi
```

## 安装

```
sudo gdebi haroopad-v0.13.1-x64.deb
```

因为我在上面下载的是haroopad-v0.13.1-x64.deb，根据自己下载改变上面命令吧
之后在终端运行

```
$ haroopad
```

即可运行
效果如下

![效果图](/public/2.png)

## 可能会遇到的问题

```
/usr/lib/x86_64-linux-gnu/libnss3.so:version 'NSS_3.14.3' not found
```

遇到上述情况是haroopad需要使用的nss比较高，本地版本低，这时只需要升级一下本地的nss即可

```
sudo apt-get install libnss3
```

这样就能升级nss
下面操作也能达到同样的效果(应该是更多)

```
sudo apt-get update
```

