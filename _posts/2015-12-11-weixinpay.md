---
layout: post
title: "wechatpay-v3.3.7"
description: ""
category: wechat
tags: [wechat,php]
---
{% include JB/setup %}

# 微信支付的实现V3.3.7

_ _ _

- 下载demo
- 修改配置
- 说明

_ _ _

## 下载demo
1. [demo地址](https://res.wx.qq.com/paymchres/zh_CN/htmledition/download/bussiness-course3/wxm-payment-biz-api218f8e.zip)
2. 解压使用其中的php版本内的代码，将之布置在环境中

_ _ _

## 修改配置

1. 修改./WxPayPubHelper/WxPay.pub.config.php

1.1 【基本信息设置】
商户向微信提交企业以及银行账户资料，商户功能审核通过后，可以获得帐户基本信息，找到本例程的配置文件「WxPay.pub.config.php」，配置好如下信息：
	appId：微信公众号身份的唯一标识。审核通过后，在微信发送的邮件中查看。
	Mchid：受理商ID，身份标识
	Key:商户支付密钥Key。审核通过后，在微信发送的邮件中查看。
	Appsecret:JSAPI接口中获取openid，审核后在公众平台开启开发模式后可查看。

 1.2【native支付链接设置】
 native支付中，用户扫码后调微信会将productid和用户openid发送到商户设置的链接上，确保该链接与实际服  务路径一致。本例程的响应服务为「./demo/native_call.php」

 1.3【JSAPI路径设置】
 通过JSAPI发起支付的代码应该放置在商户设置的「支付授权目录」中。
 并找到本例程的配置文件「WxPay.pub.config.php」，配置正确的路径。

 1.4【证书路径设置】
 找到本例程的配置文件「WxPay.pub.config.php」，配置证书路径。

 1.5【异步通知url设置】
 找到本例程的配置文件「WxPay.pub.config.php」，配置异步通知url。

 1.6【必须开启curl服务】
 使用Crul需要修改服务器中php.ini文件的设置，找到php_curl.dll去掉前面的";"即可。

 1.7【设置curl超时时间】
 本例程通过curl使用HTTP POST方法，此处可修改其超时时间，默认为30秒。找到本例程的配置文件「WxPay.pub.config.php」，配置curl超时时间。

2 修改./demo/js_api_call.php中的
{% highlight php %}
function jsApiCall() {
   WeixinJSBridge.invoke(
   'getBrandWCPayRequest',
   <?php echo $jsApiParameters; ?>,
         function (res) {
            if (res.err_msg == "get_brand_wcpay_request:ok") {
                        // message: "微信支付成功!",
                        window.location.replace("成功后想跳转的地址，记得地址前得加http://");
                    }else if (res.err_msg == "get_brand_wcpay_request:cancel") {
                        // message: "已取消微信支付!"
                        window.location.replace("支付取消后想跳转的地址,记得地址前得加http://");
                    }
                }
            );
        }
{% endhighlight %}

*这里得特别解释下，WxPay.pub.config.php中的NOTIFY_URL是支付成功后会将信息传入的地址，但并不会在成功后让前端挑转到NOTIFY_URL的地址上*

_ _ _

## 说明

*注意，这个demo本身有几个bug，在WxPayPubHelper.php的163行的
curl_close($ch);
是多余的；
还有几处的
curl_setopt($ch, CURLOPT_TIMEOUT, $second);
中的CURLOPT_TIMEOUT写成了CURLOP_TIMEOUT，少了个T，记得修改*

_ _ _
然后？然后就运行试下看看如何


