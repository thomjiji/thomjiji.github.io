---
title: "Dolby Vision Profile 8.4 & Dolby Atmos"
date: 2023-01-27T21:23:02+08:00
draft: false
ShowToc: true
tags: ["Dolby Vision"]
categories: ["HDR"]
author: ["thom"]
weight: 1
---

# 参考链接

### 关于上传杜比视界到平台（Bilibili or Vimeo）

- [How-To: Dolby Vision Encoding - Apple Compressor for Vimeo](https://professionalsupport.dolby.com/s/article/Dolby-Vision-encoding-Apple-Compressor?language=en_US#UsingInBuiltSettings)
- [Dolby Vision for Vimeo FAQs](https://professionalsupport.dolby.com/s/article/Dolby-Vision-for-video-sharing-services-FAQs?language=en_US#_Toc82103519)
- [Bilbili 技术发布的关于支持 HDR10 上传的文章](https://www.bilibili.com/read/cv7791059) —— 2020 年 9 月

### 关于制作杜比全景声 Dolby Atmos

- [Dolby Atmos Using Blackmagic Design DaVinci Resolve Studio](https://learning.dolby.com/hc/en-us/articles/360054574872-Module-11-3-Dolby-Atmos-Using-Blackmagic-Design-DaVinci-Resolve-Studio)
- [哔哩哔哩（B 站）杜比全景声/杜比音效稿件制作指南](https://mp.weixin.qq.com/s/iuW9wztwPV9bvtVaDQxhLQ) —— 以白菜的名义
- [EP05 杜比全景声家庭版的交付和编码封装](https://www.bilibili.com/video/BV1AT4y1c7Um?share_source=copy_web) —— 以白菜的名义

### 关于上传 HDR10 & HLG 到 Youtube

- [Upload High Dynamic Range (HDR) videos](https://support.google.com/youtube/answer/7126552?hl=en#zippy=%2Chdr-metadata%2Chdr-video-file-encoding%2Cupload-requirements)

### Dolby Vision Basics

- [Dolby Vision Metadata Levels](https://professionalsupport.dolby.com/s/article/Dolby-Vision-Metadata-Levels?language=en_US)
- [What are Dolby Vision Profiles?](https://professionalsupport.dolby.com/s/article/What-is-Dolby-Vision-Profile?language=en_US)
- [What are the HEVC requirements for Dolby Vision encoding?](https://professionalsupport.dolby.com/s/article/What-are-the-HEVC-requirements-for-Dolby-Vision-encoding?language=en_US)
- [HDR Monitor Consideration](https://professionalsupport.dolby.com/s/article/What-monitor-should-I-use-for-creating-Dolby-Vision?language=en_US)

### Common Dolby Vision Delivery Specifications

- [iTunes Video and Audio Asset Guide 5.3.8](https://help.apple.com/itc/videoaudioassetguide/en.lproj/static.html#itcae20df1c7)

# Some Topics

## What is Dolby Vision profile 8.4?

Dolby Vision profiles describe the video codec and set of coding techniques used to encode a Dolby Vision video. Profile 8.4 is a Dolby Vision profile where the 10-bit HDR video essence is encoded using HEVC with a Hybrid Log Gamma (HLG) transfer function and is accompanied by dynamic Dolby Vision metadata. This is the format that is used when capturing HDR video on iPhone 12 or when exporting Dolby Vision video from Final Cut Pro or Compressor.

Profile 8.4 is designed for direct distribution and playback on consumer devices (rather than, say, a mezzanine format optimized for OTT encoding). While there's a growing range of consumer devices that support Dolby Vision playback, Profile 8.4 encodes are also built with a cross-compatible HLG base layer, so that if a device does not support Dolby Vision decoding, it can often decode just the HLG base layer, which will present a viewable picture, although decoding just the base layer—without any Dolby Vision metadata—does not take advantage of the sophisticated display mapping available in Dolby Vision displays.

When Dolby Vision profile 8.4 files are played on Vimeo and on a Dolby Vision-capable device, the video player uses the Dolby Vision metadata to ensure that the content is mapped for the current display conditions.

## [MaxFALL/MaxCLL Basic](https://professionalsupport.dolby.com/s/article/Calculation-of-MaxFALL-and-MaxCLL-metadata?language=en_US)

### Background Info

MaxFALL/MaxCLL is metadata required for HDR10 content.

MaxFALL (Maximum Frame Average Light Level) indicates the maximum value of the frame average light level (in cd/m2 or nits) of the entire playback sequence. MaxFALL is calculated by averaging the decoded luminance values of all the pixels within a frame. MaxFALL is usually much lower than MaxCLL.

MaxCLL (Maximum Content Light Level) indicates the maximum light level of any single pixel (in cd/m2 or nits) of the entire playback sequence. MaxCLL is usually measured off the final delivered content after mastering. If one uses the full light level of the HDR mastering display and adds a hard clip at its maximum value, MaxCLL would be equal to the peak luminance of the mastering monitor.

**MaxCLL/MaxFALL together forms static metadata** as it uses the same values for the entire program, while Dolby Vision dynamic metadata changes on a shot by shot (or when required, frame by frame) basis.

Both HDR10 and Dolby Vision are based on the ST.2084 EOTF.

Different consumer HDR displays have different peak luminance and contrast capabilities, and more than often these fall below the capabilities of the mastering monitors used in professional post production. Therefore *in most cases, PQ encoded images will have to be scaled or mapped to match the capabilities of the target consumer display device. This scaling or mapping is controlled and managed by metadata*.

### MaxFALL/MaxCLL Calculation

*MaxFALL/MaxCLL metadata is not required for a Dolby Vision deliverable*, nevertheless, Dolby supports the effort to supply good and correct metadata to produce better end user entertainment experiences. MaxFALL/MaxCLL are not normally calculated as part of the Dolby Vision metadata creation process. They require a separate analysis that can be performed on systems like Colorfront Transkoder, MTI Cortex, etc.

Alternatively, MaxFALL/MaxCLL metadata can be calculated using Dolby's cm_analyze which is included in the Dolby Vision Professional Tools suite. **Once these values are calculated, they can be added to an existing Dolby Vision XML as Level6 (L6) metadata so as to deliver both Dolby Vision as well as HDR10 metadata in a single metadata asset.**

## How do I create a Dolby Vision project in Blackmagic Design DaVinci Resolve?

From an HDR sequence in Blackmagic Design Resolve, export to a mezzanine format (such as Apple ProRes 422 HQ) that supports PQ or HLG based HDR. Import this file into Apple Compressor and use a Dolby Vision 8.4 preset to transcode.

### Dolby Atmos Master Formats: ADM BWF

Source: [Module 1.2 – Beyond Multichannel Audio](https://learning.dolby.com/hc/en-us/articles/360052744252)

The Dolby Atmos Renderer records up to 128 inputs comprised of Bed and Object audio, OAMD as well as Binaural, Downmix, and Trim metadata, Input and Re-render configurations.

These are recorded to a Dolby Atmos Master File set (DAMF). This is the format native to the Dolby Atmos Renderer and is recorded as a three-file set comprised of:

- **atmos** — An XML file containing information about the Dolby Atmos presentation and index information about the other files in the file set. The .atmos file includes the number of inputs used as Beds or Objects, frame rate, file start, first frame of action, the number of elements used in spatial coding, downmix, and trim metadata.
- **atmos.metadata** — An XML file containing dynamic positional and size OAMD for each Object, along with binaural metadata settings.
- **atmos.audio** — A Core Audio Format (CAF) file of up to 128 tracks of interleaved audio.

The .atmos and .atmos.metadata files can be opened for inspection with a text editor. However, direct editing of these files is not recommended, as the file set can become corrupted.

While a new master is always recorded as a DAMF, two other formats are used for distribution, for encoding, or further editing:

- **ADM BWF** — The Audio Definition Model Broadcast Wav Format (ADM BWF) is an alternative Dolby Atmos master format. With ADM BWF (sometimes referred to as ADM BWAV), all other information included in the .atmos and .atmos.metadata files is included in a data chunk in the header of the wav file. The audio payload itself is up to 128 tracks of interleaved audio. ADM has several advantages:
    1. ADM BWF is a single file instead of three files in a folder, making it easy to interchange with other facilities.
    2. ADM BWF can be imported into DAWs. This allows all Bed and Object audio tracks to be recreated along with all the panning metadata. This allows for subsequent editing — language replacement, timing conformance, censorship edits, etc. — prior to remastering.
    3. ADM BWF can be encoded to Dolby True HD, Dolby Digital Plus JOC, and Dolby AC-4 IMS and is the primary deliverable to streaming operators and Blu-ray authoring.
- **IMF.IAB** – Immersive Audio Bitstream is a mezzanine format for IMF (interoperability mastering format). IAB is considered a mezzanine format rather than a master format, as OAMD is quantized. IAB.mxf is used by third-party IMF packaging tools to create a delivery container for both Dolby Atmos and video (including Dolby Vision).

While the Dolby Atmos Renderer natively records in the .atmos format only, it can convert to and export ADM BWF and IAB.MXF. The entire file can be exported, or basic top/tail (specified range) edits can be performed.

The Dolby Atmos Renderer can also open ADM BWF and IAB.MXF files as master files for playback, QC, basic top/tail editing, conversion (between the two formats), and re-export. However, some restrictions apply with open ADM BWF and IAB.MXF. Punch-in and other metadata editing are not permitted with ADM BWF and IAB.MXF. Conversion to .atmos from ADM BWF and IAB.MXF is not permitted.

The Dolby Atmos Conversion Tool (DACT) is a companion application to the Dolby Atmos Renderer and is required to convert from ADM BWF and IAB.MXF to .atmos, perform format and frame-rate conversions, as well as perform complex editing operations on master files. The Dolby Atmos Conversion Tool is a free utility.

## What is HLS and DASH

Source: [Dolby Vision Encoding of Mezzanine Assets](https://professionalsupport.dolby.com/s/article/Dolby-Vision-Encoding-of-mezzanine-assets?language=en_US)

In order to distribute content via OTT/VOD, all sources get encoded via a standard video encoder to compress and reduce the file size of the video content, followed by a packager that puts the encoded bitstream into streaming formats, like DASH or HLS.

HLS – HTTP Live Streaming – a format developed by Apple splits a video file into small segments that are contained within a MPEG2 transport stream or MP4, hence the extension “.ts” or “.mp4”. HLS requires an H.264 (AVC) or H.265 (HEVC) codec and is currently the only format supported by iOS, iPadOS, tvOS and Apple’s Safari browser.

DASH - Dynamic Adaptive Streaming over HTTP – also known as MPEG-DASH – splits a video file into small segments as well. Unlike HLS, DASH is codec agnostic. It supports H.264, H.265, VP9, and other codecs as well.

Both HLS and DASH use a separate manifest file that links to the segments (and/or other manifests). Such a manifest file is a simple text file that is understood by players supporting HLS and/or DASH.

# Notes

## 需要有这三个 metadata 才能被 Youtube 或 Bilibili 正确识别为 HDR10：

[Reference](https://support.google.com/youtube/answer/7126552?hl=en#zippy=%2Chdr-metadata%2Chdr-video-file-encoding%2Cupload-requirements): HDR videos using PQ signaling should also contain information about *the display it was mastered on* (SMPTE ST 2086 mastering metadata). It should also have details about the brightness (CEA 861-3 *MaxFALL and MaxCLL*). If it's missing, we use the values for the Sony BVM-X300 mastering display.

1. Mastering display luminance
2. Maximum Content Light Level (MaxCLL)
3. Maximum Frame-Average Light Level (MaxFALL)

## 杜比视界 Profile 8.4 B 站投稿标准

Source 1: [【全球首家】杜比视界用户投稿功能上线啦！](https://www.bilibili.com/read/cv12741112)—— 哔哩哔哩创作中心
Source 2: [【攻略】杜比视界UGC视频制作和分享操作指南](https://www.bilibili.com/read/cv12752764) —— 杜比 Dolby

- Bit Rate: 30000 kbps
- Resolution: 3840x2160 60FPS
- Bit Depth: 10bit
- Transfer Function: HLG
- Codec: HEVC (H.265)
- Dolby Vision Profile 8.4
- Container: MP4, MOV, M4V

## 杜比全景声 B 站投稿标准

- 全景声音轨
- 音频编码格式 E-AC-3（Dolby Atmos）
- 音频声道数 6（5.1）或者 8（7.1）
- 音频采样率 48kHz
- 音频码率 320kbps 以上

## 其他

HDR10 要求的 static metadata 由 MaxFALL 和 MaxCLL 组成，杜比视界的 L1 metadata 只是计算出 min ava max 三个值，在达芬奇的 Dolby Vision 面板里进行。

HDR10 和杜比视界都有元数据来控制影调的映射，在不同峰值亮度设备上的 Tone mapping。只是 HDR10 是所谓静态的元数据，而杜比视界是动态的。后者在 Tone mapping 的控制视界都有元数据来控制影调视界都有元数据来控制影调上更好。