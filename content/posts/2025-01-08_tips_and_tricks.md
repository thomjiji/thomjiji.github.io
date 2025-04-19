---
title: "Tips and tricks"
date: 2025-01-08
draft: false
tags: ["Firefox", "LazyVim", "Karabiner", "DaVinci Resolve"]
---

Firefox 的 [Profile](https://kb.mozillazine.org/Profile_folder_-_Firefox) 文件夹可以在这里获取：Firefox 左上角菜单栏 > Help > More Troubleshooting Information > Application Basics > Profile Folder.

在这个 Profile folder 里创建一个叫做 `user.js` 的文件，就可以实现导出 Firefox 的全局用户设置（修改过的 Firefox 的默认设置）。

所有在 `about:config` 里修改的设置都在一个叫做 `prefs.js` 的文件里。=> [Prefs.js file](https://kb.mozillazine.org/Prefs.js_file)

我的 Firefox 用户设置（`about:config`）：

- `mousewheel.with_control.action: 3`

Reference:

- <https://support.mozilla.org/en-US/questions/1197798>

---

Change author name in commit message:

- `git config --global user.name <name>`
- `git config --global user.email <email>`

---

安装 LazyVim 之后，LSP 的下载会遇到一些网络问题，比如每次都是 pyright 由于一些网络原因安装失败：`set -Ux https_proxy 127.0.0.1:7890` 和 `set -Ux http_proxy 127.0.0.1:7890` 都不好使，使用 sing-box 也不行。这时只需要取消系统代理就可以成功联网安装。

---

快速配置 karabiner。把一个 karabiner 的一个配置文件拷过来就行：

```
scp -r <user>@<ip_addr>:'/Users/<user>/.config/karabiner/karabiner.json' /Users/<user>/.config/karabiner/karabiner.json
```

得到两个 profile：

- [ON] Caps Lock => Ctrl
- [OFF] Caps Lock => Ctrl

搞定。

---

达芬奇的 Gallery 默认储存在 `/Users/<user>/Movies/.gallery` 这个路径。每抓取一个静帧，达芬奇都会在这个路径下对应的项目 ID 文件夹（例如 `e780fcb4-d9ad-4be3-ae40-395131ed136c/`）里生成一个 dpx 文件。当从达芬奇删除静帧时，这个路径下对应的 dpx 文件并不会被删除。所以随着项目的日积月累，`/Users/<user>/Movies/.gallery` 文件夹就会越来越大。

达芬奇用来储存项目备份（Resolve Project Backups）的路径下有很多个文件夹，这些文件夹都是用一个叫作“项目 ID”的字符串来命名的。在每个项目 ID 文件夹之下并没有这个项目 ID 对应的究竟是哪个项目的信息。而在 Gallery `/Users/<user>/Movies/.gallery` 路径下的项目 ID 文件夹里，有一个 Info.txt 的文件，里面会告诉我们这个文件夹属于哪个项目：

```
Database Name: Local Database
User Name: guest
Project Name: <project name>
```

又引出另一个发现：我们创建的 powergrade 是存在哪一个文件夹呢？首先 powergrade 也是存在 `/Users/<user>/Movies/.gallery` 这个路径，具体是这个路径下的哪个项目 ID 的文件夹，只要哪个文件夹下的 Info.txt 文件里没有 `Project Name: 802301 Color Balance` 这一行，那么这个文件夹就是用来存 powergrade 的文件夹。