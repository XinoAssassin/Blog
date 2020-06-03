---
title: Progressive Web App
tags:
  - Tutorial
date: 2018-01-01 22:26:21
---


## 什么是 PWA

前不久了解到 Progressive Web App 这个在2015年就已经由 Chrome 项目组提出来的概念，[A selection of Progressive Web Apps](https://pwa.rocks/) 这个站点上收录了许多支持 PWA 的网站，不乏有可用性非常高的 [Telegram Web](https://web.telegram.org/) 和知名的 [Flipboard](https://flipboard.com/) 应用的网页版。

PWA 是什么？直译过来就是渐进式网络应用。特性有很多，想要了解具体的直接看 [Progressive Web App](https://developers.google.com/web/progressive-web-apps/), 这里只举我最看重的几点：

- **轻量** & **离线可用**

  跟普通的网页没啥区别，加载快。而且不像 Hybrid App 那样还是依赖于一个本地的 App 壳子，需要你去 App Store 安装

  PWA 在 Android 上保存到 Home Screen 之后就会自动编译生成一个 APK 安装进系统中，也就意味着，它不止是一个网页，而已经成为了一个本地应用，离线状态下也是可用的（当然依赖于网络的东西就不行了）。而这一过程相当快，所要耗费的网络流量也远远小于 Native/Hybrid App

- **本地通知支持**

  在添加在本地之后，PWA 就拥有了本地通知能力，因为在 Android 上是通过 GCM 实现的，所以国内这点并不好用，微博 PWA 就干脆没有写通知的功能

- **All in Browser**

  其实这点就是第一点的补充，一个浏览器干所有的活，不用装那么多又大更新还要开 App Store 的应用。而这一点也包括在所有平台上都有同样的用户体验，虽然目前的 PWA 界面多是为移动设备而设计，但是至少我在 PC 上能一样很方便的用到它，而不是通过虚拟机或者其他手段。

我记得在早期 iPhone 刚发布的时候，Apple 的想推广的就是 Web App, 让用户可以用一个 Safari 干所有的活（可笑的是现在 iOS 还没支持 PWA），可惜当时的前端远远没有今天那么多好用新颖的技术，那时的移动设备性能也满足不了使用非原生代码的开销，所以最后 Apple 妥协了，推出了 App Store 直到今天。而现在 PWA 的推出，多了一个“渐进式”的前缀形容词，没有前几年强推 Web App 的那种势头，更加务实的风格更能被人们接受。

## 目前比较有用的 PWA 站点

- [微博 Lite](https://m.weibo.cn/beta)
- [Bilibili Web 版](https://m.bilibili.com)
- [饿了么](https://h5.ele.me)
- [Twitter Lite](https://mobile.twitter.com/)

其他的可以上 [A selection of Progressive Web Apps](https://pwa.rocks/) 看，虽然好像都是国外的而且好久没有更新了。

## 怎么安装

非常简单，用你的 PC 版 Chrome 或者 Chrome Android 打开上一节的随意一个站点，等待一会儿就会有一条提示你可以添加到主界面的横幅在下方出现，点击即可。或者你可以手动使用“添加到主界面”的功能来实现。

``` >>> endl;```