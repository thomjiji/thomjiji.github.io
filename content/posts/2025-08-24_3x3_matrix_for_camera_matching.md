---
title: "3x3 Matrix for Camera Matching"
date: 2025-08-24
tags: ["Color Science"]
draft: true
ShowToc: true
---

## 前言 {#前言}

使用 3x3 矩阵来做摄影机的匹配不是一个新技术了。很久之前看过这个叫作 [Paradox](https://bsky.app/profile/ctcwired.paradoxwolf.info) 的人（他 day job 似乎是干广电的一个工程师、技术人员吧）做的一个[视频](https://www.youtube.com/watch?v=inLKBxAnlzU&t)，其中讲解了他是如何对 Fuji 的一台什么相机拍摄的 Log 素材和 iPhone 拍摄的 Log 素材进行匹配的，分享了详细的过程。

首先需要两台相机，源相机（source）和目标相机（target），在同一个照明环境下，拍摄同一张色卡。然后得到两条素材中色卡的值，计算两张色卡的差值，得到一个校正矩阵。将该校正矩阵应用到源相机（source）拍摄的素材上，就能实现与目标相机（target）的匹配。

他的过程需要用到达芬奇、colour-science.org 的 [colour](https://github.com/colour-science/colour) Python 库。我知道 Nuke 有一个 gizmo 叫作 [mmColorTarget](https://www.nukepedia.com/tools/gizmos/colour/mmcolortarget/)，可以不需要用到 [colour](https://github.com/colour-science/colour)，然后全部在 Nuke 里完成。在此将步骤简要记录：


## 测试图像 {#测试图像}

手边没有不同摄影机拍摄色卡的测试图，所幸 CML 有分享一组：

-   <https://www.cinematography.net/edited-pages/ACES.html>

第一张是 ARRI Alexa，第二、三张是 Sony F55 和 Sony F65，都是 RAW。这作为我们的这个匹配测试的测试图正正好。


## 步骤 {#步骤}

1.  将两张图裁切到色卡填充满整个画面，色彩空间转换到一个统一的广色域（最好大到能够承载摄影机原始色域，例如 DWG 或 ACES AP0）。我们这里选择 ACES，传递函数选择 Linear 线性。输出 EXR，ACES Linear。
2.  开启一个 Python 虚拟环境 .venv，安装这两个 python 包：colour 和 colour_checker_detection:
    ```bash
    python -m venv .venv
    source .venv/bin/activate
    pip install colour colour_checker
    ```
3.  运行脚本：
    ```python
    from colour import matrix_colour_correction, read_image
    from colour_checker_detection import detect_colour_checkers_segmentation

    source_filename = "/path/to/source/f65-sony-rv.mxf.chart.exr"
    target_filename = "/path/to/target/alexa-raw.ari.chart.exr"

    source = detect_colour_checkers_segmentation(read_image(source_filename))[0]
    target = detect_colour_checkers_segmentation(read_image(target_filename))[0]

    matrix = matrix_colour_correction(source, target)

    print(matrix)
    ```
4.  得到一个 matrix 像这样：
    ```bash
    [[ 1.87196901  0.06371339 -0.08137244]
     [-0.02488484  1.9985176  -0.00450315]
     [ 0.03458153 -0.06038139  1.77408664]]
    ```
5.  将这个 matrix 的 9 个值复制到 Nuke 的 ColorMatrix 节点，把节点加载给源 source 素材。简单点可以首先复制一个 ColorMatrix 节点到文本编辑器：
    ```bash
    set cut_paste_input [stack 0]
    version 16.0 v4
    push $cut_paste_input
    ColorMatrix {
     matrix {
          {1 0 0}
          {0 1 0}
          {0 0 1}
     }
     name ColorMatrix1
     note_font Helvetica
     selected true
     xpos -483
     ypos 1283
    }
    ```
6.  把刚输出的那串 matrix 复制到上面的 matrix 那里，把 `[]` 用 `{}` 替换：
    ```bash
    set cut_paste_input [stack 0]
    version 16.0 v4
    push $cut_paste_input
    ColorMatrix {
     matrix {
       { 1.87196901  0.06371339 -0.08137244}
       {-0.02488484  1.9985176  -0.00450315}
       { 0.03458153 -0.06038139  1.77408664}
     }
     name ColorMatrix1
     note_font Helvetica
     selected true
     xpos -483
     ypos 1283
    }
    ```
