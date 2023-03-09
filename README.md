# Publishing Process

1. 在修改完内容、样式等随便什么东西之后，首先 commit：

```shell
git commit -m ""
```

2. 然后 build 整个 site，生成在 docs 这个 dir 里（默认是 `public/`）。

```shell
hugo
```

3. 最后再 commit 一次，把 build 后的 static site update 到 GitHub。

```shell
git commit -m "<> build"
```

# Serve localhost

```shell
hugo server -D
```

# Add new post

```shell
hugo new posts/new_post.md
```