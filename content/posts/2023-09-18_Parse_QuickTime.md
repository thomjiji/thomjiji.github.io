---
title: "Parse QuickTime"
date: 2023-09-18
draft: false
tags: ["Atom", "QuickTime", "NCLC Tag"]
---

Apple Developer Documentation:
- [Storing and sharing media with QuickTime files](https://developer.apple.com/documentation/quicktime-file-format/storing_and_sharing_media_with_quicktime_files)
	- [Color parameter atom ('colr')](https://developer.apple.com/documentation/quicktime-file-format/color_parameter_atom)

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