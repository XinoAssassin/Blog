---
title: Alder Lake SoC深度解析：Intel的大小核未来梦
tags:
  - Everything
  - Hardware
date: 2021-08-22 00:10:04
---


昨晚Intel举办了今年的架构日活动，和往年一样，Intel在架构日活动上会公布很多自家未来芯片设计方面的细节，比如说即将发布的CPU/GPU架构，新的内核设计和一些新的技术，今年的Intel架构日活动上他们带来了下一代处理器，也就是Alder Lake的详细内容。

## Alder Lake简介

Alder Lake是Intel耗时多年打造的一款全新架构，它是未来将要发布的第12代酷睿处理器的核心。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-068.jpg" width="100%"/> />

和以往的Intel处理器架构一样，Alder Lake包含了CPU、GPU、内存控制器、IO、显示输出和AI加速器等部件。它也是Intel首个采用大小核设计的高性能处理器，改动主要有以下几点：

- CPU部分采用大小核混合计算架构，最高由8大核8小核组成16核24线程
- CPU大核升级到Golden Cove架构，IPC提升约19%
- CPU小核升级到Gracemont架构，性能接近Skylake，能效比很高
- 采用Intel 7工艺制程，频率相较于10nm SuperFin工艺会有进一步提升
- 内存控制器升级支持DDR5和LPDDR5内存
- PCIe升级到5.0版本

让我们一点点来看Alder Lake的进化。

## x86大小核异构

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-053.jpg" width="100%"/>

Alder Lake上最大的变化点便是它采用大小核异构的架构，此前Intel曾在Lakefield上试水大小核异构，并推出了两款正式产品，不过它们都是低功耗处理器，性能不强。因此Alder Lake可以说是首款大小核异构设计的高性能x86处理器，在继承Lakefield的大小核异构设计之上，进行了深度改进。首先来看被Intel称为效率核心（E-Core）的Gracemont。

### E-Core：整体性能接近Skylake、但能耗更低的Gracemont

在大小核异构设计中，一般小核的设计目标是高能效比，而大核的目标则是提供极限高性能，Gracemont便是一个非常高效的核心。Intel的小核心设计是独立于大核心的另外一条线，现在一般称为Atom核心。一脉相传下来，Gracemont的上代是Tremont。从Tremont到Gracemont，Intel着重加强了小核心的后端执行能力，尤其是整数性能。

<img src="/images/adl-deep-dive/tremont-vs-gracemont.jpg" width="100%"/>

上图左边是Tremont，右边是Gracemont，可以非常明显的看到，Gracemont的执行端口多了不少，从原本的10个猛增至17个，而跟着的就是执行单元数量变多了。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-029.jpg" width="100%"/>

整数部分ALU从3个增加到4个，AGU从2个倍增到4个，对应还增加了一组MUL和DIV单元，整数执行能力得到大幅增强；浮点运算部分也有一定提升，原本只有一个的FADD和FMUL单元现在均有两个，能够拼合处理256-bit宽度的数据，也就是说能够满足执行AVX2指令集的需求；浮点ALU和STD均增加一个，计算能力会有较大提升。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-026.jpg" width="100%"/>

为了满足大幅膨胀的后端，前端也相应做了较大增强，解码部分仍然是两组三宽度设计，可以同时启用达成六解码。L1指令缓存（L1I）倍增至64KB，同时分支预测器得到加强，拥有更大的缓存。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-028.jpg" width="100%"/>

中核部分，ROB增大到256，这一数字比Skylake的224更大，与Zen 3持平。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-030.jpg" width="100%"/>

最后是缓存子系统，前面说过AGU从2个倍增到4个，分配成2个Load和2个Store。L1D的大小没有变化，仍为32KB，L2的缓存最高可达4MB，需要注意的是，L2缓存是4个小核一起共用的，同时容量可配置。对了，还需要提到的是，小核以4个为一组，一组小核的面积与一个Golden Cove差不多。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-033.jpg" width="100%"/>

总的这些改进加起来，Gracemont的性能提升相当可观。官方将它与Skylake进行了对比，在单线程的整数性能方面，Gracemont同功耗性能可提升超过40％，同性能下节约40％左右的功耗，能效比超群。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-034.jpg" width="100%"/>

