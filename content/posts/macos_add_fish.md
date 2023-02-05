---
title: "macOS add fish to $SHELL and set fish as default shell"
date: 2023-01-26
draft: false
---

# Solution

### Add fish to $SHELL and set as default

1. Add the shell to `/etc/shells` with:

```bash
$ echo "/opt/homebrew/bin/fish" | sudo tee -a /etc/shells
````

2. Change default shell with:

```fish
chsh -s /opt/homebrew/bin/fish
```

### Modify `$PATH` with fish shell

1. 在 iTerm2 当中使用 fish shell 作为默认 shell，需要把 `$PATH` 加入到 `~/.config/fish/fish_variables`

```fish
set -U fish_user_paths /usr/local/bin $fish_user_paths
```

2. Type this command in terminal: `fish_add_path /opt/homebrew/bin`. It will add `/opt/homebrew/bin`  to the `~/.config/fish/fish_variables`.

## Other useful commands

`which $SHELL`

`echo $PATH`

Homebrew 下载的 Fish shell 的 path 在 `/opt/homebrew/bin/fish` 而不是像 [fish documentation](https://fishshell.com/docs/current/index.html#default-shell) 说的那样在 `/usr/local/bin/fish`。

# Reference

- [why i can't add fish to etc shells]([https://unix.stackexchange.com/questions/454604/why-i-cant-add-fish-to-etc-shells](https://unix.stackexchange.com/questions/454604/why-i-cant-add-fish-to-etc-shells))
- [fish documentation: set default shell](https://fishshell.com/docs/current/index.html#default-shell)
- [fish documentation: fish_add_path](https://fishshell.com/docs/current/cmds/fish_add_path.html)
- [StackOverflow: Modify PATH with fish shell](https://stackoverflow.com/questions/26208231/modifying-path-with-fish-shell)