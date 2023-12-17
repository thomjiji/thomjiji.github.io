---
title: "Use mouse wheel to scroll less in iTerm2"
date: 2023-12-17
draft: false
tags: ["iTerm2"]
slug: use-mouse-wheel-to-scroll-less-in-iterm2
---

在 iTerm2 中使用 less command 有一个奇怪的点：你不能用鼠标来滚动页面。只能使用 Vim 的 keybinding `Ctrl+d` 和 `Ctrl+u` 来上下翻页。而且当你比如在查看一个 command 的 man page 时，因为 man page 默认是使用 less 来打开的，你查看完某个 man page 按 `q` 退出，然后使用鼠标滚轮往上面滚一下，滚到上面一屏，你会发现你的 Terminal 变得很乱，有很多刚刚你查看的那个 man page 的残留。

我的需求总结来说就是：

1. 能够在 less 里使用鼠标滚轮来翻页，滚动。
2. 退出 less 之后不要有残留。

其实，这里有一个概念叫作 alternate screen。你用 less 打开查看一个什么文件的时候进入的就是一个 alternative sceen，按 `q` 退出就是退出那个 alternate screen，回到这个 main screen（这个是我瞎取的）。

iTerm2 里也有这方面的设置，设置一下就行了。

首先这里有一个设置在 Session > Terminal State > Alternate Screen。这个设置我暂时没搞懂具体效果是什么，而真正起作用的也不是这个设置，只是它有点相关。

然后最重要的是这个设置，需要打开：**Preferences > Advanced > Mouse > Scroll wheel sends arrow keys when in alternate screen mode**，把它改成 Yes。

这时你会发现可以正常在 less 里用鼠标滚轮滚动了。

但是退出 less 仍然会看到有残留，这时去：**Profiles > 选中你当前的 profile > Terminal > Scrollback Buffer > Save lines to scrollback in alternate screen mode**，把它取消勾选。

完成。

---

Reference:

- https://stackoverflow.com/questions/14437979/iterm2-scroll-less-output-with-mouse
