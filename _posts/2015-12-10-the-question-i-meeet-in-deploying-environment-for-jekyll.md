---
layout: post
title: "
The Question I Meeet In Deploying Environment for jekyll
"
description: "not a hard work"
category: install
tags: [question,deloy,jekyll,linux]
---
{% include JB/setup %}


# 在部署jekyll环境遇到的问题

## 前言
***

一直就觉得部署环境是程序员逃不了的一关，所以得当基本功，大部分时候会有现成的环境包使用安装，但在linux配置环境还真是考验耐心，搜索，理解提示信息的时候了

## git
***

- ###提交问题

因为之前一直是用https的形式来clone和push的，发现老是得输入username和password，简直就是烦人，去网上搜下，有一种是在环境中写人username和password，但总是觉得有点别扭，最后才发现原来用ssh来设置url就只会在每次开机的时候push会要求提交验证一次私钥，之后都不会要求输入了，这就好办了，有两种方式可以解决

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

这次jekyll的环境是搭建在ubuntu中的，因为windows下的ruby为1.9.3，无法支持现有的jekyll，而我又不想把自己好不容易搭建好的rails环境给破坏掉，所以才将环境部署到ubuntu中，顺便吸取下windows下的多版本教训，所以就在试着在ubuntu中利用rvm来管理ruby多版本，但用起来问题还是有一些的

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

## jekyll

- ###theme

使用jekyll默认的界面显得有点过于简单，这里使用的是jekyllbootstrap
git的ssh链接如下：

```
git@github.com:plusjade/jekyll-bootstrap.git
```

clone下来后，里面有个比较特别的文件*Rakefile*，通过它可以使用一些简单的命令行来完成一些比较繁琐的操作
例如我要使用jekyllbootstrap里的theme，就可以使用下面的命令很方便地实现theme的下载和布置

```
rake theme:install git="https://github.com/jekyllbootstrap/theme-twitter.git"
```

jekyllbootstrap提供的theme有以下几款

dinky

```
rake theme:install git="git://github.com/sodabrew/theme-dinky.git"
```

hooligan

```
rake theme:install git="https://github.com/dhulihan/hooligan.git"
```

mark-reid

```
rake theme:install git="git://github.com/jekyllbootstrap/theme-mark-reid.git"
```

the-minimum

```
rake theme:install git="https://github.com/jekyllbootstrap/theme-the-minimum.git"
```

the-program

```
rake theme:install git="https://github.com/jekyllbootstrap/theme-the-program.git"
```

tom

```
rake theme:install git="https://github.com/jekyllbootstrap/theme-tom.git"
```

twitter

```
rake theme:install git="https://github.com/jekyllbootstrap/theme-twitter.git"
```

- ###使用theme的问题

不知道是我设置的时候出来了什么问题，只有根目录下的文件才会正确的插入css，其他页面都是找不到theme.name，找了好久，全文检索都没有解决，最后发现在/_includes/JB/setup中是这么写的

{% raw %}
	{% if site.JB.ASSET_PATH %}
      {% assign ASSET_PATH = site.JB.ASSET_PATH %}
    {% else %}
      {% capture ASSET_PATH %}{{ BASE_PATH }}/assets/themes/{{ page.theme.name }}{% endcapture %}
{% endraw %}

也就是说。程序是在找不到JB.ASSET_PATH才会去执行else里的操作，再全文检索一遍

```
$ grep -r 'ASSET_PAHT' ./
```
在输出的内容中很容易发现我们的配置文件*_config.yml*中就有关于ASSET_PATH的设置。我就设置如下

```
ASSET_PATH : /assets/themes/twitter
```

当然由于我使用的theme是twitter才这么写的，得根据自己使用什么theme来设置

- ###语法高亮

jekyllboostarp默认使用pygments

```
$ sudo apt-get install python-pygments
```

这样就安装完毕
输入以下命令就可以看到有多少种风格

```
$ python
```

```
>>> from pygments.styles import STYLE_MAP
```

```
>>> STYLE_MAP.keys()
```

我这边使用的是colorful
所以输入以下命令就能生成语法高亮的css

```
pygmentize -f html -a .highlight -S colorful > code.css
```

这样就会在当前目录发现有个code.css，将之复制到合适的地方，并在需要的地方导入，如defalut.html
之后只要在需要高亮的地方将代码用
{% raw %}
{% highlight ruby linenos %}
{% endhighlight %}
{% endraw %}包裹即可,效果如下


{% highlight ruby linenos %}
def show
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}

- ###关于转义的问题

在jekyll中，{% raw %}{% %}{% endraw %}会直接被编译，所以要使内容不被编译接就得在外层包裹
{% raw %}
{% raw %}
{% endraw %}即可