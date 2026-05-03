---
title: "Color Science Things"
date: 2025-08-20
tags: ["Color Management", "Color Science", "Nuke", "Tone Mapping", "Tonescale"]
math: true
draft: false
---

关于广泛的“色彩科学”这一话题，以下是我零星的来源和笔记。这里权当做一个记录，同时帮助我厘清思路：


## Troy James Sobotka {#troy-james-sobotka}

-   [Picture Formation Definition](https://github.com/sobotka/scise/wiki/Picture-Formation)
-   [Troy's Mastodon about picture formation](https://mastodon.art/@troy_s/114569030157238196)
-   [Question #6: What the F\*ck is a Transfer Function?](https://hg2dc.com/2019/05/28/question-6/)
-   [Apelles and the Birth of Illusionism: Ancient lessons in painting spatial depth](https://surfacefragments.blogspot.com/2017/04/apelles-and-birth-of-illusionism.html?m=1)


## Jed Smith {#jed-smith}


### [Output Transform Tone Scale](https://community.acescentral.com/t/output-transform-tone-scale/3498): All about tonescale functions: MM, Naka-Rushton, Sigmoid, Daniele, etc. {#output-transform-tone-scale-all-about-tonescale-functions-mm-naka-rushton-sigmoid-daniele-etc-dot}


### [Per-Channel Display Transform with Wider Rendering Gamut](https://community.acescentral.com/t/per-channel-display-transform-with-wider-rendering-gamut/3768): ARRI K1S1 implemented in Nuke node {#per-channel-display-transform-with-wider-rendering-gamut-arri-k1s1-implemented-in-nuke-node}


### 经典的 Tone Mapping 函数 (Reinhard function)：[open display transform wiki &gt; tonescale &gt; hyperbola](https://github.com/jedypod/open-display-transform/wiki/tech_tonescale#hyperbola) {#经典的-tone-mapping-函数--reinhard-function--open-display-transform-wiki-tonescale-hyperbola}

-   这个函数在 Thatcher Freeman 的[这个](https://youtu.be/Z1qC1LuwMLw?si=uf-jidGg4cOO8qSU&t=175) DCTL 教学视频得到验证，他在视频中用的也是这个函数，可见这个函数确实很普遍，很 basic。


### Self-crafted Log curve - XLog {#self-crafted-log-curve-xlog}

-   这个 Log 曲线的图像可视化（Desmos）：<https://www.desmos.com/calculator/9pgjgo1gq4>
-   Google Colab 里对这个 Log 曲线的数学推导和由来的记录，有很多讲解性的描述：<https://colab.research.google.com/drive/1NwjaD0NzNMzQeNQqZECj33PdcYGkeBM4>


### Sigmoid function {#sigmoid-function}

-   <https://discuss.pixls.us/t/blender-agx-in-darktable-proof-of-concept/48697/526>
-   <https://colab.research.google.com/drive/1CjDunkNCuhWIoWox_reDist6fTEJIim2>


### 关于 Nuke Grade node 里的 Gamma 实际上不是纯 Gamma 函数，而是在像素值超过 1 之后使用的是一个线性函数 {#关于-nuke-grade-node-里的-gamma-实际上不是纯-gamma-函数-而是在像素值超过-1-之后使用的是一个线性函数}

-   [The Math of Color Grading in Nuke](https://youtu.be/umpA40uheIs?si=jUnCxGtiudqSwDIR&t=400)


### I can understand scene-linear, but what is display-linear? {#i-can-understand-scene-linear-but-what-is-display-linear}


#### magic numbers {#magic-numbers}

> OpenDRT:
> 	scene-linear 0.18 (mid grey) =&gt; display-linear 0.11
> 	scene-linear 0.09            =&gt; display-linear 0.04
> DaVinci:
> 	scene-linear 0.18 (mid grey) =&gt; display-linear 0.17
> 	scene-linear 0.09            =&gt; display-linear 0.09


#### <https://discuss.pixls.us/t/opendrt-for-art/49257/19> {#https-discuss-dot-pixls-dot-us-t-opendrt-for-art-49257-19}

> Yes. OpenDRT is designed with digital cinema workflows in mind, and follows the convention in that domain to **map “scene-linear” middle grey 0.18 to “display-linear” ~0.11, placing middle grey at around 11% of the overall range or at about 11 nits on a 100 nits Rec.1886 SDR display**. This is the convention used by pretty much all camera manufacturer image formation LUTs (except RED), and Resolve and Baselight, and follows the placement of middle grey of the original Rec.709 OETF.
>
> This is a creative adjustment however, as you can tell with the Umbra tonescale preset, which places middle grey about 1 stop lower at 6 nits (This is an increasingly common trend among high end feature films and episodics and shows the influence of HDR image formation bleeding over into SDR). The choice of where to place middle grey through the image formation is arbitrary, because at the end of the day it is just a pivot point for creative decision making about exposure. I see how it could be surprising in this context however, as it seems to normal approach is to start from a blank slate with just the display encoding and build up an “Image Formation” with various adjustments for every picture or preset (And obviously the display encoding roughly maps all pixel values between 0-1 from scene pixel intensity to display light with no transformation (ignoring colorimetry here)).
>
> In OpenDRT’s tonescale parameters, one would adjust this with tn_Lg or Grey Luminance. This adjustment is pretty much equivalent to an exposure adjustment in linear. I did find it kindof annoying constantly having to jump between the special effects tab and the exposure tab just to adjust exposure (which pretty much always needs to be creatively adjusted per picture anyways), so maybe it would be useful in the context of ART to expose this parameter for the user and remove it from the tonescale presets.


#### <https://community.acescentral.com/t/resolve-color-management-drt-in-ocio/5289/8> {#https-community-dot-acescentral-dot-com-t-resolve-color-management-drt-in-ocio-5289-8}

> It's just a regular per channel sigmoid, Resolve’s CST has the option to change something they call “adaptation”, **the default of “9” maps 0.09 scene linear to 0.09 display linear, and 100 scene linear to be 1**. This is the same process Resolve uses for their “DaVinci Tone mapping”, and finally, in Resolve when you check the “forward ootf” and gamma 2.4 in reality is just the Rec709 invEOTF.


#### <https://github.com/jedypod/open-display-transform/wiki/tech_tonescale#hyperbola> {#https-github-dot-com-jedypod-open-display-transform-wiki-tech-tonescale-hyperbola}

> Scale
> Let's see what other variables we can add. If we a scale for the input x and a scale for the output y this could be useful. This would allow us to control exposure of both input and output values.
>
> $$(y = s_y * \frac{x}{x + s_x})$$
>
> Now **we can adjust the constants s_x to adjust the scene-linear input exposure, and s_y to adjust the display-linear output**.


### Jed Smith crafted tonescale functions (Colab) {#jed-smith-crafted-tonescale-functions--colab}

-   [tonescale_functions.ipynb](https://colab.research.google.com/drive/1aEjQDPlPveWPvhNoEfK4vGH5Tet8y1EB)
-   [tonescale_model_selects.ipynb](https://colab.research.google.com/drive/10C3HvDuoAhYad1qOG2r0v8fGR-5VdpO5#scrollTo=scmqBhMsU4OL)
-   Above two links come from [here](https://community.acescentral.com/t/output-transform-tone-scale/3498/189).


## Chris Brejon {#chris-brejon}

-   [Why Makes A Good Picture (Formation)?](https://chrisbrejon.com/articles/what-makes-a-good-picture-formation/)
-   [OCIO, Display Transforms and Misconceptions](https://chrisbrejon.com/articles/ocio-display-transforms-and-misconceptions/): 这篇文章写于之前，也广为传播


## “Friends don’t let friends view scene-linear imagery without an “S-shaped” view transform.” 这句话的出处 {#friends-don-t-let-friends-view-scene-linear-imagery-without-an-s-shaped-view-transform-dot-这句话的出处}

-   [Cinematic Color](https://cinematiccolor.org/) - Jeremy Selan
-   [Why Makes A Good Picture (Formation)?](https://chrisbrejon.com/articles/what-makes-a-good-picture-formation/) 这篇文章提到了这句话。
-   然后其他好多地方也经常听到别人引用。


## Juan Pablo Zambrano DaVinci DRT Implementation {#juan-pablo-zambrano-davinci-drt-implementation}

之前在 acescentral 论坛里就看到 Juan Pablo Zambrano 贴出了 DaVinci DRT 他的 (re)implementation，[给出了 Nuke 节点](https://community.acescentral.com/t/resolve-color-management-drt-in-ocio/5289/4?u=thomjiji)。虽然已经非常接近真相了，我可以打开他给出的 Nuke 节点看到内部究竟由哪些操作构成，但打开一看，喔有几个节点和操作，其中一个叫作 NakaRush 的节点貌似是整个的核心，影调映射看来就是由它完成的。但是，NakaRush？啥？但也没深究下去，就把那个 Nuke 节点存下来了。

今天我在哪又看到了 NakaRush 这个词，不出意外应该就是在 pixls.us 论坛的[这个回复](https://discuss.pixls.us/t/blender-agx-in-darktable-proof-of-concept/48697/662)。我就记得今天看到这个词的时候我立马又想起这个词在我 Nuke 脚本里的哪个地方有！我存下来的。那么 NakaRush 究竟是个什么，似乎有了更多眉目。

再然后，今天晚上时候我在按照 OpenDRT 项目 GitHub 上的 Wiki page，继续实现我的 Naive Tone Mapping 的时候，突然看到[这一段](https://github.com/jedypod/open-display-transform/wiki/tech_tonescale#primate-cone-cells)，它也提到了 NakaRush，而且是更为详细的描述：Naka-Rushton equation。这下水落石出，豁然开朗了：

> I did a lot of poking around comparing different approaches in the realm of sigmoid functions. Amongst my many dead ends and failed experiments, I did come across a super interesting bit of research describing the behavior of cone cells adapting to changes in stimulus with the **Michaelis-Menten equation** and its derivations like the Hill-Langmuir equation and the **Naka-Rushton equation**. These functions are usually used to model biological phenomena like enzyme kinetics, but the shape of the function fits very well the problem at hand. There are many scientific studies linking these. Of course this equation does not claim to model any of the higher order brightness processing that happens in the human visual system after the cone cells.

NakaRush 是一个函数、公式、等式。达芬奇 CST Tone Mapping 如果选 DaVinci，似乎就是这个函数在起作用。这段话中也提到 Michaelis-Menten equation，这个我记得 ACES 2.0 或者之前的 ACES 1.x 版本在用，这个相比 Naka-Rushton 甚至更耳熟一点。

-   <https://community.acescentral.com/t/resolve-color-management-drt-in-ocio/5289/4>
    -   达芬奇 CST Tone Mapping 选项下的 adaptation 是什么：<https://community.acescentral.com/t/resolve-color-management-drt-in-ocio/5289/8>

> It's just a regular per channel sigmoid, Resolve’s CST has the option to change something they call “adaptation”, **the default of “9” maps 0.09 scene linear to 0.09 display linear, and 100 scene linear to be 1**. This is the same process Resolve uses for their “Davinci Tone mapping”, and finally ,in Resolve when you check the “forward ootf” and gamma 2.4 in reality is just the Rec709 invEOTF

-   <https://discuss.pixls.us/t/blender-agx-in-darktable-proof-of-concept/48697/662>
    -   <https://www.desmos.com/calculator/qkvcafnuzr>
-   <https://github.com/jedypod/open-display-transform/wiki/tech_tonescale#primate-cone-cells>


## 3x3 Matrix {#3x3-matrix}

-   [Color depth - DXOMARK](https://www.dxomark.com/glossary/color-depth/)
