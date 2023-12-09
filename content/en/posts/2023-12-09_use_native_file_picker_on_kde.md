---
title: "Use native file picker on KDE Plasma"
date: 2023-12-09
draft: true
tags: ["KDE", "Firefox", "VS Code"]
slug: use-native-file-picker-on-kde
---

### For Firefox

> Starting with version 64, Firefox can optionally use XDG Desktop Portals to handle various desktop features, such as **opening a file picker**, or handling MIME types. — Arch Wiki | Firefox | [XDG Desktop Portal integration](https://wiki.archlinux.org/title/Firefox#XDG_Desktop_Portal_integration)

1. Type `about:config` in Firefox address bar.
2. Search `widget.use-xdg-desktop-portal.file-picker`, set it to 1.
   - 0 – Never
   - 1 – Always
   - 2 – Auto (typically depends on whether Firefox is run from within Flatpak or whether the `GDK_DEBUG=portals` environment is set)
3. Done.

### For VS Code

These two environment variables: `GTK_USE_PORTAL=1`, `GDK_DEBUG=portals`.

Method 1:

1. Set environment variable `GTK_USE_PORTIAL=1`.
1. Add it to your Fish shell (or other shells you use) alias: `alias code "GTK_USE_PORTAL=1 code"`

Method 2:

1. Press Meta key to open Application Launcher.
1. Search for Visual Studio Code.
1. Right click > Edit Application...
1. In the popup window, choose Application tab.
1. Fill the "Enviroment variable" field: `GTK_USE_PORTIAL=1`.

But it's not working on my machine. So I actually use method 1.

---

Reference:

- [Why are there two file openers in KDE? — Reddit](https://www.reddit.com/r/kde/comments/188q87m/why_are_there_two_file_openers_in_kde_i_only_want/)
