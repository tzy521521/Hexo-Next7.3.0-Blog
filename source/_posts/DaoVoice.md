---
title: DaoVoice-Hexo博客添加在线联系功能
date: 2019-09-02 15:00:59
tags:
categories:
---
Hexo博客如何添加在线联系功能呢?[DaoVoice](http://www.daovoice.io/)是一个绝佳的客户沟通工具。

最终的效果可以参考我博客的右下角,有个聊天的按钮,效果如下所示:

{% asset_img chat-style.jpeg %}

<!-- more -->
也可以参考[EZLippi的博客](https://www.ezlippi.com/blog/2018/01/next-chat.html)
# 注册DaoVoice
首先到[DaoVoice官网](http://www.daovoice.io/)上注册一个账号,注册完成后会得到一个app_id，获取app_id的步骤如下图所示:

{% asset_img daovoice.png 如何获取app_id %}


# 更改主题配置文件
以next主题(版本是7.3.0)为例,在/themes/next/layout/_third-party/chat下添加daovoice.swig文件，代码如下：
```twig
{% if theme.daovoice.enable %}
  <script>
    (function(i,s,o,g,r,a,m){i["DaoVoiceObject"]=r;i[r]=i[r]||function(){(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;a.charset="utf-8";m.parentNode.insertBefore(a,m)})(window,document,"script",('https:' == document.location.protocol ? 'https:' : 'http:') + "//widget.daovoice.io/widget/0f81ff2f.js","daovoice")
    daovoice('init', {
      app_id: "{{theme.daovoice.app_id}}"
    });
    daovoice('update');
  </script>
{% endif %}
```
更改/themes/next/layout/_third-party/chat/index.swig中的代码：
```twig
{% include 'chatra.swig' %}
{% include 'tidio.swig' %}
{% include 'daovoice.swig' %}
```
接着打开主题配置文件_config.yml，添加如下代码：
```yaml
# DaoVoice
daovoice:
  enable: true
  app_id: #这里输入前面获取的app_id
```
需要注意的是,next主题下聊天的按钮会和其他按钮重叠到一起，可以到聊天设置，修改下按钮的位置:

{% asset_img chat.png %}

最后到右上角选择管理员，微信绑定,可以绑定你的微信号，关注公众号后打开小程序，就可以实时收发消息，有新的消息也会通过微信通知，设置页面如下:

{% asset_img wechat.png %}