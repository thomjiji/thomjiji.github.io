---
title: "Nuke Learning Resources"
date: 2025-07-12
tags: ["colorscience", "nuke"]
draft: true
ShowToc: false
---

## Color Science {#color-science}


### Troy James Sobotka {#troy-james-sobotka}

-   [Picture Formation Definition](https://github.com/sobotka/scise/wiki/Picture-Formation)
-   [Troy's Mastodon about picture formation](https://mastodon.art/@troy_s/114569030157238196)
-   [Question #6: What the F\*ck is a Transfer Function?](https://hg2dc.com/2019/05/28/question-6/)
-   [Apelles and the Birth of Illusionism: Ancient lessons in painting spatial depth](https://surfacefragments.blogspot.com/2017/04/apelles-and-birth-of-illusionism.html?m=1)


### Jed Smith {#jed-smith}


#### [Per-Channel Display Transform with Wider Rendering Gamut](https://community.acescentral.com/t/per-channel-display-transform-with-wider-rendering-gamut/3768) {#per-channel-display-transform-with-wider-rendering-gamut}


#### 经典的 Tonemap 函数：[open display transform wiki &gt; tonescale &gt; hyperbola](https://github.com/jedypod/open-display-transform/wiki/tech_tonescale#hyperbola) {#经典的-tonemap-函数-open-display-transform-wiki-tonescale-hyperbola}

-   这个函数在 Thatcher Freeman 的[这个](https://youtu.be/Z1qC1LuwMLw?si=uf-jidGg4cOO8qSU&t=175) DCTL 教学视频得到验证，他在视频中用的也是这个函数，可见这个函数确实很普遍、很 basic。


#### 关于 Nuke Grade node 里的 Gamma 实际上不是纯 Gamma 函数，而是在像素值超过 1 之后使用的是一个线性函数 {#关于-nuke-grade-node-里的-gamma-实际上不是纯-gamma-函数-而是在像素值超过-1-之后使用的是一个线性函数}

-   [The Math of Color Grading in Nuke](https://youtu.be/umpA40uheIs?si=jUnCxGtiudqSwDIR&t=400)


#### Self-crafted Log curve - XLog {#self-crafted-log-curve-xlog}

-   这个 Log 曲线的图像可视化（Desmos）：<https://www.desmos.com/calculator/9pgjgo1gq4>
-   Google Colab 里对这个 Log 曲线的数学推导和由来的记录，有很多讲解性的描述：<https://colab.research.google.com/drive/1NwjaD0NzNMzQeNQqZECj33PdcYGkeBM4>


#### Sigmoid function {#sigmoid-function}

-   <https://discuss.pixls.us/t/blender-agx-in-darktable-proof-of-concept/48697/526>
-   <https://colab.research.google.com/drive/1CjDunkNCuhWIoWox_reDist6fTEJIim2>


### Chris Brejon {#chris-brejon}

-   [Why Makes A Good Picture (Formation)?](https://chrisbrejon.com/articles/what-makes-a-good-picture-formation/)
-   [OCIO, Display Transforms and Misconceptions](https://chrisbrejon.com/articles/ocio-display-transforms-and-misconceptions/): 这篇文章写于之前，也广为传播


### “Friends don’t let friends view scene-linear imagery without an “S-shaped” view transform.” 这句话的出处 {#friends-don-t-let-friends-view-scene-linear-imagery-without-an-s-shaped-view-transform-dot-这句话的出处}

-   [Cinematic Color](https://cinematiccolor.org/) - Jeremy Selan
-   [Why Makes A Good Picture (Formation)?](https://chrisbrejon.com/articles/what-makes-a-good-picture-formation/) 这篇文章提到了这句话。
-   然后其他好多地方也经常听到别人引用。


### Juan Pablo Zambrano DaVinci DRT implementation {#juan-pablo-zambrano-davinci-drt-implementation}

之前在 acescentral 论坛里就看到 Juan Pablo Zambrano 贴出了 DaVinci DRT 他的 (re)implementation，[给出了 Nuke 节点](https://community.acescentral.com/t/resolve-color-management-drt-in-ocio/5289/4?u=thomjiji)。虽然已经非常接近真相了，我可以打开他给出的 Nuke 节点看到内部究竟由哪些操作构成，但打开一看，喔有几个节点和操作，其中一个叫作 NakaRush 的节点貌似是整个的核心，影调映射看来就是由它完成的。但是，NakaRush？啥？但也没深究下去，就把那个 Nuke 节点存下来了。

今天我在哪又看到了 NakaRush 这个词，不出意外应该就是在 pixls.us 论坛的[这个回复](https://discuss.pixls.us/t/blender-agx-in-darktable-proof-of-concept/48697/662)。我就记得今天看到这个词的时候我立马又想起这个词在我 Nuke 脚本里的哪个地方有！我存下来的。那么 NakaRush 究竟是个什么，似乎有了更多眉目。

再然后，今天晚上时候我在按照 OpenDRT 项目 GitHub 上的 Wiki page，继续实现我的 Naive Tone Mapping 的时候，突然看到这，它也提到了 NakaRush，而且是更为详细的称呼：Naka-Rushton equation。这下水落石出，豁然开朗了：

> I did a lot of poking around comparing different approaches in the realm of sigmoid functions. Amongst my many dead ends and failed experiments, I did come across a super interesting bit of research describing the behavior of cone cells adapting to changes in stimulus with the **Michaelis-Menten equation** and its derivations like the Hill-Langmuir equation and the **Naka-Rushton equation**. These functions are usually used to model biological phenomena like enzyme kinetics, but the shape of the function fits very well the problem at hand. There are many scientific studies linking these. Of course this equation does not claim to model any of the higher order brightness processing that happens in the human visual system after the cone cells.

其中的 **Michaelis-Menten equation** 我记得 ACES 2.0 或者之前的 ACES 1.x 版本在用，这个相比 Naka-Rushen 甚至更耳熟一点。

1.  <https://community.acescentral.com/t/resolve-color-management-drt-in-ocio/5289/4>
    1.  达芬奇 CST Tone Mapping 下的 adaptation 是什么：<https://community.acescentral.com/t/resolve-color-management-drt-in-ocio/5289/8>
2.  <https://discuss.pixls.us/t/blender-agx-in-darktable-proof-of-concept/48697/662>
    1.  <https://www.desmos.com/calculator/qkvcafnuzr>
3.  <https://github.com/jedypod/open-display-transform/wiki/tech_tonescale#primate-cone-cells>


## Download Nuke Software {#download-nuke-software}

<https://www.foundry.com/products/nuke/download>


## Tcl Expression and Python in Nuke {#tcl-expression-and-python-in-nuke}

-   [Tcl Functions in Text Node - GATIMEDIA](https://www.gatimedia.co.uk/tcl-functions)
-   [Expressions &amp; Tcl Tips - GATIMEDIA](https://www.gatimedia.co.uk/expressions)
-   [Python &amp; TCL Scripts &amp; Tricks - Sacha Djordjevic](https://sachadjordjevic.com/index.php/nuke-python-tcl-scripts-tricks/)
-   [Expressions 101 - Written Tutorials - Nukepedia](https://www.nukepedia.com/written-tutorials/expressions-101)


## Unlock HDR Preview in Nuke (Apple EDR) {#unlock-hdr-preview-in-nuke--apple-edr}

-   [Working with HDR Images - Nuke User Guide](https://learn.foundry.com/nuke/content/getting_started/using_interface/hdr_output.html)


## Other Resources {#other-resources}

-   <https://benmcewan.com/blog/>


## Useful Articles {#useful-articles}

-   [Analysing the grade node from a mathematical perspective](https://guillermoalgora.com/grade-node-analysis.html)
-   [Understanding HSVL Within The Nuke Context](https://guillermoalgora.com/understanding-hsvl.html)
-   [A free suite of Nuke tools &amp; scripts](https://www.hiramgifford.com/buddy-system)
-   [Q100319: How to use colorspaces in Nuke?](https://support.foundry.com/hc/en-us/articles/115000229764-Q100319-How-to-use-colorspaces-in-Nuke)
-   [Q100330: Generating chromaticity diagrams](https://support.foundry.com/hc/en-us/articles/115000229804-Q100330-Generating-chromaticity-diagrams)
