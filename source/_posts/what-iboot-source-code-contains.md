---
title: 泄露的 iBoot 源代码中都有些什么
tags:
  - Programming
  - 杂
date: 2018-02-09 17:05:33
---


近日，一份疑似 iOS 9.3.x 的 iBoot 及其他部分 BootLoader 的源代码被匿名人士 push 上了 Github, 粗略地查看了一下里面的文件之后，大家都惊讶地发现这份源代码的爆点实在是太多了。

项目的 README 中是这么写的：iBoot_BootROM_iBSS_iBSS_iLLB_Source_Code

其中 iBSS 是 **iB**oot **S**ingle **S**tage 的缩写，而 LLB 是 **L**ow **L**evel **B**ootloader 的缩写。

我们先来看一下 iOS Devices 的启动流程：

在 Apple 定制的一系列 A 系列 SoC 中有一块存储区域专门存放了一部分最初的启动代码，也就是俗称的 BootROM, 这部分被 Apple 称之为 SecureROM, 它是在电源键被按下，设备上电之后最初加载的代码。SecureROM 检查下一级 BootLoader, 也就是 LLB, 如果签名没有问题，那么就加载并初始化 LLB, 此时设备的 Apple Logo 就出现了。然后 LLB 检查并初始化 iBoot, 同样是通过 Apple Root CA Public 证书验证签名，通过就会把设备带给 iBoot, 而 iBoot 会映射 Device Tree, 然后验证内核签名并加载，最终初始化内核并运行，这时候可以说 iOS 系统已经启动了。

通过简述的启动流程，我们可以看到，BootROM, LLB, iBoot 这三个无论哪一个都是在整个启动流程中充当关键角色的。所以这次源代码的泄露，非常劲爆。

在基于证书链的安全体系下，私钥的泄露可以说是直接把安全性掉到0去了，而这份代码中，在位于 /lib/pki 和其中的 tests 目录下，可以看到有一份 Fake CA 和一些私钥，还有一份中间证书和私钥，如果后续版本也是沿用这些东西的话，整个启动验证链的加密体系，就真的可以说安全性为0了。

因为这份代码是 9.3 时代的，也就是在 iPhone 7 那代发布前的版本，所以我们能看到有许多 T8010 相关的文件（T8010 是 Apple 对于 A10 芯片平台的代号）。而有趣的是，在 /target 目录下出现了 iphone8 这个文件夹，难不成 Apple 内部在 2016 年就开始对 iPhone 8 进行内部测试了？但是点进去看，目录下的文件清晰的指向了另一款产品——iPhone 6s. 往上推，iPhone 7 文件夹对应着 iPhone 6, iPhone 6 文件夹对应着 iPhone 5s, 如果按照这个内部排序，iPhone Ⅹ 这个 Ⅹ的由来，可能更好解释一点。而 iPhone 8, 则更像后来上马的 iPhone 7 Reversion.

除了主引导程序的代码外，泄露的代码中还包含了许多驱动，在 /drivers 目录下，可以清晰地认出 iOS 平台所使用的一些硬件，其中甚至包括了 Thunderbolt 驱动。

内容还有很多，笔者水平有限，也解读不出更多的东西了。

`>> endl;`