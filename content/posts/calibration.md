---
title: "显示器校色笔记"
date: 2023-02-15T11:47:45+08:00
draft: false
tags: ["Calibration", "DisplayCAL"]
---

## 2022-12-30 阅读 DisplayCAL 官网 User Guide

在对显示器校色之前，必须预热至少 30 分钟。这一点在我最近的校准过程中可以明确体会到，比如：在调整显示器自身的 RGB 输出时，当你把红绿蓝三个通道终于调到一条线上之后，这时亮度不达标了。调节显示器的亮度以达到目标值，比如 100 nits 之后，RGB 那三个值又不在一条线上了。一方面跟显示器的素质有关，不能保证在亮度增减的同时保持 RGB 三个值的输出一致。另一方面也跟显示器预热有关，没到 30 分钟状态还不稳定。

显示器校准的基本概念：Calibration 和 Profiling/Characterization。在使用 DisplayCAL 进行校准的时候，第一步就是通过显示器自身的调节按钮，比如显示器的 RGB 三通道值的输出，来与事先定义好的目标值匹配。调节显示器的亮度按钮使其达到比如 100 尼特；在色温选项里使用用户自定义的方式来调节 RGB 输出，使其达到比如 6500K 的色温（CCT）。这一步我们粗略的称之为校准 Calibration，需要人手动去调的。而第二步是软件自动实现的过程，叫做 Profiling，就是它会在屏幕上生成一系列色块，测量这些色块的值，与标准值进行匹配，然后生成一个 Profile (e.g. ICC profile) 给我们最终加载到系统上。[Profiling 实际上并不改变颜色](https://www.argyllcms.com/doc/calvschar.html)，它描述的是该显示器对各种颜色的响应，通常存储在一个 ICC 配置文件中。

之前在用 i1 Display Pro 这个色度计搭配 DisplayCAL 软件来校色的时候，校出来的结果你很难相信它是准确的。很多时候都是偏红偏黄。这当然是由很多因素导致的，每个显示器的素质不同，甚至同一型号的两台显示器也会是不同的结果。显示器面板老化等等。但有一点是之前比较忽略的，就是这个色度计这个探头本身是不是准的。

色度计（Colorimeter）也需要[在硬件或软件上进行校准](https://displaycal.net/#colorimeter-corrections)，以便从不同类型的显示器上获得正确的测量结果。色度计这个东西，一般是针对某一类或者某一个的显示设备，然后就专门用于那一类或那一个显示器的校准了，一般不用于其他显示器校准。因为当初是只针对那一个显示器做了色度计自身的硬件校准。如果色度计在最初，比如出厂前，没有针对某一类显示器做硬件校准。那么就需要使用光谱仪（Spectrometer）对那个显示器进行测量，计算出一个结果作为一个校准文件给搭配色度计使用的软件加载，这就是对色度计进行所谓的软件的校准。光谱仪（Spectrometer）据我所知非常昂贵，也不知道什么地方能搞到。但 DisplayCAL 官网有一个[数据库](https://colorimetercorrections.displaycal.net/)，里面就有别人使用光谱仪针对 i1 Display Pro 和各种显示器、监视器的校准文件。我们直接拿到那个校准文件也能用。但毕竟是别人在他们自己的那个设备上测量的结果，跟我们手上的显示器可能还是有一定偏差（虽然是同一型号的显示器）。我们可以在 DisplayCAL 软件里很方便的加载这个校准文件。

那么即便有了这个对与色度计自身进行校准的文件，我们使用 6500K 色温作为目标值校准出来的结果，你放一个纯白出来，比如开一个文件资源管理器，也很难说这就是一个纯白。当然白色这个概念，人眼对于白的感知就挺主观的，而且是可以适应它的。但是在正常办公室光照条件下，那个屏幕摆在那你一眼看过去就是感觉它偏黄，这能怎么办呢。使用 i1 Display Pro 对 MacBook Pro 的屏幕进行测量会发现，它的色温是 7000K 左右。我们都知道 Rec.709 也好 sRGB 也好，白点都是 D65，也就是 6500K。为什么苹果 MBP 的屏幕是 7000K 的色温呢。我在我自己的 MBP 14 吋上，使用不同的显示预设（BT.709-BT.1886、sRGB、XDR-1600nits）测量出来的色温也都是 7000K 上下。我不知道是不是因为其实大多数用户，更偏好一个理论上更偏冷的屏幕（7000K），而不是一个标准的但肉眼看上去比较暖、比较黄的屏幕（6500K）。

总之使用 7000K 的色温作为目标值，加载针对不同显示器的 i1 Display Pro 的校准文件，校准出来的结果是比较符合预期的。另外两个 Dell 的主副屏显示器，校准之前，两个屏幕有很明显的色差。使用 DisplayCAL 校准的时候，以 7000K 色温为目标值，先在显示器的 RGB 设置调节上，把 RGB 输出调到一条线上，然后点继续。等自动化的 Profiling 过程结束后，我们对比校准前和校准后，可以得到一个不错的匹配。副屏在校准之前，暗部区域比主屏要偏蓝偏青，然后感觉整个屏幕的饱和度都要比主屏高。校准之后有所改善。

## 2023-02-10 BenQ SW 系列的硬件校色功能

BenQ 的 SW 系列相比于 PD 系列具有硬件校色功能，使用 BenQ 的软件 [Palette Master Element](https://www.benq.com/en-us/monitor/software/palette-master-element.html) 搭配 X-rite 的 i1 Display Pro 色度计可以让校色结果直接储存和加载到显示器本身，使用显示器自身芯片进行色彩校正。而不是像软件校色那样，校色结果储存在电脑，作用在显卡，然后将信号给到显示器。据明基 BenQ 所说，这种硬件校色的实现相比软件校色[更优](https://www.benq.com/en-us/knowledge-center/knowledge/hardware-vs-software-calibration.html)。一款显示器的好坏与否，除了它自身的素质之外，有方便的机制可以让我们对其进行定期的校色，也是很重要的衡量因素。因此 BenQ SW270C 和 SW271C 具有的硬件校色功能是一个加分项。

安装 Palette Master Element 软件时，会要求用 USB Type-B 线把显示器和电脑连接，然后会自动安装 BenQ 的驱动。安装好之后的使用和校准都需要保持显示器和电脑的 USB 连接。校准的设置并不多，总的分为两块：校准和验证。校准菜单下可设置的选项也不多，白点（D65），RGB 三原色（我们选 sRGB），亮度（我们设置为 120 尼特），伽马（2.2），黑点（绝对零位）。然后点击下一步。BenQ SW270C 提供三个校准的槽位，我们可以选择将校准 LUT 加载在可选的「校准 1」里，然后就可以开始测量了。把 i1 Display Pro 的探头贴在显示器的正中央，让软件运行，进行校准。校准 LUT 会自动加载。最后是验证校准的结果，也是跑一遍色块，最后得到校准的结果。Delta E。GSJ-9 我这次的校准结果的平均 Delta E 是 0.45，最带 Delta E 是 0.76。光看这个结果，数字上是非常不错的。理论上 Delta E 小于 2，人眼就很难看出区别了。

## Reference

1. [Calibration vs. Characterization](https://www.argyllcms.com/doc/calvschar.html) - ArgyllCMS
2. [A note about colorimeters, displays and DisplayCAL](https://displaycal.net/#colorimeter-corrections) - DisplayCAL
3. [Colorimeter Corrections Database](https://colorimetercorrections.displaycal.net/) - DisplayCAL
4. BenQ [Palette Master Element](https://www.benq.com/en-us/monitor/software/palette-master-element.html) Software - BenQ
5. [Hardware vs. Software Calibration [Updated 2021]](https://www.benq.com/en-us/knowledge-center/knowledge/hardware-vs-software-calibration.html) - BenQ