---
title: "Demystifying HLG"
date: 2025-08-07
tags: ["colormanagement", "colorscience", "hlg"]
draft: true
ShowToc: true
---

## ITU-R BT.2390-7 (07-2019) {#itu-r-bt-dot-2390-7--07-2019}


### 10 Mapping of SDR content into HLG and PQ {#10-mapping-of-sdr-content-into-hlg-and-pq}

标准动态范围（SDR）内容可以通过直接映射或反向色调映射（也叫“向上转换”）的方式，被整合进 HDR 格式的节目当中。

一种映射方式是将 SDR 内容放进 HDR 容器，这类似于将 BT.709 色度编码的内容装进 BT.2020 容器。这种做法的目的是：在 HDR 显示设备上尽可能还原 SDR 内容原本的“观感”。

相较之下，“反向色调映射”（inverse tone mapping，或称“向上转换”）的目标是扩大亮度范围，充分利用 HDR 显示设备的亮度能力，从而模拟出 HDR 内容的视觉效果。本节所讲的就是这种映射方式的技术方法。

根据具体应用场景，SDR 的映射方式可分为两种：

-   显示参考映射（Display-referred mapping）：用于在 HDR（BT.2100）显示设备上，保留 SDR（BT.709 或 BT.2020）显示设备上的色彩和相对亮度关系。典型例子是将调色完成的 SDR 片段嵌入 HDR 节目中。

-   场景参考映射（Scene-referred mapping）：适用于素材直接来自 SDR 摄像机的场景，目标是让画面尽量贴近 HDR（BT.2100）摄像机的色彩和亮度表现。这通常出现在直播制作中 SDR 和 HDR 摄像机混用的场合。


## ITU-R BT.2390-12 (12-2025) {#itu-r-bt-dot-2390-12--12-2025}


### 2 Television system architechture {#2-television-system-architechture}


#### 2.1 The relationship between the OETF, the EOTF and the OOTF {#2-dot-1-the-relationship-between-the-oetf-the-eotf-and-the-ootf}

-   **OETF**: the opto-electronic transfer function, which converts linear scene light into the video signal, typically within a camera.
-   **EOTF**: electronic-opto transfer function, which converts the video signal into the linear light output of the display.
-   **OOTF**: opto-optical transfer function, which has the role of applying the ‘rendering intent’.

In television systems the displayed light is not linearly related to the light captured by the camera. Instead an overall non-linearity is applied, the OOTF.


### 6 HLG HDR-TV {#6-hlg-hdr-tv}

> the HLG HDR-TV signal parameters were designed to provide a significant degree
> of compatibility on BT.2020 colour SDR displays
>
> The design of the HLG HDR signal parameters is intended to allow distribution
> networks to provide a single HEVC Main 10 bitstream that can target both SDR and
> HDR receivers, where those SDR receivers support the BT.2020 colour container

所以我们说 HLG 向下兼容其实不是向下兼容 Rec.709 的显示设备，而是向下兼容 Rec.2020 的 SDR 设备，神奇吧？还有 Rec.2020 的 SDR 设备。貌似 Rec.2020 这个标准就是为 SDR 制定的，相当于在 SDR 的基础上只拓展了色域范围，从 Rec.709 到 Rec.2020。上面第二句话进一步验证了这一点，明确指出这些 SDR 接收端是支持 BT.2020 色欲的接收端。


#### 6.1 The hybrid log-gamma opto-electronic transfer function (OETF) {#6-dot-1-the-hybrid-log-gamma-opto-electronic-transfer-function--oetf}

