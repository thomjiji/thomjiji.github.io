---
title: "Create dummy directories using Rsync"
date: 2024-06-25
draft: false
tag: ["rsync"]
---

Use `rsync` to create dummy dirs:

```
rsync -avPih --include '*/' --exclude '*' videos/ ~/Desktop/videos-dummy/
```

Here’s a breakdown of the command:

- `-avPih`: These flags stand for:
  - `-a`: Archive mode, which preserves permissions, timestamps, symbolic links, etc.
  - `-v`: Verbose, which provides detailed output.
  - `-P`: Combines --progress and --partial, showing progress during transfer and allowing for partial transfers to be resumed.
  - `-i`: Output a change-summary for all updates.
  - `-h`: Output numbers in a human-readable format.
- `--include '*/'`: Includes all directories (and subdirectories) in the transfer.
- `--exclude '*'`: Excludes all files.
