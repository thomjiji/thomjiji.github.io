---
title: "Baselight LOOK 添加更多的 DRT 和 Looks"
date: 2023-01-28
draft: false
tags: ["Baselight"]
---

## 操作步骤

1. Go to [Truelight Colour Spaces](https://www.filmlight.ltd.uk/support/customer-login/colourspaces/colourspaces.php), download:
   1. ARRI Look Library (LogC3)
   2. ARRI Look Library (LogC4)
   3. ARRI ALF-2 v5 DRT Family
   4. ARRI REVEAL DRT family
1. 将以上四个 zip 分别解压：
   1. 把 ARRI Look Library (LogC3) 文件夹里的所有文件拷贝到 => `/Library/Application Support/FilmLight/looks`
   2. 把 ARRI Look Library (LogC4) 文件夹里的所有文件拷贝到 => `/Library/Application Support/FilmLight/looks`
   3. 把 ARRI ALF-2 v5 DRT Family 文件夹里的所有文件拷贝到 => `/Library/Application Support/FilmLight/etc/colourspaces`
   3. 把 ARRI REVEAL DRT family 文件夹里的所有文件拷贝到 => `/Library/Application Support/FilmLight/etc/colourspaces`
1. 打开 Baselight LOOK，完成。

要实现预期的视觉效果，你必须在输出端应用 ARRI DRT 系列：LogC3 库使用 ALF-2，LogC4 库使用 REVEAL。或者，你也可以使用 T-CAM 的 Scene Look（例如 C-302 ARRI-2018），通过 T-CAM DRT 模拟 ARRI DRT 的视觉效果。

## 参考链接

1. [Truelight Colour Spaces](https://www.filmlight.ltd.uk/support/customer-login/colourspaces/colourspaces.php)
1. [ARRI Colour Workflow in Baselight](https://www.youtube.com/watch?v=EGmSWRJkCt0&t=343s)
1. [ACES and Baselight](https://www.youtube.com/watch?v=QTwWS8hluJk&t=730s)
1. [ARRI Look Library and ALF-2 v5 DRT Family Download](https://www.filmlight.ltd.uk/support/customer-login/colourspaces/colourspaces.php)