> The dynamic range of modern video cameras is considerably greater than can be
> conveyed by a video signal using a conventional OETF gamma curve (e.g.
> Recommendation ITU-R BT.709 or Recommendation ITU-R BT.2020). In order to
> exploit their full dynamic range conventional video cameras sometimes use a
> ‘knee’ characteristic to extend the dynamic range of the signal. The knee
> characteristic compresses the image highlights to prevent the signal from
> clipping or being ‘blown out’ (overexposed). Knee characteristics are discussed,
> for example, in “Circles of Confusion”, by Alan Roberts, published by the EBU.
> The ‘shoulder’ characteristic of conventional photochemical film used in movie
> cameras provides a similar effect.
>
> When a hybrid log gamma HDR video signal is displayed on a conventional SDR
> display the effect is similar to the use of a digital camera with a knee or
> using film. It is not surprising therefore, that the HLG video signal is highly
> compatible with conventional SDR displays, because what you see is very similar
> to the signal from an SDR camera. Indeed the ‘knee’ characteristic of the HLG
> OETF, defined in Table 5 of Recommendation ITU-R BT.2100 (and shown in Fig. 18
> below), provides an extended highlight range that is comparable to some ‘knees’
> used for SDR. Note that the ‘knee’ curve in the Figure is diagrammatic for
> illustrative purposes only. Whilst knees are sometimes described in the
> literature as linear, as in this Figure, in practice they are ‘smooth’ and avoid
> the discontinuous gradient shown here, which can result in objectionable colour
> shifts.

现代视频摄像机的动态范围远远超过传统 OETF Gamma 曲线（例如 ITU-R BT.709 或 BT.2020 推荐标准）所能传达的范围。为了充分利用摄像机的动态范围，传统视频摄像机有时会使用一种“膝点（knee）”特性来扩展信号的动态范围。

膝点特性会压缩图像中的高光部分，以防止信号削波或“过曝”（blown out）。关于膝点的相关讨论可见于 EBU 出版、Alan Roberts 撰写的《Circles of Confusion》。传统胶片摄影机使用的光化学胶片中，也存在类似的“肩部（shoulder）”特性，产生类似效果。

当一个 Hybrid Log Gamma（HLG）HDR 视频信号在传统 SDR 显示器上显示时，呈现出的效果与使用带有膝点的数码摄像机或胶片类似。因此，HLG 视频信号与传统 SDR 显示器高度兼容也就不足为奇了，因为其视觉效果非常接近 SDR 摄像机的信号。

实际上，HLG 的 OETF 所定义的“膝点”特性（见 ITU-R BT.2100 推荐标准的表 5，以及下方图 18 所示）提供了一个扩展的高光范围，与某些 SDR 所使用的膝点相当。需要注意的是，图中的膝点曲线仅为示意用途。虽然文献中有时将膝点描述为线性的（如该图所示），但在实际应用中，它们通常是“平滑”的，以避免图中那种存在不连续梯度的表现方式，因为后者可能导致令人不悦的颜色偏移。


## HLG 标准提出的时间 {#hlg-标准提出的时间}

1.  ARIB STD-B67 日本电波产业会（ARIB）最初提出
2.  ITU-R BT.2100-1
3.  ITU-R BT.2100-2, ITU-R BT.2100-3
4.  ITU-R BT.2390-0 到 ITU-R BT.2390-8
5.  ITU-R BT.2408 系列，实操


## Notes {#notes}

-   摄影机捕捉的是叫作 OETF，是 Log，它的曲线是向上突出。显示器发出的光是 EOTF，它的曲线是向下凹陷。
-   Inverse OETF 有时候也会看别人写成 \\( OETF^{-1} \\)，相当于 OETF 的反函数。Inverse OETF 不一定就是 EOTF。
-   我们的成片文件，比如 Pr 输出的 Rec.709 Gamma 2.4 规格的这个文件本身，Pr 的编码结果、这个文件的本源实在，你可以把它想象成是一个向上突出的曲线，这条曲线是 Inverse EOTF。它的作用是给到显示器之后，显示器自身的 EOTF（Gamma 2.2、2.4）就把它 cancel out 了。于是我们最后得到 linear light。
-   显示器做的事情是 1 over Gamma 2.2 或者 2.4：( \frac{1}{2.2}=0.45 ), 1/2.4=0.42
