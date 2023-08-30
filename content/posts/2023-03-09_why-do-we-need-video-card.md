---
title: "为什么要使用上屏卡"
date: 2023-03-09
draft: false
tags: ["Calibration", "DisplayCAL", "ICC Profile", "Video Card"]
---

做高端校色软件 ColourSpace 的 LightIllusion 有篇文章 [What's wrong with ICCs?](https://www.lightillusion.com/what_is_wrong_with_iccs.html) 指出了 ICC profile 的一些问题。这非常关键，因为我们目前的校色流程就是基于 ICC profile 的。

总的来说，ICC profile 能 work 的前提是软件本身支持它，软件是否是 ICC aware。通常来说 ICC aware 的软件有一个 CMM 色彩管理模块（Color Management Module）能够正确识别我们生成的 ICC profile，然后能够对图像做调整以达到「准确」的结果，实现校色的目的。先不说 ICC 这种方式本质上是对图像的调整，而不是对显示器本身的校正（后者是更好的方式，所谓硬件校准就是如此）。即使软件具有 CMM，准确度和性能还得看各个软件自身的优化。这就变成一件非常麻烦和复杂的事情。那些不是 ICC aware 的软件，看到的就做一个完全未经校准的画面。

那对于 Adobe 全家桶来说，像 Ps、Lr 这种平面设计软件，它们是有 CMM 模块，叫作 ACE（Adobe Color Engine）。就是能够识别图像自带的 ICC profile，将其转换到 Ps 的工作色彩空间。那对于 Pr 来说，它也有被他们称为 DCM（Display Color Management）的东西，作为 Pr 内在的色彩管理方式，也是利用系统的 ICC profile 来进行工作的。

> DCM uses the ICC profile for your monitor to apply a colorimetric conversion from the sequence working color space (or media color space in the case of the source monitor) to the monitor color space. The profile is specified by the operating system. We generally recommended using the default profile that the system choses for your display. If your monitor has been professionally calibrated, use that profile. The default profile may have the name of your monitor or something generic like “Color LCD”. Using the specified display profile will ensure predictable colors across the system and in other apps like web browsers and email. DCM requires GPU Acceleration. GPU acceleration can be configured in Project Settings > General > Video Rendering and Playback. [^1]

ICC 主要还是平面设计领域的色彩管理方案，对于视频来说有很多局限性。虽然 ICC profile 是一种常见的色彩管理方式，但它并不是完美的，因为它只是对图像进行调整，而不是对显示器本身进行校正。使用上屏卡的一个最主要的原因就是绕过 ICC profile 乃至操作系统的整个色彩管理，更直接的获得一个干净的视频信号，而不去经过操作系统的色彩管理。而且对于监视器来说，一般需要是 SDI 接口，上屏卡也能满足这一点要求。

# Reference

[^1]: [New HDR Workflow in Premiere Pro - 2020 User Guide (PDF)](https://wpmedia-lib.larryjordan.com/wp-content/uploads/2020/09/New-HDR-Workflow-in-Premiere-Pro-2020-User-Guide.pdf)