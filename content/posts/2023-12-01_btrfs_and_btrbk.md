---
title: "btrfs and btrbk"
date: 2023-12-01
draft: false
tags: ["btrfs", "btrbk", "snapshot", "backup"]
slug: btrfs-and-btrbk
---

`/etc/btrbk/btrbk.conf` config file:

```
volume /
  snapshot_dir btrbk_snapshots
  subvolume home
  incremental strict
```

Key points:

- `volume /`: "Base path within a btrfs filesystem containing the subvolumes to be backuped (usually the mount-point of a btrfs filesystem mounted with `subvolid=5` option)." In this case, we use the existing root directory — which is already a btrfs filesystem (by default on Fedora). btrbk refer this `/` (next to `volume`) as "\<volume-directory\>".
- `snapshot_dir btrbk_snapshots`: `btrbk_snapshots` is relative to the `volume` set above, which in this case the `/` (root directory). So the full path of `btrbk_snapshots` will be `/btrbk_snapshots`, which located just under the root directory. It's like the "destination" of snapshots.
- `subvolume home`: `home` is also relative to `/` which set above. It's the "source" of snapshots. There are also a `target` field can be set: `target /mnt/btrbk_backup`, but it's optional. The `target` is where the "backup" subvolumes to be created (refer to the man page of `btrbk.conf` via `man btrbk.conf`), while the `snapshot_dir` is where "snapshot" to be stored.
- `incremental strict`: which means "non-incremental (initial) backups are never created" (`man btrbk.conf`).

---

What is snapshot?

> Every Btrfs snapshot is a subvolume. However, not every subvolume is a snapshot. The difference is in what the subvolume contains. A snapshot is a subvolume with added content: it holds references to current and/or past versions of files (inodes). — [Working with Btrfs – Snapshots | Snapshots in Btrfs](https://fedoramagazine.org/working-with-btrfs-snapshots/)

---

似乎，btrbk 说的 snapshot 指的是与原系统 filesystem 存储在一起的 snapshot，而 backup 指的是在另一个独立的 filesystem 下的 snapshot。与原 filesystem 存在一起的 snapshot **不能**称作是一个 backup，因为如果原 filesystem 炸了，那么 snapshot 一起完蛋。而 backup 这时候可以发挥作用了，因为它是存在另一个 filesystem 下的，一般来说是存在另一个盘上，这样才是 backup。以下是 btrbk man page 对 btrfs snapshot 和 btrbk snapshot 区别的简短说明：

> Note that in btrfs terminology, a snapshot is a “subvolume with a given initial content of the original subvolume” (showing a parent-uuid, see btrfs-subvolume(8)), and they can be read-write (default) or read-only. In btrbk terminology, snapshot means “read-only btrfs snapshot”, and backup means “read-only subvolume created with send/receive” (showing a received-uuid). — `man btrbk`

可以得知，btrbk 创建的 snapshot 默认是 readonly 的，btrfs 默认是 read 和 write。而 btrbk 的 backup 的概念为：使用 btrfs send 和 btrfs receive 命令创建的 readonly 的 snapshot （sumvolume），而 send/receive 命令一般来说用于在不同 btrfs 文件系统之间移动 snapshots，可以说就是在不同盘之间移动数据。因此，我们上面的理解与 btrbk 的说明相符。

btrbk.conf 的 target 就是 backup 存放的地方，而 snapshot_dir 顾名思义是 snapshot 存放的地方。

---

References:

- [Working with Btrfs – General Concepts — Fedora Magazine](https://fedoramagazine.org/working-with-btrfs-general-concepts/)
- [Working with Btrfs – Subvolumes — Fedora Magazine](https://fedoramagazine.org/working-with-btrfs-subvolumes/)
- [Working with Btrfs – Snapshots — Fedora Magazine](https://fedoramagazine.org/working-with-btrfs-snapshots/)
- [Working with Btrfs – Compression — Fedora Magazine](https://fedoramagazine.org/working-with-btrfs-compression/)
- [Incremental backups with Btrfs snapshots — Fedora Magazine](https://fedoramagazine.org/btrfs-snapshots-backup-incremental/)
- [Recover your files from Btrfs snapshots — Fedora Magazine](https://fedoramagazine.org/recover-your-files-from-btrfs-snapshots/)
