---
title: "Demystifying Saturation"
date: 2025-08-19
tags: ["colorscience", "nuke"]
draft: true
ShowToc: false
---

## Understanding HSVL Within The Nuke Context {#understanding-hsvl-within-the-nuke-context}

> Saturation is independent of Hue or Luminance and is an indicator of the intensity of a colour. Highly saturated colours appear "stronger" and by increasing Saturation we are "revealing" more of the Hue present in the color, for example, being "more red", "more blue", etc. This is because, while zero Saturation (i.e. grey) indicates that all the three channels have the same value (R=G=B) and low Saturation implies that the three channels are closely aligned, high Saturation signifies a strong disparity in the values of the three channels. Increasing Saturation pulls the values in the channels further apart, thus increasing the dominant Hue, while reducing Saturation pulls the values from the channels closer together.

饱和度与色相或亮度无关，它是颜色强度的一个指标。高饱和度的颜色看起来更“强烈”，而当我们增加饱和度时，相当于“揭示”出更多该颜色中存在的色相，例如“更红”、“更蓝”等。这是因为，当饱和度为零（即灰色）时，三个通道的值相同（R=G=B）；而低饱和度意味着三个通道的值非常接近；高饱和度则表示三个通道的值差异很大。增加饱和度会把各通道的数值拉得更分散，从而增强主导的色相；而降低饱和度则会把各通道的数值拉得更接近。


## Camerimage 2018: Session Two: Natural Colours &amp; Texture | A View on Colours &amp; Texture in Painting (FilmLight - Daniele Siragusano) {#camerimage-2018-session-two-natural-colours-and-texture-a-view-on-colours-and-texture-in-painting--filmlight-daniele-siragusano}

同色异谱（Metamerism）是影像领域的一个基本原理。现实世界中，光和物体的颜色本质上是一个覆盖可见光波段的连续光谱；而“颜色”只是人类大脑对这种光谱的主观感知。在显示器上再现颜色时，情况发生了巨大简化：显示器并不会还原原始场景的完整光谱，而是通过三个数值（R、G、B）来合成光。由这三个分量组合产生的光谱，与现实场景中的原始光谱往往完全不同。然而，尽管两者的光谱差异巨大，它们却能在人眼中引发相同的颜色感知。这就是“同色异谱”现象的核心。

在一个场景中，最亮的反射体通常是白色物体。它们几乎均匀地反射整个可见光谱的光线。相比之下，现实世界中能让人感知到“某种颜色”或“鲜艳的颜色”的物体，其实是因为它们选择性地反射了一部分光谱，而吸收了更多光谱能量。

颜色的饱和度越高，意味着该物体吸收的光谱范围越广，留下的反射部分越少。比如一个红苹果之所以显得是红色，是因为它主要反射红色波段的光，而吸收了其他波段的光。如果一个苹果显得“更红”，那就说明它吸收了更多非红色波段的光。由此可见，这种红色的饱和度存在物理上的上限。

在自然状态的物理世界里，对于反射光物体来说，饱和度越高，亮度（luminance）往往就越低。因此，在调色时，如果提高某个色相的饱和度，通常也需要适度降低它的亮度，才能保持观感上的合理性。

宽色域显示器的问题在于，它能够显示出超过自然极限的高饱和度颜色。例如，假设我们想要一个“理想红苹果”达到某个极端饱和度的色彩，那么在物理世界中，要么需要让它反射更多能量（也就是更强的入射光），要么需要让苹果的反射率远高于自然水平。但这样做的结果就是：这个苹果会显得“不自然”，甚至像是在发光。原因就在于，当饱和度超过自然上限时，相应的亮度会被人为推得过高，从而打破了现实中颜色与亮度的对应关系。


## Reference {#reference}

-   <https://guillermoalgora.com/understanding-hsvl.html>
-   <https://vimeo.com/333078338>
