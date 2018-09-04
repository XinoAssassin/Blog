---
title: GPU 编解码器
tags:
- Hardware
- Everything
---

十多年前——约是 06，07 年之时——在 AVC 编码和 FHD 制式开始普及流行的时候，当时主流水平的 CPU 不能流畅的解码这种制式的视频，于是三大厂——当时还是四大厂刚并成三大厂的时候，纷纷推出了自家的硬件解码加速功能，ATI 的叫做 Avivo，Nvidia 的叫做 PureVideo，Intel 的叫做 Clear Video。虽然名字有差异，但是三家的思路是基本一致的——用 GPU 的处理能力来帮助解码。近几年来，因为 CPU 能力的加强以及系统对于硬件解码支持力度的加大，似乎这些技术被大家淡忘了，但是随着 HEVC、VP9 以及 4k UHD 甚至 8k 视频的出现，硬件解码技术又被重新摆上台面。以下就分三家来分别讲讲近几年的发展。

## AMD

A 家在 Radeon HD 2xx0 系列上将原本的 Avivo 技术加强为 Unified Video Decoder(UVD)。

在 Radeon R9 285 上 UVD 版本号刷到 5，增加了对 4k AVC 的解码支持。

在 Radeon Rx 300 系列上版本号刷到 6，增加了 4k HEVC 的解码支持。

在 Radeon Rx 400 系列上版本号刷到 6.3，增加了 10bit HEVC 以及 VP9 的解码支持。

然后在 Zen 架构的 APU 上，UVD 被 Video Core Next(VCN) 所取代，完整支持 VP9 的解码。