---
title: "gpg-agent doesn't work anyway!"
date: 2023-12-02
draft: true
tags: ["GPG", "SSH"]
slug: gpg-agent-does-not-work-anyway
summary: "Solved by this command: gpg-connect-agent updatestartuptty /bye >/dev/null"
---

### TL;TR

Solved by this command:

```sh
gpg-connect-agent updatestartuptty /bye >/dev/null
```

https://superuser.com/q/1631020/1865466

### 事发经过

Damn! 就在刚刚解决了一个实在令人头疼的问题。MBP 的 SSH 突然坏了，一直报这个错：

```
sign_and_send_pubkey: signing failed for ED25519 "(none)" from agent: agent refused operation
```

因为我用的是 GPG key 来做 SSH 的 authentication，第一时间就是重启 gpg-agent：

```sh
gpgconf --kill gpg-agent
gpgconf --launch gpg-agent
```

通常来说这样一下就又好了，之前好几次我都是这样解决的。但这次完全就没用。它这里说的“agent refused operation”，这个 agent 应该指的是 gpg-agent，所以第一时间想到重启它。但没用的话，又找是不是 ssh-agent 的问题。按道理来说我用 gpg key 来做 ssh authentication 的话，跟 ssh-agent 没啥关系。事实也是，我回到用 ssh key 来 auth 的话就没问题。输入 ssh key 的 passphase 可以正常连接。一通排查，问题就感觉出在 gpg-agent 身上。但什么蛛丝马迹也没有。gpg-agent 也都正常运行着。通过 `gpg-connect-agent reloadagent /bye` 和 `ps aux | grep gpg-agent` 检查了，然后又 kill 了几遍 gpg-agent，还是没用。

一时间都有点怀疑人生，这几天学 SSH，补习 GPG，是不是还是对它们的工作原理存在一些什么误解，哪个步骤没设置对。在出问题之前，其实我还是在看关于 `.ssh`、`.gnupg` 文件夹自身的权限问题，应该把它们设置成什么权限，总的来说就是：

```sh
chmod 700 .ssh
chmod 600 .ssh/*
```

`.gnupg` directory 按道理[也一样](https://wiki.archlinux.org/title/GnuPG#Home_directory)。

我回想起里，在第一案发时间，我是在改动 `.gnupg` 的权限：我把这个文件夹里面的所有内容也都改成了 600 权限，根据 Arch wiki 的推荐。但我后来看这个文件夹默认不是这样的，然后就照着我另一台 Fedora Asahi Remix 上的 `.gnupg` 文件夹又改回去了。主要是那几个 ssh-agent socket 文件本来是具有执行权限的。于是我又把它们的执行权限加了回去。按道理来说这样啥也没变。我又开始怀疑这个改动是不是冥冥之中改了点什么其他东西。这就又摸不着头脑...

最后合理怀疑是 TTY 呼不出来了，呼不出来就没法输入 passphrase 来验证，gpg-agent 验证不了，拿不到解密后的 private key 就只能报错了。

---

`man gpg-agent`:

> Note: in case the gpg-agent receives a signature request, the user might need to be prompted for a passphrase, which is necessary for decrypting the stored key.  Since the **ssh-agent protocol does not contain a mechanism for telling the agent on which display/terminal it is running, gpg-agent's ssh-support will use the TTY or X display where gpg-agent has been started.**  To switch this display to the current one, the following command may be used:
>
> `gpg-connect-agent updatestartuptty /bye`
>
> Although all GnuPG components try to start the gpg-agent as needed, this is not possible for the ssh support because ssh does not know about it. **Thus if no GnuPG tool which accesses the agent has been run, there is no guarantee that ssh is able to use gpg-agent for authentication.** To fix this you may start gpg-agent if needed using this simple command:
>
> `gpg-connect-agent /bye`

ssh-agent 协议不包括告诉 agent 在哪个显示器/终端上运行的机制，于是 gpg-agent 的 ssh-support 将使用启动 gpg-agent 的那个 TTY。（用那个 tty 作为 passphrase 输入窗口的 prompting？）

用 gpg key 来做 ssh 的 authentication，ssh 就需要与 gpp-agent 建立一个稳定的沟通途径。但 ssh 这边没有一个机制可以直接“呼出”gpg-agent，就像其他 GPG 组件一样。所以任务就来到你这边，你需要确保 gpg-agent 正确运行了，ssh 才能用它来成功建立 authentication。
