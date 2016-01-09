---
layout: post
title: "matching-phone"
description: ""
category: wechat
tags: [wechat,css]
---
{% include JB/setup %}

# 关于适配手机的问题

其实在开启项目之前是准备用jquery mobile来解决屏幕适配的问题，但看了下其他人用后的效果，界面全部被撑开，也就是说那玩意就只能用手机看而已，也就是说我如果想在浏览器看到比较正常的界面就得做成两套？虽然我确实是懒，但在有些地方还是有点强迫，我这次毕业设计如果只是为了完成的话，那完全可以不用顾忌手机端以外其他终端的情况，但自己又不想留下太多遗憾，最后决定还是要做成两边兼顾，这就确定我得放弃jquery mobile这套现成的工具，最终摸索中发现bootstarp结合html5的特性还是能胜任这一要求，做到手机和pc界面不会出现较大差异，下面做下小结。

## 禁止界面缩放

>在移动设备浏览器上，通过为视口（viewport）设置 meta 属性为 user-scalable=no 可以禁用其缩放（zooming）功能。这样禁用缩放功能后，用户只能滚动屏幕，就能让你的网站看上去更像原生应用的感觉。

在head标签之中添加 viewport 元数据标签即可。

		<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">

## 字数过多变为省略号

运用css中的

	        overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;

即可解决，当然这里还要说点小技巧，当存在多个对同一个对象（java思维不知道html这边怎么说）的同一属性设定，那就会存在相互碾压，当然这里有权值的概念，但如果一味用
！import和id来获取更高得权限势必会影响其他同样使用了该样式的控件，我的做法是

1.发现在head写的样式无效时，在chrome中按f12，查看自己所写的样式被哪个样式给碾压。
2.将自己写的样式起一个类名，并按碾压者的有效范围来写影响标签
3.将之前被碾压的样式复制进这个类里
4.将新的类赋给之前需要改变的标签

额，本来是想给个实际例子的，可发现那情况还真的不知道怎么模拟出来，算了，改天再遇到一定会贴上来的。

## 英文不换行？！

之前做的实验，中文和英文都会在遇到padding界限的时候就自动换行的，但有次无意中做了个实验，输入一长串的英文竟然是不换行的，之前都是按正常的一个单词一个空格的做法是一点问题都没有的，这就有点麻烦了，你不可能说阻止用户滚键盘式的输入，那本身就是毫无意义的输入，还好在css还是有相应的解决方案的

            word-break: break-all;
            overflow: auto;

至此适配所会遇到的问题也算是初步解决的
代码已发布到github

https://github.com/t3reezhou/bnuzweixin

*（注：由于里面有我的本地配置文件，所以我就删该本地文件了，Application\Home\Conf\config.php
就是这个问题，大家可以自己创建一个，具体配置请看thinkphp的官方文档*

暂时做到如下的效果

[展示](http://1.bnuzweixin.sinaapp.com/)