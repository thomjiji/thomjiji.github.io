---
title: "ICC Profile"
date: 2023-03-09T14:59:50+08:00
draft: false
---

对普通显示器校色的时候，很难不碰到 [ICC profile](https://www.google.com/search?q=ICC%20profile) 这个东西。我们都知道这个名词，显示器的 ICC 文件，或许模糊的对它有一些概念。但它究竟是什么？原理如何。

ICC profile 跟我们已经有很明确概念的 ACES，或者更广泛一些地讲：「现代的色彩管理流程」，是很相似的 idea。都有一个处于整个链路中间的非常大的色彩空间，大到足以容纳所有不同的输入、显示和输出设备的各种色域（sRGB、AdobeRGB、Rec.709 等等）[^1]。这个色彩空间在 ICC 这个概念下叫作 PCS（Profile Connection Space）。ICC profile 通过定义设备源或目标色彩空间与 PCS 之间的**映射**来描述特定设备的色彩属性[^2]。

ICC 本身并不进行任何校准，它们只包含显示器关于色彩成像能力的数据。这些数据用于帮助可以识别 ICC profile 的软件（我们叫它 ICC aware 的软件）通过 CMM（色彩管理模块）对图像进行调整，以尝试纠正显示器的色偏来进行校准。ICC profile 完成的实际上是对**图像的调整**，而不是对显示器的校正。不同的软件使用不同的 CMM 的话，即使都采用同一个 ICC，最终出来的结果也可能不一样。

# Reference

[^1]: [ICC Profile](https://www.ibm.com/docs/en/i/7.4?topic=management-icc-profiles) - IBM
[^2]: [ICC profile](https://en.wikipedia.org/wiki/ICC_profile) - Wikipedia
