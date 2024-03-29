---
title: 视频压制指北——2021版
tags:
- Tutorial
---

视频压制是一门技术活。

可能很多朋友都在日常中曾经接触过压制，用过像 MediaCoder 啊，小丸工具箱啊这类的工具软件，后者还好，前者就是个基于 FFmpeg 的图形化前端，懒得多说。如果想要追求稍微好一点的压制效果，那还是自己去钻研一下为妙，一站式工具软件显然满足不了我们的需求，那么我们就从较为完整的压制工具链入手，学习如何压制。

## 预处理和编码

现在的压制过程基本上分成两个部分，第一部分是预处理，这个部分首先将片源解码成无压缩的YUV视频流，然后对画面进行处理，常见的有反交错、去色带、加字幕、降噪、缩放等等等等，运用得当可以修复原始片源画面的一些瑕疵，甚至还能够优化最终编码出来的文件大小。

第二部分编码便是将预处理器输出的YUV视频流编码成另一种格式的过程，目前最终送入编码器的一般都是YUV4MPEG流，简称Y4M。和原始的YUV流相比，它的头部带有分辨率、颜色空间等元信息，编码器能够获取到这部分元信息并且可以自动选择正确的压制参数。压制者可以通过编码器参数来对文件大小和成品质量进行调整，两者是一个 tradeoff 关系。

在预处理和编码过后可能还有一些后期工作比如封装加章节信息改名之类的，不过这些都已经是小事了。那么我们学习压制首先还是来看一下预处理怎么做。

## 用 VapourSynth 吧不要再用 AviSynth 了

常用的视频预处理器在今天主要有 VapourSynth 和 AviSynth 两种，前者天生支持多线程和大内存，更为现代；后者逐渐在被前者取代，到今天基本上已经没有新人在用 AviSynth 了，因此本文只讲 VapourSynth。

VapourSynth 

## 用x265吧不要再用x264了

大多数人接触、用上AVC编码可能也就十年左右的时间，实际上离这套视频编码规范首次发布正式版已经过去17年了，离给AVC加入High Profile的第三版标准发布也有15年时间了，对于一个视频编码来说，它已经足够老了，具体到实际应用中，AVC在高于FHD分辨率下面的压缩比已经不够用了；另外业界基本上都没有对High 10这个Profile做相关的适配，导致标准化15年后基本没有设备能够对High 10进行硬件解码。也就是说，AVC在应用高于8-bit色深时，是欠缺相关支持的，因此也基本上只有二次元压制组在用Hi10p这种Profile，一般情况下我们享受不到高色深带来的好处。



## 就算你要用x264也请换新版

## 多学多练

