---
title: GeForce RTX 前瞻——在图灵架构初来乍到之时
tags:
  - Hardware
date: 2018-08-14 23:16:25
---


## 终于确认的 Turing

距离 Nvidia 发布目前主流消费市场上的显卡—— GeForce 10 系列已经过去两年又三个月了，或许是因为竞争对手实在不给力，其推出的 Vega 系列没有撼动 Pascal 系显卡统治地位的能力，又或许是新卡的配套工程出了一些问题，比如制程问题还有 GDDR 6 显存的产量问题。

<p style="text-align:center;"><img src="/images/Roadmap-2015.jpg" width=100% />2015 年 Roadmap</p>

这是一张 2015 年初，Nvidia 发布会上的路线图，上面可以很清楚地看到，Pascal 架构之后是 Volta 架构，横坐标对应着 2018。而 Volta 架构的亮相则是在去年年底，2017 年 12 月 7 日，老黄发布了 Titan V，一张价值 3000 美金的发烧级显卡。就在人们翘首期盼新的基于 Volta 架构的显卡之时，Nvidia 却迟迟没有发布新一代的 GeForce 系列，2018 年整个上半年，无数爱好者被时不时传出的一些 Nvidia 新卡的消息吊足了胃口，连新卡用的架构都有三个说法：Volta、Turing 还是 Ampere？直到 2018 年 8 月 13 日，Volta 之后的新架构 Turing 终于随着新一代 Quadro 专业级显卡正式登场了，而也证实了传闻中的 Turing 架构的真实性，以及爱好者们期待许久的新一代 GeForce 显卡就快要登场了，下面就是综合最近的一些消息对 GeForce 新系列的一些前瞻。

<p style="text-align:center;"><img src="/images/old-huang-with-QuadroRTX.jpg" width=100% alt="" />老黄举着新的 Quadro RTX 显卡</p>

## RTX

我们先从命名说起。

新一代的 Quadro 专业级显卡的命名方式均为 Quadro RTX x000，而早前 Nvidia 注册了关于 RTX 的一些商标，这可能预示着新系列的 GeForce 显卡命名可能在顶级显卡上会放弃使用了十年之久的 GTX 前缀（后缀），而换用 RTX 作为新的前缀。

那么 RTX 又代表了什么？

**R**ay **T**racing（光线追踪）然后带个 X。

熟悉游戏图形的朋友可能略微知道，目前桌面级游戏几乎都是基于光栅化的渲染方式，而大家经常看到的一些电影、游戏 CG 在渲染层面则是采用了实时光线追踪的方式。在桌面级平台上实现实时光线追踪这一目标是业界一直努力的一个方向，年初微软在 Windows 10 的更新中也为 DirectX 加入了新的光线追踪 API。而现在 Nvidia 终于将这一技术带到了桌面级平台上面，用业界专家的话来说：

> This is a significant moment in the history of computer graphics, Nvidia is delivering real-time ray tracing five years before we had thought possible.  --Jon Peddie, the well-respected CEO of analyst firm JPR and a noted graphics expert 

<p style="text-align:center;"><img src="/images/an-example-of-Ray-Tracing.jpg" width=100% />实时光线追踪样图</p>

Nvidia 宣称其在新架构上新加入了一种 RT 单元专用于实时光线追踪，新架构在模拟真实世界场景上能 **6 倍**于前代，在实时光线追踪上则能做到 **25 倍**于帕斯卡架构。

所以 Nvidia 将这一特性作为新一代显卡的前缀来增强宣传作用是完全合理的。

这是两段近日发布的 Nvidia 的实时光线追踪的 Demo：

- https://www.youtube.com/watch?v=bFUWu387ErM
- https://www.youtube.com/watch?v=3jb3flTRykQ

其中效果我只能用恁牛逼啊来叙述。

## GDDR6 显存

不像 CPU，GPU 对于高速缓存的依赖非常之高，自 2008 年，HD 48x0 这代开始使用 GDDR5 显存到现在已经过去了十年，业界不断在为 GDDR5 续命，不过近些年来，AMD 在其高端卡上已经使用 HBM 取代了 GDDR5，而 Nvidia 也在 GTX 1080 上首次使用了 GDDR5X ，在 Titan V 上使用了 HBM 2 显存来获取更高的带宽。其继任者 GDDR6 标准在 2017 年 7 月被正式通过了，随后在今年年内，三星、镁光以及海力士这三家显存颗粒主要供应商都宣布开始出货 GDDR6 显存。

而 13 日 Quadro RTX 系列的发布证实了“Turing 架构将会使用 GDDR6 显存”的传闻，大概至少 GeForce 的旗舰级新卡会用上它吧。

## 核心及其他改进

从现有的 Quadro 新卡来看，完整的 Turing 核心似乎是一个超大的核心，相比于 Pascal，其核心面积大了约 60%，晶体管数量多了约 58%，如果数字太过抽象，那么下面这张图就能很好地帮助你理解 Turing 到底大了多少。然而，即使较 Pascal 而言扩张了相当大的面积，Turing 仍然差 Volta 那么一筹（晶体管数量 21B，核心面积 815mm<sub>2</sub>）。

<p style="text-align:center;"><img src="/images/pascal-vs-turing.jpg" width=100% alt="" />Pascal VS Turing</p>

<p style="text-align:center;"><img src="/images/turing-architecture.png" width=100% alt="" />Turing 架构图</p>

这是官方发布会公布的 Turing 架构图，罗列一下现在已知的改进点：

- NVLINK 带宽增加了
- Tensor 单元（Volta 架构引入的用于人工智能以及机器学习的单元）改进了
- 核心缓存改进了
- 视频引擎改进
- 输出支持 VirtualLink 以及 USB-C，支持新版 DisplayPort

## 11x0 or 20x0

近日，AIDA64 的新版本中包括了一些 Nvidia 的新 GPU，其中 GV 104 就被命名成了 GeForce GTX 1180。

<p style="text-align:center;"><img src="/images/AIDA64-1180.jpg" width=100% alt="" />GeForce GTX 1180</p>

然而现在 Turing 架构的正式发布似乎标志着 AIDA64 出错了。首先，按照惯例，如果新一代 GeForce 显卡使用 Turing 架构，那么其核心代号则应该是 GT xxx，考虑到已经出现过 GT 系列核心代号（GeForce 100/200/300 系列），那么可能会使用别的代号也说不定，但是 GV 开头的可能性已经很小了。其次，按照上文 RTX 一节所说，新卡应该会以 RTX 为前缀而非 GTX。

就在 Quadro 新卡发布之后的 14 日上午，Nvidia GeForce 的官方推特发布了一个新的疑似预告片的[短视频](https://twitter.com/NVIDIAGeForce/status/1029164596903337984)，以 "#BeForTheGame" 命名，视频疑似预告了在 8 月 20 日科隆游戏展上 Nvidia 将会发布新一代桌面级显卡。在视频的第 45 秒，一个一闪而过的聊天信息中发现了有趣的东西：

<img src="/images/video-45s.png" alt="45 秒" />

内容是一个 RayTeX 的玩家和另一个 ID 为 Not_11 的玩家的对话。

> RayTeX: new build?
>
> Not_11: No haha I wish, some got this at work
>
> RayTeX: you getting on??
>
> Not_11: eating, gimme 20

两个非常有趣的 ID，一个缩写为 RTX 另一个叫不是 11，可能是官方剧透。最后一句 eating, gimme 20 与其 ID 相呼应，貌似下一代命名为 GeForce RTX 20x0 已经是板上钉钉的事情了。

但是新卡仍然没有发布，谁知道呢。