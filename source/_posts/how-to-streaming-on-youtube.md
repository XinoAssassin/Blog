---
title: 怎样在境外网站上进行推流
tags:
  - Tutorial
date: 2019-08-06 17:07:15
---


其实很简单，只需要将obs的网络连接引向代理，但是Shadowsocks中直接挂全局模式不太好，因为会将本来能够直连的网站也会通过代理连接，而且应该是不能代理到obs的（我从来不用全局）。

那么要怎么做到这点呢？我常用的是[Proxifier](https://www.proxifier.com/)，使用教程网上很多，这边不再赘述，只要在规则里面把obs的执行文件加入需代理列表即可，各种使用RTMP协议的直播网站应该都是直接可以这样搞定。

<img src="/images/proxifier-rule.png" width="100%" />

当然，一个高带宽的代理是必须条件。

题外话，无论是Youtube还是Twitch，其直播后台都比Bilibili强太多了。

<img src="/images/youtube-back.png"  width="100%" />