而在多线程方面，同样是4线程，与开启超线程的2个Skylake内核相比，4个Gracemont内核能够在少用80％功耗的情况下输出同样的整数性能，而如果火力全开，那么能够提供约1.8倍的整数性能，同时功耗还更低。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-035.jpg" width="100%"/>

总的来说，Alder Lake使用Gracemont来提升处理器在多线程情景下的总性能，同时在注重节能的场景下，可以凭借小核优异的能效比实现更长的续航表现。

### P-Core：IPC提升约19％的Golden Cove

小核很强，而大核——Intel称为性能核心（Performance Core，简称P-Core）——的Golden Cove内核只能说是改的更大提升更多。用Intel的官方口径来说，就是变得更宽、更深和更智能了。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-050.jpg" width="100%"/>

更宽指的是内核解码、执行指令的并行程度更大；更深指的是内核中的各种指令缓存变得更大；更智能指的是部分组件具有更准确的判断能力。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-040.jpg" width="100%"/>

Golden Cove的前端部分改动相当大，最明显的就是多年未变的4宽度（实际上是4+1宽度）解码器升级成了6宽度解码器（应该是6+1）。不像Arm等RISC体系的内核，属于CISC体系的x86要增加指令解码器的代价相当大，因此不管是AMD还是Intel都把前端解码器维持在4宽度，现在Intel首先前行一步。同时L1I缓存的带宽也扩大一倍到32Bytes以满足6宽度解码器的需要。

增加解码器宽度会增加处理器的流水线长度，这让分支预测错误的惩罚更重。Intel选择增加分支预测缓冲区（BTB）来应对这一问题，其分支条目数量从5K直接增加到12K，比Zen 3的6.5K多将近一倍。分支预测器本身也变得“Smarter”了，准确率继续提升。

宏指令（µOP）的吞吐量从多年未变的每周期6个增加到8个，同时用于缓存宏指令的宏指令缓存（µOP Cache）继续增大，从2.25K直接增大到4K，与Zen 2/Zen 3持平。宏指令队列的结构有所调整，现在为超线程进行了更多优化，双线程同时利用的情况下，单线程队列深度为72；而如果是单线程利用核心的情况则可以完整利用144的队列深度。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-041.jpg" width="100%"/>

中核部分，同步变得更宽，发射区从原本的5宽度加宽到6宽度，ROB缓存从Sunny Cove的384加大到512，直逼苹果Firestorm内核的600+，ROB增大会显著增加内核功耗。另外，执行端口方面增加两个，现在共有12个端口，不过整数和浮点仍然共用发射端口，没有改成流行的分离式。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-042.jpg" width="100%"/>

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-043.jpg" width="100%"/>

虽然是共用端口，不过Intel还是把整数和浮点的改进分开讲了。后端执行部分的改动相对较小，从上面两张图中可以看到，整数部分增加了一个ALU；FPU部分增加了两个FADD单元，它比FMA单元更高效，指令周期也更短了；而FMA单元增加了对FP16数据的支持，对低精度计算有帮助，不过因为需要调用AVX-512指令集，所以在Alder Lake上我们无法利用到它。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-044.jpg" width="100%"/>

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-045.jpg" width="100%"/>

另一个新增的端口被用于缓存子系统，新增了一个Load AGU，这样每周期的Load带宽提升至3，和Zen 3持平。L2沿用Willow Cove的设计，仍然是非包含式设计，每核心具有1.25MB。不过加入了新的预取机制，降低了DRAM的读取次数。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-046.jpg" width="100%"/>

总的改进加起来让Golden Cove相比起Cypress Core有了平均约19％的同频性能提升，最高甚至能有60％左右的提升。不过比较奇怪的是，有几个项目的成绩出现了倒退。总的来说，Golden Cove是一次全面的大改，可能是自Skylake以来改动最大的一个内核微架构。

### Intel Thread Director：调度大小核的关键角色

大小核心的性能提升都非常可观，但要如何调度它们，让它们充分发挥自己的长处呢？其实Arm已经替x86淌过浑水了，big.LITTLE架构发展至今已有十余年时间，主流的操作系统都添加了对大小核的调度支持，包括Windows。操作系统是知道处理器上多个性能不同的内核的。但之前在Lakefield上我们也看到了Windows在调度大小核x86处理器时候的糟糕表现了，该怎么解决这个要命的问题呢？Intel选择了一个软硬件结合的方案，称为线程总监（Thread Director，暂译，等官方中文名）。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-065.jpg" width="100%"/>

