---
title: "I Fixed Rime and Neovide"
date: 2025-05-04
draft: true
tags: ["Rime", "Neovide", "Neovim"]
---

# TL;DR

**问题**：Rime 的自动切换到英文输入法的设置对 macOS 上的 Neovide 不生效！

**原因**：我一般是从 Terminal 启动 Neovide，这样其实调用的是 Homebrew 叫作 formula 的那个程序：`/opt/homebrew/bin/neovide`。而我们在 Rime 的配置文件 `squirrel.yaml` 设置的开启默认英文 `ascii_mode: true` 是给 Neovide 的 GUI app 的！那它自然就不生效了。

**解决方案**：给 Neovide 设置一个 alias，让它指向 Neovide GUI app 的可执行文件 `/Applications/Neovide.app/Contents/MacOS/neovide`。像我在 Fish shell 的 `~/.config/fish/config.fish` 就是这样设置:

```shell
alias neovide "/Applications/Neovide.app/Contents/MacOS/neovide"
```

这样你在 Terminal 输入 `neovide` 就不会调用那个 formula `/opt/homebrew/bin/neovide`，而是 Neovide GUI app 的可执行文件。Rime 的默认英文的设置也就正常生效了。

# 前情提要、问题浮现

Neovide 是一个我前阵子发现的宝藏 app，它让 neovim 拥有 vscode 一般的顺滑操控和鼠标动画。我第一次知道这个 app 的那天晚上，下班一回到家，等不及迅速安装配置好了，然后就被惊到。这个动画的顺畅程度跟 vscode 真的有过之而无不及。那天晚上真是惊叹不已，要知道 neovim 在 Terminal 里的体验你是绝对不会奢求它有多好的 animation 的。即便 LazyVim 的 snacks 插件有通过一个 Animate 的模块做了一点点让 neovim 稍微顺滑一点的努力，但远不及 Neovide 这个顺滑。Neovide 属于你用了就回不去了的那种。

以前我记得还用 PyCharm 的时候，有一个帖子就在苦苦哀求 PyCharm 添加像 vscode 一样的顺滑的鼠标 animation，然而直到今天好像都没有实现。neovim 这样一个 text based 的 TUI app 通过 neovide 居然能做到像 vscode 这样图形界面 based 的 app 一样的顺滑程度。

但是！一个我们中文输入法用户很难避开的问题也来到了 Neovide 这里。我是使用 Rime 这个输入法引擎的，平时切换中英输入法就是按一下 Shift 键。Rime 有一个功能就是：你可以设置哪些 app 启动的时候是默认自动切换到英文输入法。像比如你打开 Terminal 肯定是输入一些英文的命令，那么你就设置让 Terminal 打开的时候 Rime 自动给我切换到英文输入法，这样我就不用手动去按一下 Shift 键。

在 `squirrel.yaml` 文件里有一个 `app_options`：

```yaml
app_options:
  com.googlecode.iterm2:
    ascii_mode: true # 开启默认英文
    vim_mode: true   # vim 模式下退出 insert mode 自然切换回英文
  com.neovide.neovide:
    ascii_mode: true
    vim_mode: true
```

但是这个设置在 Neovide 怎么着都不生效！打开 Neovide 的时候还是不会自动切换到英文，一开始 hjkl 就会触发中文输入法。这个需要你手动切换一下到英文输入法的操作，其实是很打断的。你火急火燎之下需要用 neovide 打开一个文件快速编辑一下，neovide 一眨眼就打开了。你想快速 navigate 到那个文件，但 hjkl 没有让你快速到达那个你想要编辑的文件，而是把输入法弄出来了。你真想一拳把输入法凑开。

# 初见端倪

本来都是把这个问题当作无解搁置了，不能自动切换英文就不能吧，就这样吧。maybe 是 Neovide 的什么 bug，这个软件没法做到那么完美也没办法。

但那天晚上我在解决一个 neovim 的 autoformat 配置问题的时候，发现是由于自己把配置没放对地方导致 conform.lua 并没有生效，进而才导致 autoformat 坏掉了。发现是这么个可笑的原因，然后浪费了晚上好几个小时。

解决了这个可笑的配置问题之后，Neovide 和 Rime 这个搭配的问题又浮上来了。你其实没法忽略它，一开启 Neovide，一 hjkl 就会触发中文输入法真的很恼人。Neovide 什么都好，对我来说就这点很致命，是我没法安心的用下去的主要原因。然后又开始尝试解决。

先是搜索，发现其实大家也有这个问题，像这个 Rime 的 [Issue](https://github.com/rime/home/issues/1537)，但它好像在 Windows。这个 Rime 的 [Discussion](https://github.com/rime/home/discussions/1283) 也提到类似问题。

又去检视了一下我的 `squirrel.yaml` 配置：

```yaml
# ascii_mode、inline、no_inline、vim_mode 等等设定
# 可参考 /Library/Input Methods/Squirrel.app/Contents/SharedSupport/squirrel.yaml
app_options:
  com.neovide.neovide:
    ascii_mode: true # 开启默认英文
    vim_mode: true   # vim 模式下退出 insert mode 自然切换回英文
  com.apple.Spotlight:
    ascii_mode: true
  com.microsoft.VSCode:
    ascii_mode: true
    vim_mode: true
  com.googlecode.iterm2:
    ascii_mode: true
    vim_mode: true
  com.jetbrains.pycharm:
    ascii_mode: true
  com.blackmagic-design.DaVinciResolve:
    ascii_mode: true
  com.apple.finder:
    ascii_mode: true
  org.mozilla.firefox:
    ascii_mode: false
  dev.warp.Warp-Stable:
    ascii_mode: true
  net.freemacsoft.AppCleaner:
    ascii_mode: true
  org.gnu.Emacs:
    ascii_mode: true
  com.jetbrains.CLion:
    ascii_mode: true
  com.apple.Terminal:
    ascii_mode: true
```

没啥问题啊，ascii_mode 设为 true。之前也尝试把它设为 false 试了下，以防我把这个 ascii_mode 是用来干啥的记错了。那这个 `com.neovide.neovide`、`com.googlecode.iterm2` 这些奇怪的 `com...` 究竟是个什么呢？其实我不是很清楚，只知道你可以去 `/Application/<app_name>.app/Contents/Info.plist` 这个文件里找到 `CFBundleIdentifier` 这个 xml tag，这个 tag 里面的内容就可以写在 `squirrel.yaml` 的 `app_options` 作为 app 的名字，让 Rime 正确找到 app 然后做正确的事——自动切换到英文。

那对于 Neovide 来说，`/Application/Neovide.app/Contents/Info.plist` 里的 `CFBundleIdentifier` 确实也是叫这个：`com.neovide.neovide`。难不成 Neovide 打包 Mac app 的时候没搞对？Neovide 的什么 bug？我把 `com.neovide.neovide` 改成 `com.Neovide.neovide`...还是没用。

这时不知道为啥我直接打开 Spotlight 启动了 Neovide 的 GUI app，我发现居然没有触发中文输入法？！这也就是说 Rime 做了正确的事，自动切换到了英文。同时也能看到启动完成的一刹那，Rime 的输入框框显示切换到了英文。我瞬间知道为什么了。赶紧去 Terminal：

```fish
$ which neovide
/opt/homebrew/bin/neovide
```

shit!
