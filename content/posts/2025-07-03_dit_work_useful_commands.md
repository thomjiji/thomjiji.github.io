---
title: "DIT work useful commands"
date: 2025-07-03
tags: ["DIT"]
draft: false
---

## 注意事项

### 安全

运行终端命令的时候，你必须清楚的知道**你在做什么**。不要盲目地把一串命令复制粘贴过来，一敲回车就直接运行了。

如何知道一串终端命令究竟在做什么？大多数终端的命令和程序都有一个叫作 man page 的东西，它是这个命令、程序的 manual 手册。一般来说你可以在里面查阅到关于运行这个命令的所有信息，以及各种自定义选项。只需要在你要运行的命令前加上 `man`，比如 `man find`，`man gdu`。

-   always use `rm -i`
-   或者将 `rm` 命令用 trash-cli 替换掉：在需要使用 `rm` 命令的时候，通通换成 `trash` 命令。macOS 自带 `trash` 命令（`man trash`），其他平台需要[安装 trash-cli](https://github.com/andreafrancia/trash-cli)。
-   使用 `find ... -delete` 执行删除操作前，always 先 pipe 给 less 命令来查看将要删除哪些东西

## du

macOS 自带的 du 命令行为比较奇怪，我们使用 GNU 版本的 gdu

```bash
brew install coreutils
```

这将会下载安装 `gdu` 命令到本机。

### 如何知道一个文件有多大

```bash
gdu -sbc --si /path/to/dir
```

-   `-s`, `--summarize`: display only a total for each argument.
-   `-b`, `--bytes`: equivalent to '--apparent-size --block-size=1'.
-   `-c`, `--total`: produce a grand total.
-   `--si`: like `-h`, but use powers of 1000 not 1024.

以上命令会以 MB、GB 的形式输出文件大小，如果想得到更加明细的字节数，则去掉 `--si`：

```bash
gdu -sbc /path/to/dir
```

这样输出的字节数与 Finder `Cmd+i` 出来的字节数是匹配的。

#### 实例

```bash
$ gdu -sbc --si '/Volumes/DIT-4T#16/<project>/素材/'
395G    241009
184G    241020
853G    241022
137G    241027
226G    241111
70G     241119
641G    241129
116G    241216
735G    241217
435G    241219
709G    241220
1.7T    241221
96G     241226
6.3T    total

$ gdu -sbc '/Volumes/DIT-4T#16/<project>/素材/'
394135765498    241009
183963861229    241020
852788569255    241022
136063767206    241027
225994040381    241111
69765056933     241119
640764426683    241129
115596465862    241216
734707008434    241217
434676339401    241219
708259543121    241220
1656050830994   241221
95502148123     241226
6248267823120   total
```

## find

### 找出所有 macOS 生成的以 . 开头的文件

```bash
find . -type f -iname ".*" | less   # 找出所有的 .DS_Store
find . -type f -iname "._*" | less  # 找出所有的以 ._ 开头的文件
```

-   `.` 表示当前路径。
-   `-iname` 表示“case insensitive”，比如 DIT 和 dit 都会被 match。
-   `-type f` 表示仅匹配“文件”（file），不匹配“文件夹”（directory），比如有一个文件叫作 source，有一个文件夹也叫作 source，那么 find 只会匹配到前者。
-   `|`: pipe，管道，表示将 `|` 前面的命令的输出（output）用作给 `|` 后面命令的输入（input）。
-   `less`: 另一个常用的 cli 工具。用做“display the contents of a file in a terminal”。我们将 find 命令的输出传递给 less 的好处是：不会一下子输出一大堆东西把你的终端“刷屏”了，而是把 find 命令的输出给到 less 这个“文件内容查看器”。当你按 `q` 退出 less 的时候，你的终端还是干净的，不会被 find 命令输出的一大堆东西洗刷掉。在 less 中可以用快捷键 `Ctrl+u` 和 `Ctrl+d` 来翻页，按 `/` 然后输入你想搜索的词来搜索，比如输出 `/error` 它会高亮出所有的 error，按 n 和 p 可以跳到下一个和前一个 error。

### 找出所有 MP4 文件并且知道它们分别多大，以及它们加起来多大

```bash
find . -type f -iname '*.mp4' -exec gdu -sbc {} + | sort -h
```

或者直接使用 `gdu` 命令，加上一点正则表达式也可以得到同样结果：

```bash
gdu -sbc --si **/*{MP4,mp4} | sort -h
```

## tree

### 跨文件夹检查文件名连续性

```bash
tree -P '*MP4' /path/to/dir
```

通过将路径指向比如 CAM 文件夹：CAM#1，`tree` 命令会遍历该文件夹之下所有文件，筛选出 MP4 后缀的文件将它们放在一个清晰的树状层级结构里输出出来。

#### 实例

```bash
tree -P '*MP4' /Volumes/DIT-4T#20/<project>/素材/FX3#1/
FX3#1/
├── CARD#A
│   ├── AVF_INFO
│   ├── M4ROOT
│   │   ├── CLIP
│   │   │   ├── 250628_TT_10001.MP4
│   │   │   ├── 250628_TT_10002.MP4
│   │   │   ├── 250628_TT_10003.MP4
│   │   │   ├── 250628_TT_10008.MP4
│   │   │   ├── 250628_TT_10009.MP4
│   │   │   ├── 250628_TT_10010.MP4
│   │   │   ├── 250628_TT_10012.MP4
│   │   │   ├── 250628_TT_10014.MP4
│   │   │   ├── 250628_TT_10015.MP4
│   │   │   ├── 250628_TT_10016.MP4
│   │   │   ├── 250628_TT_10017.MP4
│   │   │   ├── 250628_TT_10019.MP4
```

## xxhash

### 对当前路径下所有 MP4 文件批量生成校验值

```bash
xxh64sum **/*{MP4,mp4}
```

可以快速的对比两个双备盘中某张卡里素材的完整性。有时候在 offload 的过程中可能出现一些问题，导致正常的 offload 流程没法完成，比如卡读取错误，我们只能用最原始的方式直接 Finder 将读取失败的某一条手动拷贝到两个盘。这种非标准 offload、双备流程下，我们需要更严格的检验方式。校验值是唯一的金律。

#### 实例

```bash
$ cd '/Volumes/GSJ#543/<project>/素材/<shooting_day>'
$ xxh64sum **/*{MP4,mp4}
9d6d75135f9cd3e9  FX3#1/CARD#A/M4ROOT/CLIP/250628_TT_10001.MP4
68516a302147d2be  FX3#1/CARD#A/M4ROOT/CLIP/250628_TT_10002.MP4
287b115c97ab2238  FX3#1/CARD#A/M4ROOT/CLIP/250628_TT_10003.MP4
0f8dabb33462624f  FX3#1/CARD#A/M4ROOT/CLIP/250628_TT_10008.MP4
6489583ee962da7d  FX3#1/CARD#A/M4ROOT/CLIP/250628_TT_10009.MP4
c12f36991bbe43f9  FX3#1/CARD#A/M4ROOT/CLIP/250628_TT_10010.MP4
6553bb0e0434e264  FX3#1/CARD#A/M4ROOT/CLIP/250628_TT_10012.MP4
```

## ls -lO, ls -l@, GetFileInfo, xattr

```bash
ls -lO # show file flags (BSD-style)
ls -l@ # show extended attributes
xattr # inspect extended attributes in detail
xattr filename
xattr -l filename
```
