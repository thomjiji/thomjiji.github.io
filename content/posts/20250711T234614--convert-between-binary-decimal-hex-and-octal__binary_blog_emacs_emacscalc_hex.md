---
title: "Convert between binary decimal hex and octal"
date: 2025-07-11
tags: ["binary", "emacs", "emacscalc", "hex"]
draft: false
---

## 使用 Emacs Calc {#使用-emacs-calc}

1.  首先开启 Emacs Calc：​`C-x * c`
2.  输入你想要进行转换的源头数字的原始形式。比如你想要把一个 decimal number 192 转换成二进制，那么输入 192
3.  然后按 d 改变 Emacs Calc 的显示模式，再按 2 选择以二进制显示。这样你刚输入的数字 192 就变成了 `2#11000000`​。
4.  这样我们的转换实际上是改变 Calc 对数字的显示模式，其实也达到了我们的目的。


### Notes {#notes}

我们可以看到，Emacs Calc 显示非 decimal 十进制数字的时候使用的 notation 是这样：

```nil
2#11000000 <-- binary
16#C0      <-- hex
8#300      <-- octal
```

在 `#` 前加上一个数字表示二进制、16 进制和八进制。正常的十进制就没有前缀了。


### Using Emacs Calc (Base to Decimal) {#using-emacs-calc--base-to-decimal}

1.  Start Emacs Calc: `C-x * c`
2.  Input the number using the base notation `base#number`.
    -   For Hex `C0`: Type `16#C0` then press `RET`.
    -   For Binary `11000000`: Type `2#11000000` then press `RET`.
3.  Press `d 0` (decimal mode).
4.  Calc will instantly convert the value on the stack to its standard decimal form (e.g., `192`).


#### Notes {#notes}

-   When you are in a non-decimal display mode (like Binary), you can just type the digits directly (e.g., `1100`) and Calc will assume they are in the current display base.
-   However, typing the `base#` prefix is the most reliable way to enter numbers because it works regardless of your current display mode.
-   `d 0` is the shortcut for "Decimal" display, while `d r` allows you to set any arbitrary "Radix" (base) from 2 to 36.


## 使用 M-: (eval-expression) {#使用-m--eval-expression}

1.  按 `M-:` (eval-expression)
2.  输入你想要转换的十进制数，比如 192，按 `RET`
3.  得到该十进制数对应的八进制和十六进制结果：~192 (#o300, ​#xc0)

可以看到这个方法只给到了 octal 和 hex，并没给到 binary 的结果。当然你可以输入一个 binary，比如 `#b11000000` 然后按 RET 即可得到一样的输出：192 (#o300, ​#xc0)。所以我们是可以通过这个方法做 binary =&gt; decimal、octal、hex 的转换，但是！不可以做 =&gt; binary 的转换，it's not possible.


## 使用 Python {#使用-python}

在 Terminal 输入 python3 进入 “Python Interactive Shell REPL”：

```bash
$ python3
Python 3.13.5 (main, Jun 12 2025, 00:00:00) [GCC 15.1.1 20250521 (Red Hat 15.1.1-2)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> bin(192)  # convert decimal number to binary
'0b11000000'
>>> oct(192)  # convert decimal number to octal
'0o300'
>>> hex(192)  # convert decimal number to hexadecimal
'0xc0'
>>> 0b11000000  # convert hex to decimal
'192'
>>> 0o300  # convert octal to decimal
'192'
```
