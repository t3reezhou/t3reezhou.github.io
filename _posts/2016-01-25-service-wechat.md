---
layout: post
wordpress_id: Setting_Service_wechat_2016_01_25
title: "Setting Service wechat"
description: ""
category: wechat
tags: [wechat]
---
{% include JB/setup %}

# 配置微信服务号

和企业号比较起来，服务号的配置相对简单得多了，个人认为需要配置的也就

1.菜单栏

2.网页授权获取用户基本信息

3.URL(服务器地址)

## 菜单栏

如果你没有开启服务器配置的话，那你完全就可以左侧菜单栏里选择自定义菜单来设置，但如果你开启的话，你就没法在开启的状态下操作菜单栏，你可以通过代码来实现，当然你也可以通过微信自己做的[api测试界面](http://mp.weixin.qq.com/debug/)来做到自定义菜单的效果

点击上面的链接

![配置菜单栏-1](/public/swechat160125/set-menu-1.png)


填写appid和secret，不清楚的在公众号设置界面左侧菜单栏点击基本配置，就能看到如下的界面

![配置菜单栏-2](/public/swechat160125/set-menu-2.png)

如果该公众号开启了安全管理的话，那就找你老板或者账号管理员通过验证后就能看到。

将内容输入后，提交就可以获取token（*注意如果你做了token的缓存的话，而你这边又获取了一遍token，那缓存的token就会无效，用定时器的要特别注意哈*）

接着我们再改变下"接口类型"和"接口列表",如下

![配置菜单栏-3](/public/swechat160125/set-menu-3.png)

将获取到的token复制进框中

body部分是用json来设定你想要的菜单栏，格式如下

	{
     "button":[
     {
          "type":"click",
          "name":"语音识别",
          "key":"V1001_TODAY_MUSIC"
      },
      {
          "type":"click",
          "name":"获取关注者信息",
          "key":"V1001_SUBSCRIBE"
      },
      {
           "name":"菜单",
           "sub_button":[
           {
               "type":"view",
               "name":"搜索",
               "url":"http://www.soso.com/"
            },
            {
               "type":"view",
               "name":"视频",
               "url":"http://v.qq.com/"
            },
            {
               "type":"click",
               "name":"赞一下我们",
               "key":"V1001_GOOD"
            }]
       }]
 	}

url的设置为跳转页面，key值为回调模式会接收到的值。
虽然我在上一篇的企业号配置有写到关于带code跳转的设置，其实都一样，但我还是再强调一次

普通的跳转只用把域名或者ip写上就可，但如果希望跳转的同时能获取到用户信息的话，那就得用下面的形式填写

	https://open.weixin.qq.com/connect/oauth2/authorize?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE&state=STATE#wechat_redirect

将其中的APPID换成上图的appid，REDIRECT_URI换成你要跳转的url（*注意该url需要进行urlEncode，而且记得带上http或者https*），其他的可以不用改，当然，如果你想附加信息进去的话，那可以将STATE替换为你要传递的内容（*同样也需要urlencode*），这里还有一个和企业号的区别，就是SCOPE，服务号这边有两种选择

1.snsapi_base

2.snsapi_userinfo

两者的区别在于，snsapi_base不需要用户授权既可以获取用户的openId，而snsapi_userinfo会弹出授权页面，如果用户同意的话，那跳转的页面会带code，否则不带，而snsapi_userinfo可以根据code获取到用户的openId，并获取用户信息，当然，snsapi_base也可以获取用户信息，但如果用户未关注的话，那snsapi_base只能走到获取用户openId那步，无法继续获取用户的其他信息。

假设我替换REDIRECT_URI为www.sample.com
保存完之后，用户点击该按钮
那么我们是会先跳转到微信那边，微信根据可信域名和appid来判断是否授予code，通过的话，微信会将code写入url，并将用户推到该url中，该url大致如下

	http://www.sample.com?code=CODDE&state=STATE

之后我们根据获取的code就可以获取用户的openId。

## 网页授权获取用户基本信息

上述的可信域名其实就是我现在要讲得"网页授权获取用户基本信息",该设置需要公众号进行认证后才能够使用，具体设置如下

点击右侧菜单栏的接口权限

![配置可信域名](/public/swechat160125/set-snapi-1.png)

点击修改即可设置（我没去申请认证，所以。。。将就下）

![配置可信域名](/public/swechat160125/set-snapi-2.png)

在这写上你要授权的域名，不需要带http和https，也是到一级目录就可，点击确认即可

## URL(服务器地址)

其实就是对应企业号的回调模式，由于这里可以设置为明文，所以我们可以不需要加解密来完成验证，而这就简单了

点击左侧菜单栏的基本配置

![配置回调模式-1](/public/swechat160125/set-backcall-1.png)

点击修改配置就可以进行配置

![配置回调模式-2](/public/swechat160125/set-backcall-2.png)

url为你需要处理用户事件的链接，token和EncodingAESKey同样是为了验证使用的，为了方便验证我们选择明文模式，点击提交微信就会发个get给url，我们只需将echostr参数拿到自己返还给微信就可以了，具体操作可看[接入指南](http://mp.weixin.qq.com/wiki/home/index.html)





