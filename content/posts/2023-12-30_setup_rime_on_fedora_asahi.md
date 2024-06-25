---
title: "Setup Rime on Fedora Asahi Remix"
date: 2023-12-30
draft: false
slug: setup-rime-on-fedora-asahi
tags: ["Asahi Linux", "Rime", "Fedora"]
---

1. Install fcitx5 and fcitx5-rime via `sudo dnf install fcitx5-rime fcitx5`.
2. Go to System Setting > Input Devices > Virtual Keyboard, select “Fcitx 5”.
3. Edit environment file via: `sudo vi /etc/environment`.
    1. Add the following content, [reference](https://fcitx-im.org/wiki/Using_Fcitx_5_on_Wayland#KDE_Plasma):

        ```
        INPUT_METHOD=fcitx
        XMODIFIERS=@im=fcitx
        ```

4. Open Input Method Selector > Use fcitx5 > Log out.
5. Clone [雾凇拼音](https://github.com/iDvel/rime-ice) repo into `~/.local/share/fcitx5/`, rename it to `rime`:
    ```
    $ mv rime-ice rime
    ```
6. Restart fcitx5 in the lower right corner of your Desktop.
