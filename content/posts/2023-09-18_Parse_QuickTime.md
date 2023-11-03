---
title: "Parse QuickTime"
date: 2023-09-18
draft: false
tags: ["Atom", "QuickTime", "NCLC Tag", "ProRes"]
---

### Apple Developer Documentation

- [Storing and sharing media with QuickTime files](https://developer.apple.com/documentation/quicktime-file-format)
	- [Color parameter atom ('colr')](https://developer.apple.com/documentation/quicktime-file-format/color_parameter_atom)
- [Uncompressed Y´CbCr Video in QuickTime Files](https://developer.apple.com/library/archive/technotes/tn2162/_index.html#//apple_ref/doc/uid/DTS40013070-CH1-TNTAG9) — Documentation Archive

### Other resources

- [Apple ProRes - MultiMedia Wiki](https://wiki.multimedia.cx/index.php/Apple_ProRes)
- [QuickTime Tags - ExifTool](https://exiftool.org/TagNames/QuickTime.html)
- [MPEG-4 files - Atomic Parsley](https://atomicparsley.sourceforge.net/mpeg-4files.html)
- [MP4RA - Official Registration Authority for the ISOBMFF family of standards](https://mp4ra.org/)

### Implementations/tools

| Implementations                                                                   | Language | Notes                                                                                                     |
| --------------------------------------------------------------------------------- | -------- | --------------------------------------------------------------------------------------------------------- |
| [metacolor.editor](https://github.com/piersdeseilligny/metacolor.editor)          | C#       | Simple Implementation, can only change NCLC tags. Provide GUI.                                            |
| [qtff-parameter-editor](https://github.com/bbc/qtff-parameter-editor/tree/master) | C++      | Good implementation, can only change NCLC tags. CLI.                                                      |
| [AMCDXVideoPatcherCLI](https://mogurenko.com/)                                    | C++      | Close source. Can modify colr atom, add gama atom (but value of gamma can't not be changed — seems a bug) |
| [qtfile_pp](https://github.com/da8eat/qtfile_pp)                                  | C++      | AMCDXVideoPatcherCLI 作者的开源 parser，想必 AMCDXVideoPatcherCLI 的具体实现能够从这里看出一些端倪        |
| [dryv](https://github.com/Stuff7/dryv)                                            | Rust     | No docs                                                                                                   |
| [bento4](https://github.com/axiomatic-systems/Bento4)                             | C++      | 最完善                                                                                                    |

### Inspection tools

- hexdump -vC
- [fq](https://github.com/wader/fq)
- [MP4Box.js](https://gpac.github.io/mp4box.js/test/filereader.html) - a file inspection tool
- [mp4analyser](https://github.com/essential61/mp4analyser)

### Notes

这么些处理 atom (包括 NCLC tag) 的实现，都是用 C++ 写的。其实 Rust 也能做这些 low-level 的事情，对吗？只是这门语言比较新，暂时还没有人用它来做这个事而已。那么我能不能来做呢？用 Rust 写一个 parse MOV file format 的实现。其实已经有了：[dryv](https://github.com/Stuff7/dryv)——这两天就在更新。

这需要对 low-level 底层的东西有足够的了解是吗？底层的东西我又不太会。但这无疑是一个机会和切入点，就像 Asahi Lina 在她的哪一个 stream 说的，她最开始入门 low-level 编程也是为了要 hack 任天堂的一个什么掌机。我需要写一个 parse QuickTime file 的 Rust 的实现，实现出来一定挺酷。

进一步的说，其他的一些元数据，比如时码、卷号，这些在 DIT 工作当中会碰到的需要对其做一些操作的元数据。现有的达芬奇、Pomfort 之流没有提供类似的功能，或者需要你进入到一整个流程当中，才能实现对某个文件修改元数据的需求。我可以做一个 goto 的锋利小工具。目前市面上有 [QTchange](https://www.videotoolshed.com/handcrafted-timecode-tools/qtchange/)，但它卖的挺贵，29.95 欧。上面提到的 [AMCDXVideoPatcher](https://mogurenko.com/) 的 GUI 好像连卷名也能修改。就真的好想知道具体是怎么办到的，可惜它没有公开源码，不过相比 QTchange 这个免费还要啥自行车。

---

我导出两个片段，一个 1-1-1，一个 1-2-1。除此之外其他一切都一样。最后使用 hexdump 查看两个文件的字节数 bytes 大小，发现相差 12 bytes，而 gama atom 恰好就是 12 bytes。差的就是这个 gama atom。

---

Decode ProRes: [RDD 36:2015 - SMPTE Registered Disclosure Doc - Apple ProRes Bitstream Syntax and Decoding Process](https://ieeexplore.ieee.org/document/7438722)。这个标准专门讲解了如何 decode ProRes 的 file stream。而且，不能只修改 mov 这个 container 的 colr atom，还要修改 ProRes header 里的 Primaries, Transfer Function and Matrix 信息：

> In addition to the colour information carried within the Color Atom, information regarding the transfer function, colour matrix and primaries are **also stored within the frame header information of the ProRes elementary stream**, alongside other parameters, such as frame rate, spatial resolution and chroma format. This header is repeated throughout the bitstream. Full details of the header layout can be found in the [SMPTE specification](http://ieeexplore.ieee.org/document/7438722/).

A lot of work to do...

---

这里有两个 colr_atom-ish 的地方，一个是 MOV file format 本身的 colr atom，一个是 ProRes 编码的每一帧的 header（ProRes header）：也是由 color primaries、transfer characteristic 和 matrix coefficients 组成。所以，如果想要实现修改 NCLC tag，不仅需要修改 colr atom（MOV file format 里的），还要修改每一帧的那三个东西。MOV file format 的 colr atom 三个 tag 各占 2 bytes（u16），ProRes frame 的三个 tag 每个只占 1 byte（u8）。

It looks like: 只有 Transfer characteristics 是 Unspecified 的状态，gama atom 才会起作用。如果 Transfer characteristics 是有值的，比如 BT.709，那么即使有 gama atom，比如使用 Mediainfo 查看 Gamma 为 2.4，也是不起作用的。ColorSync utility 仍然只会以 Transfer characteristics 为准，而忽略 gama atom。确实像之前听到别人所说，gama atom 像是 Apple 提供的一个后门，一个 hacking ColorSync 的迂回办法。

---

今天（2023-09-30）实现了 overwriting colr atom，search 的算法还是用的之前的，因为之前的算法虽然的 debug build 下非常慢，但只要换到有优化的 release build 之后就非常快了（其实也不算非常快，只能说还算可以接受）。所以就先用着，继续实现后面的功能。

gama atom 这个东西的 FourCC (four character code) 是 `67 61 6d 61`。具体的值紧随其后，比如 `00 02 66 66` — Gamma 2.4，`00 02 33 33` — Gamma 2.2。如果要修改 Gamma 值，直接 overwrite 这 4 个 byte 就行了。QuickTime File Format 一个 [2001 的 PDF 文档](https://developer.apple.com/standards/qtff-2001.pdf)也说明 gama atom 是个 32-bit 的 [fixed-point number](http://www.sunshine2k.de/articles/coding/fp/sunfp.html#ch3)，也就是占 4 个 byte。如果需要去掉 gama atom，那么将那 4 个 byte 用 `00 00 00 00` overwrite 掉即可。这样的话，当我们需要把 1-2-1 转换到 1-1-1 的时候，首先修改 atom，然后去掉 gama atom。去掉 gama atom 的方法算是找到了。但是问题是如何添加上 gama atom，这可能才是我们想要的。目前没找到合适的方法让 gama atom 无中生有，添加一个 gama atom 到整个 file stream。难道要 shift 所有其他 bytes？

---

做 byte search 的话，会 match 到 两个 gama atom 的 pattern。更奇怪的是，对于 1-1-1 的文件，它没有 gama atom，但也能 match 到 gama atom 的那个 pattern：

```sh
$ grep "gama" 1-1-1_20mins_hex.txt
05861500  17 c5 96 57 02 3c 67 61  6d 61 ff 71 4d 86 fc e4  |...W.<gama.qM...|
```

但需要文件的时长够长。比如只有几秒的，就只有一个正常的 gama atom。会不会出现两个 gama atom 的 pattern，取决于这个 prores 文件的时长多长，我测试过 5 分钟没有，10 分钟开始有，20 分钟也有。

| File name          | Numbers of gama  | Notes                               |
| ------------------ | ---------------- | ----------------------------------- |
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

```sh
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

修改 gama atom 值的功能实现了以后，我把一个 1-2-1 的 MOV 视频的 Gamma 值从 2.4 改成 1.96。然后将其与 1-1-1 的 MOV 视频对比，可以发现画面整个一模一样。

---

Assimilate SCRATCH 的 Rec.709 Gamma 2.4 render 的行为是正常的，在 Output Settings > Data Format 中选择：

- Color: Rec709
- EOTF: Gamma 2.4

这样渲染出来的 ProRes MOV 文件的 tagging 为 1-2-1 Gamma 2.4。

SCRATCH 中，你也可以选择把文件特别的 tag 成 Gamma 2.2 或者 2.6 都可以，有 2.2 和 2.6 的选项。对于 atom-modifier（我的 CLI 工具）来说，你可以用它把你 1-2-1 文件的 Gamma 设置成任何值（讲道理除了 2.4、2.2、2.6 你也不需要其他值了），甚至负数（别）。

---

AMCDXVideoPatcher 可以在原文件根本没有 gama atom 的情况下，添加一个 gama atom，然后设定相应的 Gamma 值（它的 GUI 好像有 bug，只能设定 1.8 的 Gamma 值，那个输入框无法输入其他值）。我对比了使用它添加 gama atom 之前和之后，文件的状态。发现：添加完 gama atom 之后，gama atom 之后的 atoms 的 offset 全部 shift 了 12 bytes。Maybe 确实像我前几天所说：

> 难道要 shift 所有其他 bytes？

---

[clean aperture (clap) - QuickTime File Format](https://developer.apple.com/documentation/quicktime-file-format/clean_aperture)

之前听说的一个问题，ARRI 的一些分辨率在 SCRACH 和 DaVinci Resolve 的分辨率显示不一样。SCRACH 比如说可能显示的是 2944x2160，DaVinci Resolve 显示的 as expected 为 2880x2160。症结在此：clean aperture (clap) — 一个 video sample description ("stsd") 的 extension。colr 和 gama atom 都属于这个 "stsd"。

---

colr atom, gama atom are located in...here!
### Sample table atom ("stbl"):

> The sample table atom contains all the time and data indexing of the media samples in a track. Using tables, it is possible to locate samples in time, determine their type, and determine their size, container, and offset into that container.

"stbl" 本身有两个属性：

- Size: 4 bytes
- Type: "stbl", 4 bytes

剩下就是很多其它的 child atoms 了，相当于都在 "stbl" 之下。其中就有 "stsd":

"stbl":
- "stsd": Sample description atom
- "stts": Time-to-sample atom
- "ctts": Composition offset atom
- "cslg": Composition shift least greatest atom
- "stss": Sync sample atom
- "stps": Partial sync sample atom
- "stsc": Sample-to-chunk atom
- "stsz": Sample size atom
- "stco": Chunk offset atom
- "sdtp": Sample dependency flags atom
- "stsh": Shadow sync atom

### Sample description atom ("stsd"):

"stsd" layout:

- Size: 4 bytes
- Type: "stsd", 4 bytes
- Version: 1 bytes
- Flags: 3 bytes
- Number of entries: 4 bytes
- **Sample description table**: variable

**Sample description table** 一般来说又由以下几个部分组成:

General structure of sample description:

- ... (continued from above bullets)
	- Sample description size
	- Data format
	- Reserved
	- Data reference index

但更常见的是除了这四个 field 之外还有其他 specific to the **media type** 的 field。比如有我们关心的 **Video** sample description ("stsd")，它有一些 additional 的 fields。

- ... (continued from above bullets)
	- Version
	- Revision level
	- Vendor
	- Temporal quality
	- Spatial quality
	- Width
	- Height
	- Horizontal resolution
	- Vertical resolution
	- Data size
	- Frame count
	- Compressor name
	- Depth
	- Color table ID

除此之外，还有一个叫做 Video sample description extension 的东西，如果它存在 present，那么上面的 bullet points 又会增加几个。这时 colr atom, gama atom 出现了：

- ... (continued from above bullets)
	- avcC
	- colr
	- gama
	- pasp
	- clap
	- ...

你会发现，这些其实是一些 atom（相当于 stbl 的 child atom? I guess.），而上面那些是一些类似 fields 的东西。没错，它们都在 Sample description tale 之下。这就是 colr atom, gama atom 的 location。

---

卷名 Reel number/name is located at: (Source reference!)

- stbl
	- stsd
		- **tmcd**

- [Timecode sample description](https://developer.apple.com/documentation/quicktime-file-format/timecode_sample_description)
	- ...
	- [Source reference](https://developer.apple.com/documentation/quicktime-file-format/timecode_sample_description/source_reference): A user data atom containing information about the source tape.

时码相关的信息也在这个 atom — "tmcd"。

---

How to create/insert a new atom? This article explains how to create and insert a new atom using the CoreMedia library in Swift:

[Create new atoms and insert them in a QT atom container.](https://developer.apple.com/documentation/quicktime-file-format/creating_new_atoms) — Apple Developer Documentation

---

"mdat":

我们经常会发现一到 mdat 这个 atom，parser 就会跳过这么多 byte。

```sh
mdat: s=    263696 (0x00040610), o=        28 (0x0000001c)
  ...skipped 263688 bytes
moov: s=      1544 (0x00000608), o=    263724 (0x0004062c)
```

> The most important part of an MPEG-4 file is the mdat atom - **its where the actual raw information for the file is stored**. — [atomic parsley](https://atomicparsley.sourceforge.net/mpeg-4files.html#:~:text=The%20most%20important%20part%20of%20an%20MPEG%2D4%20file%20is%20the%20mdat%20atom%20%2D%20its%20where%20the%20actual%20raw%20information%20for%20the%20file%20is%20stored.)
这是实际上文件的 raw data 的所在地。

ProRes 每一帧 的 frame size 都可以在 "stsz" 找到。

---

Movie atom:

> Only metadata is stored in a movie atom.

一个 MOV file 的所谓 metadata，全都储存在 movie atom ("moov") 里。

> Sample data for the movie, such as audio or video samples, are referenced in the movie atom, but are not contained in it. — [QuickTime File Format > Movie Atoms - Apple Developer Documentation](https://developer.apple.com/documentation/quicktime-file-format/movie_atoms)

QuickTime File Format 把 media file 实际的数据储存在一个个 Sample 里，Sample 进而又组成一个个 Chunk，实现更高效的数据 access。Sample 的 data 不实际储存在 movie atom 里，像上面说的，movie atom 只储存 **meta**data。

---

User data atoms — "udta":

可以把生成这个 mov file 的软件的名字包含进去，

| List entry type | Description                                                                     |
| --------------- | ------------------------------------------------------------------------------- |
| ...             | ...                                                                             |
| `'©swr'`        | Name and version number of the software (or hardware) that generated this movie |
| ...             | ...                                                                             |

比如：

```sh
          |                                               |                |        [3]{}: box
0x11a37b70|                                       00 00 00|             ...|          size: 60
0x11a37b80|3c                                             |<               |
0x11a37b80|   75 64 74 61                                 | udta           |          type: "udta" (User-data)
          |                                               |                |          boxes[0:1]:
          |                                               |                |            [0]{}: box
0x11a37b80|               00 00 00 34                     |     ...4       |              size: 52
0x11a37b80|                           a9 73 77 72         |         .swr   |              type: "©swr" (Encoder)
0x11a37b80|                                       00 28   |             .( |              length: 40
0x11a37b80|                                             55|               U|              language: "und"
0x11a37b90|c4                                             |.               |
0x11a37b90|   42 6c 61 63 6b 6d 61 67 69 63 20 44 65 73 69| Blackmagic Desi|              value: "Blackmagic Design DaVinci Resolve Studio"
0x11a37ba0|67 6e 20 44 61 56 69 6e 63 69 20 52 65 73 6f 6c|gn DaVinci Resol|
0x11a37bb0|76 65 20 53 74 75 64 69 6f                     |ve Studio       |
```

---

> Because the first field in any atom contains its size, including any contained atoms, it is easy to skip to the end of an unknown atom type and continue parsing the file. — [QuickTime File Format > QuickTime Movie File — Apple Developer Documentation](https://developer.apple.com/documentation/quicktime-file-format/quicktime_movie_files#:~:text=Because%20the%20first%20field%20in%20any%20atom%20contains%20its%20size%2C%20including%20any%20contained%20atoms%2C%20it%20is%20easy%20to%20skip%20to%20the%20end%20of%20an%20unknown%20atom%20type%20and%20continue%20parsing%20the%20file)

---

[alfg/mp4-rust: MP4 reader + writer library in Rust!](https://github.com/alfg/mp4-rust)

这个项目值得贡献一下。目前的状态还是比较初级，有很多 atom/box 都没支持。

说到底 [fq](https://github.dev/wader/fq) 这个用 Go 写的 parser 才是牛啊。[mp4-rust](https://github.com/alfg/mp4-rust) 不支持的 atom/box 我都是通过 fq parsing 出来发现的。目前我所知的 atom/box，fq 都是支持的。那么可以给 mp4-rust 贡献一下的点在于，给它添加我最需要的 colr atom 和 gama atom 的支持。这也是我最开始关注这个项目的原因。但现在发现还需要给它添加 parse ProRes 编码家族的支持，就像 Ian Jun 给它[添加 avc1 编码支持](https://github.com/alfg/mp4-rust/commit/0df82aec5f4ee636d1fbb71bb043430ed2d83005)一样（添加一个 avc1 box）。avc1 是 H.264 的 Codec ID，它之下有一个 avcc configuration atom/box。

说到 parser ISOBMFF，项目其实蛮多的。MediaInfo 也是其中之一，也是 canonical 的 implementation，是类似工具在实现效果上的参考，fq 也一样（这样来看 fq 这个工具写挺好的）。

---

这个 [pull request](https://github.com/axiomatic-systems/Bento4/pull/694)（目前还未被合并）给 [Bento4](https://github.com/axiomatic-systems/Bento4) 添加了 colr, gama, pasp, fiel atom 的支持。然后利用 Bento4 现有的工具，特别是这个 [mp4edit](https://github.com/axiomatic-systems/Bento4/tree/master/Source/C%2B%2B/Apps/Mp4Edit) 已经可以完全实现修改 colr atom 和添加 gama atom 了。到此为止，我最初的需求被这个工具解决了。

插入 gama atom：

```sh
./mp4edit \
	--insert \
	"moov/trak/mdia/minf/stbl/stsd/apcn":gama_atom_2.4.bin \
	input.mov \
	output.mov
```

修改 colr atom：

```sh
echo -n -e '\x00\x00\x00\x12colrnclc\x00\x09\x00\x10\x00\x09\x0a' > colr.bin

./mp4edit \
	--replace \
	"moov/trak/mdia/minf/stbl/stsd/apcn/colr":colr.bin \
	input.mov \
	output.mov
```

---

tmcd atom 包含卷名信息，给没有卷名的 MOV 添加卷名，使用：

```sh
./mp4edit \
    --replace \
    "moov/trak[1]/mdia/minf/stbl/stsd/tmcd":../atoms/tmcd_atom_addedReelNum.bin \
    ../test_footages/no_reel_number/1-1-1_10frames.mov \
    ../test_footages/no_reel_number/1-1-1_10frames_replacedTmcdAtom.mov
```

原理是替换掉 tmcd 这个 atom：使用带有卷名信息的 tmcd atom .bin 文件替换掉原始文件中的 tmcd atom。

目前卷名还是 hardcoded 在那个 bin 文件里，需要通过 programming 的方式实时生成相应的不同卷名的 .bin file。

所以到目前为止只是验证了使用 mp4edit 来修改、添加卷名的可行性。

---

Can I compile Rust code together with a binary C++ file?

1. Call mp4edit executable in Rust code.
2. Build a GUI using egui library.
