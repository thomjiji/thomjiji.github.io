---
title: "Roam & Zettelkasten"
date: 2023-01-27
draft: false
ShowToc: false
tags: ["Roam"]
---

## 一些随机的摘抄

> A zettelkasten consists of many individual notes with ideas and other short pieces of information that are taken down as they occur or are acquired. The notes are numbered hierarchically, so that new notes may be inserted at the appropriate place, and contain metadata to allow the note-taker to associate notes with each other. For example, notes may contain tags that describe key aspects of the note, and they may reference other notes.

1. Zettelasten 笔记法，每一个新的 node 的文件名会添加上当前的时间戳？（numbered hierarchically）就像 Org Roam 一样。
2. 每一个 node 会有 tag，就像我现在在 Obsidian 中尝试的一样。

---

> He argues that in order to internalize something we need to re-elaborate it, curate it. We should also be able to let go some of these resources because they are not so precious. Let's add action number 3 to the Zettelkasten method: improve and enrich your existing notes.

有点道理，就像我的 Notion 一样，虽然自己做了很多分类，建了很多具有层级结构的 page。但是有一些 page 还是开始变得混乱起来，以至于我都不太想再去管理。你记的东西，为了能够内化它们，你需要去 curate 和 re-elaborate。

---

## Personal wiki should not have a hierarchy

> notes should not have an hierarchy but structure should emerge spontaneously.

> When I want to add something to my wiki, I don't want to worry about where to place the information (is it personal? is it work? is it machine learning or reinforcement learning?). I capture the blog post, notes about a book, youtube video, podcast and I add metadata. Then, smart software automates the process of creating a network of notes and visualizing it.

> I just finished converting my structured wiki into an unstructured one and it works much better. Let's make an example. I had an org file called `workflow.org`, where I had collected information about different software I use. One heading for `emacs`, one heading for `python`, one heading for `docker`… One subheading for `emacs` is `org-mode`, with all the keybindings and useful functions.
> 
> Each heading (and potentially subheading) has been turned into a node (take a look at Hugo's Notes graph). It makes sense to have an `emacs` node, to which other nodes can refer to. For example `org-mode` is a node referring to it. The package `org-roam` I am about to describe is another node linking to both `emacs` and `org`.

也是先建立一个「中心化」的 node，在这个 node 里去索引其他与之关联的 node，像他在这里说的：在 `workflow.org` 这个文档里，每个标题和子标题将会被转为一个单独的 node，拥有一个属于它们自己的 page。自然而然地，在标题和子标题们就和 `workflow.org` 链接上了。但是是以 `workflow.org` 为中心，如果我们在 Graph View 里来看的话。

> Hierarchy is teared apart and structure emerges spontaneously. Whenever I want to add a new note about `org`, I will just add a new node and refer to the `org` node, I don't need to care about which folder or file to write it in.

## Reference

[Blog: Discovering org roam](https://www.lucacambiaghi.com/posts/discovering-org-roam.html)