在操作系统层面上，Intel和微软合作改进了Windows的任务调度，从Windows 11开始，系统的任务调度器能够获取更多信息，用于判断当前正在运行的线程需要什么样的性能模式，它要调用哪些指令集，同时它还懂得让硬件为高优先级任务让位。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-056.jpg" width="100%"/>

同时，Intel在Alder Lake处理器中集成了一个非常小的MCU，用来监控当前处理器内核的运行情况，能够监测到每个线程的特征，比如它运行什么样的指令集、它的性能需求如何等等。在收集完信息之后，它会将收集到的信息反馈给Windows 11，而后者将会把这些信息与自己收集到的信息相结合，判断是否应该将线程转移到别的核心上。这一切发生在短短30微秒以内，而传统的调度器可能需要100多毫秒才能判断出结论。

当然，Alder Lake默认还是会把线程安排在P-Core上，除非高性能核心上面都有任务在跑。Intel将Alder Lake分为以下三个性能层级：

1. 每个P-Core上只跑1个线程
2. E-Core上只跑1个线程（当然它也只能跑1个）
3. 在P-Core的超线程上跑线程、

也就是说，在一般情况下，系统调度器会优先把线程安排到P-Core原生的线程上，8个原生P-Core线程被放完后，轮到的是E-Core，如果还不够用，它才会去利用P-Core超线程出来的线程（因为超线程出来的线程性能肯定是不如E-Core的好嘛）。比如一个20线程的任务，会利用上P-Core原生的8个线程+E-Core原生的8个线程外加4个P-Core超线程出来的4个线程。

当然，Windows 10也还是有大小核调度的能力的，但是说简单点就是不够智能。在Windows 11下Alder Lake应该会有更好的能效表现。

## 支持DDR5与LPDDR5内存，仍然兼容DDR4和LPDDR4

讲完内核部分，我们略过没有实质性变化的Xe GPU，直接来看其他的一些变化点，首先是内存控制器：

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-076.jpg" width="100%"/>

可以看到Alder Lake新增了对DDR5和LPDDR5内存的支持。默认情况下DDR5支持到4800MT/s，LPDDR5支持到5200MT/s，前者在今年晚些时候会开始出货，而后者在移动设备上已经被广泛应用，本来Tiger Lake是号称支持LPDDR5的，后来因为种种原因没能最终实现。而在Alder Lake正式推出之后，应该会有很多轻薄本用上LPDDR5内存。

## 支持PCIe 5.0的新IO

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-077.jpg" width="100%"/>

Alder Lake的PCIe支持非常激进，直接一步升级到最新的PCIe 5.0，带宽较PCIe 4再翻一番，x16下数据带宽高达64GB/s。当然因为功耗原因，这应该是桌面平台独有的。在Rocket Lake-S和Tiger Lake上新增的x4通道则仍然是PCIe 4.0规格的，可以用于连接SSD。虽然没有明说，但与PCH互联的总线应该是升级到DMI 4.0了，至少会是x4的宽度，而高端PCH应该会通过DMI 4.0 x8与CPU相连。PCH能够再导出12条PCIe 4.0和16条PCIe 3.0，扩展性比起以前来可谓是一个天上一个地下。

## 大一统的Alder Lake

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-074-1629402245043.jpg" width="100%"/>

相比起11代酷睿在桌面和移动端的分裂，Alder Lake又重新统一了回来，当然不同平台还是会有不同的规格。

<img src="/images/adl-deep-dive/intel-architecture-day-2021-presentation-075.jpg" width="100%"/>

桌面端的Alder Lake最高会有8大核8小核，不过没有集成的Thunderbolt 4控制器，核显规格也仍然只有32EU。移动端最高则是会有6大核8小核，外加96EU和4个Thunderbolt控制器，当然还是会集成祖传的IPU。对功耗更为敏感的超轻薄端最高就只有2大核8小核了，Thunderbolt控制器数量也减少到2个。

Alder Lake是近些年来Intel推出的改变最大的一个架构，不管是计算内核本身的改动还是大小核的设计，可以说是非常激进的。很惊喜Intel能给我们带来这样一个很有创造性的新架构，Intel可能会在10月末的innovatiON活动上正式发布Alder Lake的产品，也就是第12代酷睿处理器，非常期待它的正式表现。
