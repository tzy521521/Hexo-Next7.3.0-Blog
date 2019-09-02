---
title: DaoVoice-Hexo博客添加在线联系功能
date: 2019-09-02 15:00:59
tags:
categories:
---
Hexo博客如何添加在线联系功能呢,发现了一个不错的网站可以提供在线联系的服务，当有用户在网页上给你留言后会通过邮件或者微信通知你，可以及时的解答用户的疑问。

最终的效果可以参考我博客的右下角,有个聊天的按钮,效果如下所示:

{% asset_img chat-style.jpeg 如何获取app_id %}

<!-- more -->

# 配置方法
首先到[DaoVoice官网](http://www.daovoice.io/)上注册一个账号,注册完成后会得到一个app_id，获取app_id的步骤如下图所示:

{% asset_img daovoice.png 如何获取app_id %}


# 更改配置文件
以next主题为例,在/themes/next/layout/_third-party/chat下添加daovoice.swig文件，代码如下：
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

{% asset_img chat.png 如何获取app_id %}