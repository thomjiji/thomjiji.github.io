---
title: "Use cat command to add text to file quickly"
date: 2023-01-27T21:28:03+08:00
draft: false
cover:
    image: Study_01_Mac.png
    alt: "This is cosmic"
tags: ["Linux"]
---

## Using `echo`

```bash
touch file.md
echo "hello" > file.md
```

## Using `cat` and standard redirect symbol `>`

```bash
cat >> file.md
```

This command creates an empty yet edited file as it prompts the user to create a text file and type in the file at the same time.

If you don't want to edit the file, simply press `CTRL+C` and it will simply exit and create an empty file.

If you would like to add some text to the file, you can type in after this, like this:

```bash
cat >> file.md
This is some text in the file from command line.
```

## Reference

[https://www.geeksforgeeks.org/cat-command-in-linux-with-examples/](https://www.geeksforgeeks.org/cat-command-in-linux-with-examples/)