---
title: PC 硬件史话——序章
tags:
  - Hardware History
  - Hardware
date: 2019-02-26 12:40:00
---


## 前言

笔者因为从小被父亲的爱好所影响，所以自然而然地喜欢上了 PC 硬件方面的东西。好多年前《微型计算机》杂志上不断有各种 PC 的元件发展史的文章，当时非常爱读这类历史传奇一般的说明文，现在有能力去直接读英文资料了，也就产生了自己写的念头。

思前想后几个月，我决定还是以比较方便我自己阅读的资料为主，尽量客观的以时间轴的方式来书写这段跨度长达近乎六十年的传奇历史。

参考的资料主要是以英文维基为主，并辅以各种其他资料来保证准确性和真实性。

序章里面还要讲讲这一整段传奇故事的大背景。

## 背景

### 半导体器件的发展

十九世纪的电学研究开启了第二次工业革命，把人类带入了电气时代。随着材料科技的迅猛进步，半导体的性质逐渐被人们所掌握，这直接导致了在二十世纪初，各种新型电子元件被发明，比如现在大家非常熟悉的二极管、电子管等等。在早期，这些半导体元件的个头都比较大，而随后出现的晶体管，则开启了一场真正意义上的革命。

晶体管分为两种类型，一种是场效应管（Field-effect Transistor, FET），其概念由 J. E. Lilienfeld 于 1926 年提出，但限于当时的条件，没有能够生产出实际能够工作的器件。而现在普遍认为的第一支晶体管是由贝尔实验室在 1947 年发明的，参与研发的人员有 John Bardeen, Walter Brattain 和 William Shockley。 他们发明的晶体管是与 J. E. Lilienfeld 提出的场效应管不同的双极性结型晶体管（Bipolar Junction Transistor, BJT），也就是我们现在俗称的三极管。

<img src="/images/PNP.png" />  <img src="/images/NPN.png" />

由于种种原因，在发明了晶体管之后，Shockley（后文译作肖克利）离开了贝尔实验室。1956 年，他在山景城创办了以他自己名字命名的肖克利半导体实验室（Shockley Semiconductor Laboratory），实验室所在的那片地区后来发展演变成为了举世闻名的硅谷（Silicon Valley）。肖克利实验室招揽了许多有志向研究半导体的年轻科学家与工程师，其中最为著名的便是日后的「八叛逆」: 罗伯特·诺伊斯（Robert Noyce）、高登·摩尔（Gordon Moore）、朱利亚斯·布兰克（Julius Blank）、尤金·克莱尔（Eugene Kleiner）、金·赫尔尼（Jean Hoerni）、杰·拉斯特（Jay Last）、谢尔顿·罗伯茨（Sheldon Roberts）和维克多·格里尼克（Victor Grinich）。

<img src="/images/The_Traitorous_Eight.jpg" />

#### 八叛逆与仙童半导体

可能是因为肖克利在实验室管理以及个人性格上存在的一些缺陷，八叛逆向肖克利的上级 Arnold Beckman 要求替换掉他在实验室的位置，然而 Arnold Beckman 最终做出的一系列支持肖克利的决定使得八叛逆不得不考虑离开肖克利实验室另寻出路。

当时半导体器件的主要基底材料是锗，而八叛逆认为硅比锗拥有更好的商业前景，因为相对于储量不高、提炼繁杂的锗，硅可以从沙子中提炼出来，可以有效降低原料成本和生产时间。罗伯特·诺伊斯用慷慨激昂的演讲向仙童摄影器材公司的老板 Sherman Fairchild 展示了他们的愿景，并成功说服了他。随后在 1957 年，他们获得了仙童摄影器材公司的资助，创办了仙童半导体（Fairchild Semiconductor）。仙童半导体在 1958 年成功的以硅为基底开发出了一款在商业上非常成功的晶体管——2N697。随后在 1959 年，八叛逆之一的金·赫尔尼又研发出了新的平面工艺，相对于传统的台面工艺，新的平面工艺无论是在成本还是产品的稳定性上，都有着巨大的进步，这项技术至今仍在半导体制造中扮演着极其重要的角色。

<img src="/images/Fairchild_ad_Electronics-1958-08-15.jpg" width="50%" />

继承了前人的理念之后，在 1958 年 9 月 12 日，德州仪器（Texas Instruments）的 Jack Kilby 成功的研发出了第一块能够工作的集成电路，随后他在 1959 年 2 月 6 日为这项发明申请了专利。半年之后，八叛逆之一的罗伯特·诺伊斯成功独立研发出了另一种不同的集成电路，与 Jack Kilby 不同的是，罗伯特·诺伊斯的集成电路是以硅为基底，并且更加实用。一年之后，诺伊斯又将平面工艺运用到集成电路的制造流程上，这也使得业界更为认可仙童半导体出产的集成电路。仙童半导体在创造了一系列对后世影响深远的研究发明之后俨然已经成为了整个半导体行业的领军者。

### 摩尔定律

1965 年 4 月 8 日，八叛逆之一的高登·摩尔在 Electronics 杂志上发表了名为 [*Cramming more components onto integrated circuits*](http://hasler.ece.gatech.edu/Published_papers/Technology_overview/gordon_moore_1965_article.pdf) 的文章，文中，他基于对行业发展的长久观察和思考后做出了一项具有历史意义的预测：

> The complexity for minimum component costs has increased at a rate of roughly a factor of two per year. Certainly over the short term this rate can be expected to continue, if not to increase. Over the longer term, the rate of increase is a bit more uncertain, although there is no reason to believe it will not remain nearly constant for at least 10 years. 

这就是半导体行业中著名的"Moore's Law"的前身，在 1975 年 IEEE（电气电子工程师学会）的一次会议上，摩尔修订了预测的增长率，原本两倍的增长率将在 1980 年之后减半。年内稍晚时候，加州理工学院的 Carver Mead 教授将摩尔的预测总结为"Moore's Law"。

<img src="/images/transistors-per-microprocessor.png" width="50%" />

摩尔定律事实上只是一条经验定律，但是却延续至今仍旧没有失效，甚至一定程度上在很长一段时间内指引了半导体业界的发展。

### 分崩离析与传奇的开始

让我们把目光重新对准仙童半导体。在六十年代的前五年，仙童半导体的风光一时无两，员工数量从最早的12人发展到了12000人。然而王权没有永恒，这家公司并不会这么一帆风顺下去。1965 年开始，仙童半导体在公司内部管理上开始出现了一些问题，到了 1967 年 7 月，公司已经开始亏损并且领头羊的位置被德州仪器所夺取。1968 年 8 月，罗伯特·诺伊斯、高登·摩尔和 Andrew Grove 一起从仙童半导体离职，创办了 NM Electronics，一年之后，公司更名为 Intel。1969 年，仙童半导体的一群工程师决定离开公司创业，他们找到了 Jerry Sanders 一起合伙，5 月 1 日，Advanced Micro Devices 公司成立。

<img src="/images/The-Roots-of-Silicon-Valley.jpg" width="50%" />

好了，演员已经全数登场，传奇即将上演。