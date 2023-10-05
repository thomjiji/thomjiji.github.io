---
title: "Parse QuickTime"
date: 2023-09-18
draft: false
tags: ["Atom", "QuickTime", "NCLC Tag", "ProRes"]
---

Apple Developer Documentation:
- [Storing and sharing media with QuickTime files](https://developer.apple.com/documentation/quicktime-file-format)
	- [Color parameter atom ('colr')](https://developer.apple.com/documentation/quicktime-file-format/color_parameter_atom)
- [Uncompressed Y´CbCr Video in QuickTime Files](https://developer.apple.com/library/archive/technotes/tn2162/_index.html#//apple_ref/doc/uid/DTS40013070-CH1-TNTAG9) — Documentation Archive

Other resources:
- [Apple ProRes - MultiMedia Wiki](https://wiki.multimedia.cx/index.php/Apple_ProRes)
- [QuickTime Tags - ExifTool](https://exiftool.org/TagNames/QuickTime.html)

Implementation:
- [metacolor.editor](https://github.com/piersdeseilligny/metacolor.editor) - C#
- [qtff-parameter-editor](https://github.com/bbc/qtff-parameter-editor/tree/master) by BBC - C++
- [AMCDXVideoPatcherCLI](https://mogurenko.com/) - Close source
- [qtfile_pp](https://github.com/da8eat/qtfile_pp) AMCDXVideoPatcherCLI 作者的开源 parser（想必 AMCDXVideoPatcherCLI 的具体实现能够从这里看出一些端倪）- C++

Inspection tools:
- hexdump -vC
- [fq](https://github.com/wader/fq)
- [MP4Box.js](https://gpac.github.io/mp4box.js/test/filereader.html) - a file inspection tool
- [mp4analyser](https://github.com/essential61/mp4analyser)

---

这么些处理 atom (包括 NCLC tag) 的实现，都是用 C++ 写的。其实 Rust 也能做这些 low-level 的事情，对吗？只是这门语言比较新，暂时还没有人用它来做这个事而已。那么我能不能来做呢？用 Rust 写一个 parse mov file 的实现。其实已经有了：[dryv](https://github.com/Stuff7/dryv)——这两天就在更新。

这需要对 low-level 底层的东西有所了解是吗？底层的东西我又不太会。但这无疑是一个机会和切入点，就像 Asahi Lina 说的她最开始入门 low-level 编程也是为了要 hack 任天堂的一个什么掌机。我需要写一个 parse QuickTime file 的 Rust 的实现，实现出来一定挺酷。

进一步的说，其他的一些元数据，比如时码、卷号，这些在 DIT 工作当中会碰到的需要对其做一些操作的元数据。现有的达芬奇、Pomfort 之流没有提供类似的功能，或者需要你进入到一整个流程当中，才能实现对某个文件修改元数据的需求。我可以做一个 goto 的锋利小工具。目前市面上有 [QTChange](https://www.videotoolshed.com/handcrafted-timecode-tools/qtchange/)，但它卖的挺贵，29.95 欧。上面提到的 [AMCDXVideoPatcher](https://mogurenko.com/) 的 GUI 好像连卷名也能修改。就真的好想知道具体是怎么办到的，可惜它没有公开源码，不过相比 QTChange 这个免费还要啥自行车。

---

我导出两个片段，一个 1-1-1，一个 1-2-1。除此之外其他一切都一样。最后使用 hexdump 查看两个文件的字节数 bytes 大小，发现相差 12 bytes，而 gama atom 恰好就是 12 btyes。差的就是这个 gama atom。

---

Decode ProRes: [RDD 36:2015 - SMPTE Registered Disclosure Doc - Apple ProRes Bitstream Syntax and Decoding Process](https://ieeexplore.ieee.org/document/7438722)。这个标准专门讲解了如何 decode ProRes 的 stream。而且，不能只修改 mov 这个 container 的 colr atom，还要修改 ProRes header 里的 Primaries, Transfer Function and Matrix 信息：

> In addition to the colour information carried within the Color Atom, information regarding the transfer function, colour matrix and primaries are also stored within the frame header information of the ProRes elementary stream, alongside other parameters, such as frame rate, spatial resolution and chroma format. This header is repeated throughout the bitstream. Full details of the header layout can be found in the [SMPTE specification](http://ieeexplore.ieee.org/document/7438722/).

A lot of work to do...

---

这里有两个 colr_atom-ish 的地方，一个是 MOV file format 本身的 colr atom。一个是 ProRes 编码的每一帧的 header（ProRes header），也有一个 colr_atom-ish 的东西：也是由 color primaries、transfer characteristic 和 matrix coefficients 组成。所以，如果想要实现修改 NCLC tag，不仅需要修改 colr atom（MOV file 里的），还要修改每一帧的那三个东西。MOV file format 的 colr atom 三个 tag 占 2 bytes（u16），ProRes frame 的三个 tag 只占 1 byte（u8）。

It looks like: 只有 Transfer characteristics 是 Unspecified 的状态，gama atom 才会起作用。如果 Transfer characteristics 是有值的，比如 BT.709，那么即使有 gama atom，比如使用 Mediainfo 查看 Gamma 为 2.4，也是不起作用的。ColorSync utility 仍然只会以 Transfer characteristics 为准，而忽略 gama atom。确实像之前听到别人所说，gama atom 像是 Apple 提供的一个后门，一个给影视软件对输出文件做正确 NCLC tagging 的后门。

---

今天（2023-09-30）实现了 overwrite colr atom，search 的算法还是用的之前的，因为之前的算法虽然的 debug build 下非常慢，但只要换到有优化的 release build 之后就非常快了（其实也不算非常快，只能说还算可以接受）。所以就先用着，继续实现后面的功能。

gama atom 这个东西的 FourCC (four character code) 是 `67 61 6d 61`。具体的值紧随其后，比如 `00 02 66 66` — Gamma 2.4，`00 02 33 33` — Gamma 2.2。如果要修改 Gamma 值，直接 overwrite 这 4 个 byte 就行了。QuickTime File Format 一个 [2001 的 PDF 文档](https://developer.apple.com/standards/qtff-2001.pdf)也说明 gama atom 是个 32-bit 的 [fixed-point number](http://www.sunshine2k.de/articles/coding/fp/sunfp.html#ch3)，也就是占 4 个 byte。如果需要去掉 gama atom，那么将那 4 个 byte 用 `00 00 00 00` overwrite 掉即可。这样的话，当我们需要把 1-2-1 转换到 1-1-1 的时候，首先修改 atom，然后去掉 gama atom。去掉 gama atom 的方法算是找到了。但是问题是如何添加上 gama atom，这可能才是我们想要的。目前没找到合适的方法让 gama atom 无中生有，添加一个 gama atom 到整个 file stream。难道要 shift 所有其他 bytes？

---

做 byte search 的话，会 match 到 两个 gama atom 的 pattern。更奇怪的是，对于 1-1-1 的文件，它没有 gama atom，但也能 match 到 gama atom 的那个 pattern：

```sh
~/Desktop ❯ grep "gama" 1-1-1_20mins_hex.txt
05861500  17 c5 96 57 02 3c 67 61  6d 61 ff 71 4d 86 fc e4  |...W.<gama.qM...|
```

但需要文件的时长够长。比如只有几秒的，就只有一个正常的 gama atom。会不会出现两个 gama atom 的 pattern，取决于这个 prores 文件的时长多长，我测试过 5 分钟没有，10 分钟开始有，20 分钟也有。

| File name          | Numbers of gama  | Notes                               |
|--------------------|------------------|-------------------------------------|
| `1-1-1_10mins.mov` | one gama pattern | It's not supposed to have!          |
| `1-1-1_20mins.mov` | one gama pattern | It's not supposed to have!          |
| `1-2-1_10mins.mov` | two gama pattern | It should has one, but we found two |
| `1-2-1_20mins.mov` | two gama pattern | It should has one, but we found two |

---

### icpf

frame:
- frame_size: u(32)
- frame_identifier: f(32)
- frame_header:
	- frame_header_size: u(16)
	- **reserved**
	- bitstream_version: u(8)
	- encoder_identifier: f(32)
	- horizontal_size: u(16)
	- vertical_size: u(16)
	- chroma_format: u(2)
	- **reserved**
- picture:
	- ...

```
00000020  6d 64 61 74 00 01 51 80  00 09 68 00 69 63 70 66  |mdat..Q...h.icpf|
00000030  00 94 00 00 61 70 6c 30  07 80 04 38 80 00 01 02  |....apl0...8....|
00000040  01 30 00 03 04 04 05 05  06 07 07 09 04 04 05 06  |.0..............|
```

Frame size is 4 bytes before the icpf.  In this case, it's `00 09 68 00`.
Frame header size is 4 bytes after the icpf. In this case, it's `00 94 00 00`.
After frame header size, it's "encoder_identifier": in this case, it's `61 70 6c 30`.

`(next) pos = previous (frame_size + pos)`

`current (pos + frame_size) = next (pos)`

---

修改 gama atom 值的功能实现了以后，我把一个 1-2-1 的 MOV 视频的 Gamma 值从 2.4 改成了 1.96。然后将其与 1-1-1 的MOV 视频对比，可以发现画面的反差一模一样。

---

Assimilate SCRATCH 的 Rec.709 Gamma 2.4 render 的行为是正常的，在 Output Settings > Data Format 中选择：

- Color: Rec709
- EOTF: Gamma 2.4

这样渲染出来的 ProRes MOV 文件的 tagging 为 1-2-1 Gamma 2.4。

SCRATCH 中，你也可以选择把文件特别的 tag 成 Gamma 2.2 或者 2.6 都可以，有 2.2 和 2.6 的选项。对于 atom_modifier 来说，你可以用它把你 1-2-1 文件的 Gamma 设置成任何值（讲道理除了 2.4、2.2、2.6 你也不需要其他值了），甚至负数。
