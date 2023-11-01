---
title: "File size"
date: 2023-11-01
draft: true
---

## Reference

- [Binary and Decimal Prefixes - Articles](http://wolfprojects.altervista.org/articles/binary-and-decimal-prefixes/#:~:text=The%20decimal%20prefixes%20are%3A%20k,is%20used%20to%20indicate%20bytes.)
- [Binary prefix](https://en.wikipedia.org/wiki/Binary_prefix#Comparison_of_binary_and_decimal_prefixes)
- [Decimal and Binary Prefixes – Blocks and Files](https://blocksandfiles.com/2022/04/23/decimal-and-binary-prefixes/)
- [du invocation (GNU Coreutils 9.4)](https://www.gnu.org/software/coreutils/manual/html_node/du-invocation.html)

## Notes

`du *` 输出的其实不是文件大小，而是所谓 block size。

```sh
$ du *
5568	1-2-1_2frames_ap4h.mov
8040	1-2-1_2frames_ap4x.mov
3928	1-2-1_2frames_apch.mov
2152	1-2-1_2frames_apcn.mov
688	    1-2-1_2frames_apco.mov
1592	1-2-1_2frames_apcs.mov
```

我们使用 exa 这个 ls 的替代品，用 `exa -lh -b -S` 这个命令得到这样的输出：

```sh
$ exa -lh -b -S
Permissions  Size Blocks User Date Modified Name
.rw-r--r--@ 2.7Mi   5568 thom 27 Oct 17:32  1-2-1_2frames_ap4h.mov
.rw-r--r--@ 3.9Mi   8040 thom 27 Oct 17:32  1-2-1_2frames_ap4x.mov
.rw-r--r--@ 1.9Mi   3928 thom 27 Oct 17:32  1-2-1_2frames_apch.mov
.rw-r--r--@ 1.0Mi   2152 thom 27 Oct 17:32  1-2-1_2frames_apcn.mov
.rw-r--r--@ 342Ki    688 thom 27 Oct 17:32  1-2-1_2frames_apco.mov
.rw-r--r--@ 793Ki   1592 thom 27 Oct 17:32  1-2-1_2frames_apcs.mov
```

- `-l`: Display extended file metadata as a table.
- `-h`: Add a header row to each column.
- `-b`: List file sizes with binary prefixes. `--binary`.
- `-S`: List each file’s number of file system blocks.

可以看到 Blocks 那一栏跟 `du *` 的输出是一样的。`-S` 的 flag 的作用也正是列出 file system blocks。

---

要使用 `du -k *` 才勉强接近 Finder 显示出的文件大小（但仍然差很远，请接着看下去）。

使用 `du -k *` 会得到以下的输出：

```sh
$ du -k *
2784	1-2-1_2frames_ap4h.mov
4020	1-2-1_2frames_ap4x.mov
1964	1-2-1_2frames_apch.mov
1076	1-2-1_2frames_apcn.mov
344	    1-2-1_2frames_apco.mov
796	    1-2-1_2frames_apcs.mov
```

- `-k`: Display block counts in 1024-byte (1 kiB) blocks.

首先这里输出的数字是 KiB，不是 bytes。

你需要把它们乘以 1024，比如：2784 * 1024 = 2850816，才得到 bytes 也就是字节数。然后使用 Finder 查看相同文件得到字节数是 2849756，很接近了。但是 2850816 - 2849756 = 1060，仍然相差了 1060，这又是怎么回事呢？

---

`exa -lh -B -S` 得到与 Finder 匹配的字节数 bytes，其中 `-B` 的意思是 List file sizes in bytes, without any prefixes：

`exa -lh -b -S` 得到二进制下的 MiB：`2.8Mi`。

`exa -lh -S` 不 pass `-b` 或 `-B` 的 flag 得到与 Finder 匹配的 MB：`2.8M`。但有可能由于四舍五入位数的不同而导致比如 Finder 显示 64.6MB，这里显示 65M。

> - `exa -lh -b -S`: binary prefix
> - `exa -lh -B -S`: bytes
> - `exa -l`: exa default to decimal prefix with this command.

---

du 输出的是 block size。什么是 block size 呢，就是文件系统 file system 实际分配给这个文件的空间。而其他命令比如说 `exa -lh -b -S` 输出的结果是所谓 apparent size（表面上的大小），就是这个文件从表面上看应该是这么大（从表面上看应该占据了这么多的空间）。也就是说，一个文件的 metadata 里显示的这个文件的大小（表面上的大小 apparent size），“我这个文件要占据这么些空间”，与实际上文件系统分配 allocate 给它的空间可以是不同的，是两码事。为什么呢？因为有一种文件叫做 sparse file 的存在。就是你说你要占据这么多的空间，但实际上你拿这么多空间来存储的都是 0，使用 `dd if=/dev/null of=big bs=1 count=0 seek=2G` 命令可以生成一个 apparent size 是 2GiB，实际上文件系统分配给它的空间是 0 的文件。因为这个 command 实际上没有写入任何数据到那个文件当中去。

---

macOS 自带的 du 命令的行为实在是太奇怪了，要么 flags 跟 GNU 的 du 不一样，非得自己改一下，改个别的名，比如 GNU du 要显示 apparent size 的话，pass `--apparent-size` flag：`du --apparent-size --block-size 1 *`；macOS du 要显示 apparent size，pass `-A` flag。但最关键的来了，这个 `-A` 它不 work！

对于 macOS du 来说，`du -A -B 1 *` 对应着 GNU du 的 `du --apparent-size --block-size 1 *`。是吧，就功能上来说，`-A` 对应 `--apparent-size`，`-B` 对应 `--block-size`。这边 GNU du 的 `du --apparent-size --block-size 1 *` 输出结果为：

```
64606136	A100_001_20231023.mov
118636627	A100_002_20231023.mov
99361921	A100_003_20231023.mov
```

但 macOS du 的 `du -A -B 1 *` 输出结果为：

```
126184	A100_001_20231023.mov
231713	A100_002_20231023.mov
194067	A100_003_20231023.mov
```

它输出的仍然是 disk usage 而不是 apparent size，而且输出的这个数字还是 block size 为 512 bytes 下的 block count，而不是我 pass 给它的 1，要求它输出 block size 为 1 bytes 下的 block count，也就是所有的 bytes（bytes 就是它二者相乘得到的：block size * block count = total bytes）。相当于这两个 flag 都不 work。

认为 GNU du 输出的那三个数字是对的的原因，是与其他命令交叉比对得到的结果：`diskus -b A100_002_20231023.mov`, `exa -lh -B -S`。

所以咱直接不用 macOS 自带的 du 了，可以 [brew install coreutils](https://apple.stackexchange.com/a/69332)，里面就有纯正的原生的 du，告别 macOS 魔改过的 du。

---

blocks (the number of block) * block size (the size of each block, for example 512 bytes) = total bytes

---

到最后，记住这几个 command 就可以了：

- `du --si *`: `--si` like `-h` (human readable), but use powers of 1000 not 1024
- `du -b *`: `-b` equivalent to `--apparent-size --block-size=1`.
- `exa -lh -B -S`:
	- `-l`: Display extended file metadata as a table.
	- `-h`: Add a header row to each column.
	- `-B`: List file sizes in bytes, without any prefixes.
	- `-S`: List each file’s number of file system blocks. Use block size 512 bytes actually.
 - `diskus`:
	 - `diskus -b --size-format binary NINJAV_S001_S001_T001.MOV`
	 - `diskus -b --size-format decimal NINJAV_S001_S001_T001.MOV`

查看所在的盘的文件系统的 block size：`stat -f %k .`
