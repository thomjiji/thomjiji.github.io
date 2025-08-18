---
title: "Nuke Learning Resources"
date: 2025-07-12
tags: ["colorscience", "nuke"]
draft: true
ShowToc: false
---

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


## Color Science {#color-science}


### Troy James Sobotka {#troy-james-sobotka}


#### [Question #6: What the F\*ck is a Transfer Function?](https://hg2dc.com/2019/05/28/question-6/) {#question-6-what-the-f-ck-is-a-transfer-function}


#### [Picture Formation Definition](https://github.com/sobotka/scise/wiki/Picture-Formation) {#picture-formation-definition}


#### [Troy's Mastodon about picture formation](https://mastodon.art/@troy_s/114569030157238196) {#troy-s-mastodon-about-picture-formation}


### Jed Smith {#jed-smith}


#### [Per-Channel Display Transform with Wider Rendering Gamut](https://community.acescentral.com/t/per-channel-display-transform-with-wider-rendering-gamut/3768) {#per-channel-display-transform-with-wider-rendering-gamut}


#### 经典的 Tonemap 函数：[open display transform wiki &gt; tonescale &gt; hyperbola](https://github.com/jedypod/open-display-transform/wiki/tech_tonescale#hyperbola) {#经典的-tonemap-函数-open-display-transform-wiki-tonescale-hyperbola}

<!--list-separator-->

-  这个函数在 thatcher freeman 的[这个](https://youtu.be/Z1qC1LuwMLw?si=uf-jidGg4cOO8qSU&t=175) DCTL 教学视频得到验证，他用的也是这个函数


#### 关于 Nuke Grade node 里的 Gamma 实际上不是纯 Gamma 函数，而是在像素值超过 1 之后使用的是一个线性函数：[The Math of Color Grading in Nuke](https://youtu.be/umpA40uheIs?si=jUnCxGtiudqSwDIR&t=400) {#关于-nuke-grade-node-里的-gamma-实际上不是纯-gamma-函数-而是在像素值超过-1-之后使用的是一个线性函数-the-math-of-color-grading-in-nuke}
