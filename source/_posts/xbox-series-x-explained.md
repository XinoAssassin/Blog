---
title: 从硬件编辑的角度解读Xbox Series X的规格
tags:
  - Hardware
date: 2020-03-01 16:34:31
---



# 从硬件编辑的角度解读Xbox Series X的规格

在对外正式公开Xbox Series X这台主机74天之后，2月25日，Phil Spencer用一篇新的[博文](https://news.xbox.com/en-us/2020/02/24/what-you-can-expect-next-generation-gaming/)对外公布了这台次世代主机身上更多的细节，想必各位或多或少已经读过[新闻](https://www.gcores.com/articles/120661)了。不过普通玩家可能会对文章里面的很多名词感到陌生，什么Zen 2、RDNA 2、VRS……可能只看懂这台主机被吹上天了。本文就对这些名词简单进行一番介绍，顺便带出笔者作为一个硬件媒体编辑，对于这台主机的一些个人看法。让我们顺着Phil Spencer的文章脉络，一点一点来看。

# 硬件部分：Zen 2、RDNA 2、VRS、光追的简介

Phil Spencer首先介绍的是Xbox Series X使用的处理器——下一代定制处理器（Next Generation Custom Processor）。这枚处理器比今年CES展会上AMD发布的Ryzen 4000系列APU还要新，Phil Spencer其实早就已经剧透了这枚芯片的模样，在1月头上的时候，他把自己的Twitter头像给改成了这枚芯片的照片。

![](/images/dO7wSpcd_400x400.jpg)

Xbox Series X上面使用的这枚“下一代定制处理器”中集成了CPU和GPU，CPU部分是这半年来PC市场上表现得非常好的Zen 2处理器，而GPU部分则是基于尚未公布技术细节的RDNA 2架构。那么这块芯片究竟有多强呢？

## CPU：基于Zen 2，相对本世代主机有巨大提升

![](/images/Zen_2_Logo.jpg)

首先来看CPU部分，也就是Zen 2处理器。它是AMD近年来的最大翻身之作，在拥有相同核心数、相同频率的情况下它的综合性能已经可以比肩Intel最新一代的酷睿桌面版处理器（比如说，Ryzen 7 3700X的综合性能压了同为八核的i7-9700K一头，比i9-9900K稍弱），而它在玩家更看重的游戏性能方面也表现得相当优秀，基本上已经有对手90％的功力了。对比本世代主机上面那颗羸弱的Jaguar APU，那提升幅度只能用巨大来形容，原本CPU方面存在的严重短板将不复存在。

## GPU：基于RDNA 2，性能可能约在RTX 2080与RTX 2080 SUPER之间

然后来看对于游戏更为重要的GPU。这里Phil Spencer给出了一个具体的数字——12 TFLOPS，这个数字代表的是GPU的单精度浮点运算能力，经常性被用来当作GPU的主要性能指标。但我们并不能单单看这个指标就说新主机的GPU性能会是Xbox One X的两倍，因为它只是一个理论值，实际性能还得看架构，而新主机的GPU架构恰恰就有着巨大的改变。RDNA架构是AMD完全重新打造的显卡架构，在游戏图形效能上面较沿用约八年的GCN架构有很大的提升。比如说，理论单精度性能比RX 580显卡（基于GCN 4.0架构，就是PS4 Pro和Xbox One X上的GPU架构）低1 TFLOPS的RX 5500 XT（基于RDNA架构），其实际游戏性能反而要比前者强。（对于RDNA与GCN架构的实际表现差异，Digital Foundry做过一期视频，有兴趣的可以去看一下：《[Navi RDNA vs GCN 1.0: Last-Gen vs Next-Gen GPU Tech Head-To-Head!](https://www.youtube.com/watch?v=fzPo7gu-fTw)》）

<p style="text-align:center;"><img src="/images/PS5_XboxSeriesX_Specs-5.jpg" width="100%" />Digital Foundry推测中的Xbox Series X GPU规格</p>

比较凑巧的是，此前的泄漏显示Xbox Series X的GPU将会集成56组计算单元（CU），Digital Foundry对这枚GPU的单精度浮点性能推算结果正好是12 TFLOPS，所以现在我们可以假设这枚GPU拥有56组CU，那么对比拥有40组CU的Radeon RX 5700 XT（目前RDNA架构的旗舰显卡），它的计算单元规模大了40％，这将会带来显著的性能提升。

因此我们有理由认为Xbox Series X上面的显卡将会比Radeon RX 5700 XT强上不止一个档次。如果放在PC市场来看，RX 5700 XT跟NVIDIA GeForce RTX 2070 SUPER差上半档左右，也就是说，如果不算上RDNA 2架构可能带来的效率提升，Xbox Series X的GPU性能可能会处于RTX 2080到RTX 2080 SUPER之间，这是笔者的保守估计。

## RDNA 2带来的VRS和光线追踪支持

新的图形架构带来的不单单是性能方面的提升，在功能性上也终于追上了这个时代，加入了VRS（**V**ariable **R**ate **S**hading，可变速率着色）和光线追踪的支持。对于后者，我们见到了它在游戏中的实际表现，而VRS是什么东西呢？

### 可变速率着色

VRS的原理是通过改变单次像素着色器操作所处理的像素数量，来改变屏幕不同区域的着色质量。简单来说，它可以改变同个画面中不同部分的渲染精细度，**它的用处是提高画面帧数**。我们还是拿NVIDIA的那张示意图来举例：

<img src="/images/vrs_grids_car_002.png" width="100%" />

在不开启VRS的情况，也就是正常情况下，一帧画面的所有像素都是独立着色的；而开启VRS之后，原本独立的像素被分成了一个个像素块，它们会共享着色结果，此时GPU会根据程序员设定的重要性分级为所有像素块分配不同的着色精细度。拿上面的图片为例，车辆和远景部分的像素仍然是独立着色的，但快速变动的道路和路边的像素块就是区块共同着色的，此时由于显卡的计算资源得到了节约，所以游戏的帧数会有所提高。

目前3DMark已经引入了VRS相关的测试，在该软件的测试中，VRS分为两级，在Tier 1测试中，你会发现画面整体的精细程度都变差了，这是因为整个画面的着色速率都被降低了。

<p style="text-align:center;"><img src="/images/VRS-T1-Comp.jpg" width="100%" />左：VRS关；右：VRS开</p>

而在完整的Tier 2特性下，才会像上面所说的那样对整个画面进行分区，此时的效果就好了太多：

<p style="text-align:center;"><img src="/images/VRS-T2-Comp-2.jpg" width="100%" />上：VRS关；下：VRS开。现在区别就小了很多。</p>

这项技术最大的意义就是提高帧数，而且分辨率越高它的作用越明显。在3DMark的测试中，一张公版RTX 2070在1080p分辨率下的帧数提升为24.43％，而在4K分辨率下，提升幅度已经达到70.84％，在8K下面更是有116.62％的提升，直接从18帧幻灯片变成了40帧基本流畅的水平（数据来源：[超能网](https://www.expreview.com/72147.html)）。

这项功能是由NVIDIA最先在Turing GPU上引入的，Intel也在他们的Gen 11核显上面加入了这项特性，我们也终于将在RDNA 2架构中看到AMD方面的支持。所以，这项技术是Xbox Series X实现8K游戏的一大技术利器，如果运用得当，我们将会在不牺牲过多画面质量的情况下得到更为流畅的画面，或是享受到更高分辨率的画面。

## 硬件光线追踪支持

微软家嘛，肯定会用自己的DXR API作为光追的入口啦。官方也明确说了这是基于硬件加速（Hardware-accelerated）的光线追踪支持，所以很明确的一点就是RDNA 2架构中将会加入新的针对光线追踪的处理单元，一如当年Turing架构中的RT Core，用专用处理单元的形式为光追提供更好的支持。

<img src="/images/RDNA_2_RT.jpg" width="100%" />

这方面因为没有太多的信息，所以也没什么可说的。但有一个趋势是很明显的，那就是在主机端的推动下，未来支持光追的游戏将大幅变多。目前的PC市场上面也就只有NVIDIA在大力推光追，但实际收效并不明显，Turing推出一年多了也就只有寥寥十数款已发售游戏支持光追，其中很多游戏也只是浅尝辄止，没有完全应用光追的所有效果。这方面还是需要占据游戏业界主流地位的主机来推动。

# 功能性：快速恢复、DLI、HDMI 2.1、120fps、四代同堂以及智能分发

在硬件部分之后，Phil Spencer介绍了由新硬件带来的新功能和新特性，主要有SSD存储、快速恢复、延迟动态输入、HDMI 2.1、120 fps支持和四代兼容性、智能分发，对于这部分内容，我们分成一个个小点来看，首先是SSD以及在它支持下的快速恢复功能。

## 高速SSD带来的超快加载以及快速恢复

新主机将会用SSD这个事情实际上在去年就已经公布了，但我们不清楚的是新主机会用什么规格的SSD，是中端的“够用型”SSD呢还是高端的性能级SSD。现在Phil Spencer似乎暗示了Xbox Series X将会使用高端性能级SSD，因为他使用了“next-generation”来形容SSD。

对于目前的SSD来说，什么是下一代呢？要么存储密度上有提升，要么是接口进入了下一代。恰好，新的Zen 2 CPU带来了新的PCIe 4.0总线，简单的说，它的速度是上代（PCIe 3.0）的两倍。而今年正好会有大量采用PCIe 4.0接口的SSD会上市，像三星就已经在CES 2020上面展出了他们的消费级PCIe 4.0 SSD——980 PRO。因此，完全有理由认为Xbox Series X上面的SSD将会使用PCIe 4.0接口来最大化读写性能。

<p style="text-align:center;">
<img src="/images/samsung-980-pro.jpg" /><br/>三星980 PRO，支持PCIe 4.0，图片来自<a herf="https://www.anandtech.com/show/15352/ces-2020-samsung-980-pro-pcie-40-ssd-makes-an-appearance">AnandTech</a></p>

一个多月前，DigiTimes报道过[一则群联打入Xbox供应链的新闻](https://www.digitimes.com/news/a20200110PD204.html)：

>Phison has reportedly broken into the supply chain of Microsoft's Xbox, while Silicon Motion has seen orders for home consoles with built-in SSDs surge.

我们可能会在Xbox Series X上面见到来自群联（Phison）的SSD，而现在的分析偏向于认为Sony将会在SSD上面和三星进行合作。


讨论完了SSD的可能性，我们来看由它提供支持的快速恢复功能，笔者个人猜测这项功能基于虚拟机快照技术。

常用虚拟机的朋友应该不会对快照这个常见功能感到陌生，它会把虚拟机目前的内存数据和运行状态保存成文件，在下次开启虚拟机/读取快照时就从保存好的文件里面读数据，直接恢复到快照保存时的状态。是不是听上去和官方描述的“几乎瞬间（almost instantly）”、“多个游戏（multiple games）”、“从冻结状态（from a suspended state）”很像？

<img src="/images/xbox-os-arch.jpg" width="100%" />

可能有些读者就有疑问了，虚拟机里面怎么打游戏？那就不用担心微软的技术力了，Xbox One的系统——Xbox OS已经应用了微软自家的虚拟机技术，游戏和系统是分别跑在两个虚拟机中的。Xbox Series X的系统基本上可以确定会沿用目前的Xbox OS，所以这套成熟的虚拟机体系也将会被沿用。而此前因为HDD的读写速度不够而没法启用的快照功能自然就可以用了。

## 动态延迟输入（DLI）

在使用无线游戏手柄的时候，有一点是无法忽视的，那就是延迟问题。我们这些普通玩家可能没什么感觉，但在高手眼中，这点延迟就已经会造成手眼不同步。原本微软用来连接手柄的就是他们自己开发的一种专有无线协议了，这次他们新引入了动态延迟输入特性，在寥寥一句话的介绍中也智能推测出这是一个用于同步用户输入与显示输出的特性，既然名为动态，那么可能就是根据输入端的连接延迟在输出上面做相应的补偿来保证手眼同步。

## HDMI 2.1

HDMI是主机上面常见的用来输出画面的接口，其最新版本为HDMI 2.1，相对于HDMI 2.0，它不仅仅提高了最大支持的分辨率和帧率，更引入了多种新的特性，其中Phil Spencer重点提到了自动低延迟模式（**A**uto **L**ow **L**atency **M**ode）和可变帧率（**V**ariable **R**efresh **R**ate），其实这两种特性在目前已经得到了大范围运用了，只不过HDMI 2.1是将其标准化了。

### 8K@60fps、4K@120fps

HDMI 2.1一举将接口的最大传输速率提升了2.67倍，从18Gbit/s直接提升到48Gbit/s，并且加入了在DisplayPort上面已经成功运用的DSC压缩技术，从而将最大可传输分辨率提升到了10K的高度。不管分辨率是4K、8K还是10K，HDMI 2.1都可以提供最高120Hz的传输帧率，为Xbox Series X的120fps提供了坚实的保障。

![](/images/image-20200301142006596.png)

### 可变帧率（VRR）

说到可变帧率，PC玩家肯定是不陌生的，这项特性是用来让显示器的刷新率与显卡输出的画面帧数保持一致，减少画面撕裂现象出现的。目前PC上面有FreeSync和G-SYNC两种方案，前者是AMD主推，并且与DisplayPort接口中的Adaptive-Sync相兼容，目前相关的产品更多；而后者则是NVIDIA主推，但已经推出G-SYNC Compatible对FreeSync做了兼容。

早在2016年，FreeSync就已经可以跑在HDMI接口上了，而近来也有LG的一系列OLED电视机支持搭配N卡开启G-SYNC Compatible，用的也是HDMI接口，未来支持可变帧率技术的电视机将会越来越多，只要他们支持HDMI 2.1 VRR特性，Xbox Series X即可兼容并开启可变帧率模式，为玩家带来更加顺滑的画面，减少撕裂。

### 自动低延迟模式（ALLM）

实际上Xbox One就已经支持自动低延迟模式，听过AOC电视那期广告节目的应该会记得，电视机的延迟是远高于显示器的，但现在很多电视机厂家都在着重优化这一块，尽量降低内部延迟，为游戏玩家提供更好的体验。而ALLM特性就可以自动进行延迟优化。

![](/images/ALLM_600wide.jpg)

根据HDMI官方的说法，ALLM可以简化玩家的操作。在需要时，主机会向电视机发送一个开启低延迟模式的信号，而在不需要时也能够自动关闭掉这个模式以恢复电视内置的画面优化。

### 其他

实际上HDMI 2.1还引入了很多增强游戏体验的特性，像快速媒体切换和快速帧传输都是崭新的面向游戏应用的特性。前者用来减少在内容之间切换的空白屏幕时间，后者则可以让图像帧更早的到达电视机进行处理，减少Lag情况。

## 120 fps支持

主机端锁60帧是很常见的事情，但我们应该有点更高的追求。近年来高刷新率显示器的流行让很多PC玩家都认识到原来60帧真的束缚住了我们的视觉体验。其实在手机业界也是一样的，更高刷新率的屏幕可以明显带来更好的观感。本世代的主机可能限于机能问题只能锁60帧，但是对于硬件规格上有巨大飞跃的新主机而言，真的可以抛弃掉锁60帧这个传统了，于是Phil Spencer也写明了，在Xbox Series X上面，帧数上限被拉高到了120帧。再加上强悍的硬件能力支持，以及各路保障视觉体验的特性，在下个世代，玩家的视觉体验提升将不局限于分辨率和画面质量，更是在帧数，画面顺滑度和体感延迟上都会有很大的提高。

## 四代同堂和智能分发

四代同堂就不用多说了，微软在Xbox的兼容性上面一向是非常良心的，它为新入坑的玩家提供了一个用来回顾老游戏的非常方便的入口，只要有兴趣就可以用一台主机玩前后接近二十年的游戏，甚至不用重复购买。

重点来看智能分发。

有印象的读者可能还记得，在一月中旬的时候Xbox游戏工作室的主管Matt Booty在接受采访的时候确认Xbox Series X将没有独占游戏。给这点带来保障的就是新的智能分发技术，买一次即可在两代主机上游玩，并且有相应的优化版本，还得到了CDPR这个第三方的响应。本身Xbox第一方15个工作室的作品都会采用这种分发模式，再加上第三方的支持，在未来两年中老玩家在老平台上面玩到新游戏应该不是什么困难的事情。

能怎么说呢？Xbox牛逼就完事了。

# 总结：用户体验至上，核心即为游戏

总的来说，目前官方透露出来的Xbox Series X是一台硬件配置强悍，功能特性着重为玩家服务的次世代游戏主机。还记得本世代初期，Xbox One的惨败吗？很明显，Xbox团队吸取了经验教训，他们不再想做一个有游戏功能的客厅机顶盒，而是专注于游戏，为玩家带来最好的游戏体验。而在本世代中期开始转变的营销思路也得以延续，Xbox现在卖的是服务，Xbox Game Pass得到大家的追捧是因为它很实在，游戏又多又新，而微软也很清楚，订阅制服务实际上是更赚钱的（看看Office 365就知道了）。想要吸引更多的玩家买他们的服务首先要把硬件基础给搞好，于是Xbox Series X就诞生了。

