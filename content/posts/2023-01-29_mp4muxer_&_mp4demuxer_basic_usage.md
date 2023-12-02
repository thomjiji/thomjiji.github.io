---
title: "mp4muxer & mp4demuxer Basic Usage"
date: 2023-01-29
draft: false
tags: ["Dolby Vision", "Dolby Atmos"]
slug: mp4muxer-mp4demuxer-basic-usage
---

## **Installation**

- [mp4muxer](https://github.com/DolbyLaboratories/dlb_mp4base)
- [mp4demuxer](https://github.com/DolbyLaboratories/dlb_mp4demux)

## Commands

1. 将 MP4 的音视频分离（脱壳 Demux）：

```shell
mp4demuxer --input-file input.mp4 --output-folder ~/Output
```

2. 合并杜比视界 Profile 8.4 和杜比全景声双音轨：

```shell
mp4muxer \
    -i input.h265 \
    --input-video-frame-rate 25 \
    --hvc1flag 0 \
    --dv-bl-compatible-id 4 \
    -i Stereo.aac \
    -i Atmos.ec3 \
    -o DolbyVision_DolbyAtmos.mp4
```

3. 合并 SDR 视频和杜比全景声双音轨:

```shell
mp4muxer \
    -i input.h264 \
    --input-video-frame-rate 25 \
    -i Stereo.aac \
    -i Atmos.ec3 \
    -o SDR_DolbyAtmos.mp4
```
