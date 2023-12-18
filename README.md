### Publishing Process

1. After making any changes (such as adding new posts, adjusting styles, etc.), first
   perform a commit.

   ```shell
   git commit -m ""
   ```

1. Next, build the entire site with the new changes. The static site will be built into
   the `docs/` directory (for the GitHub page). Hugo default is `public/`.

   ```shell
   hugo -D
   ```

   `-D` flag means build with drafts.

2. Finally, commit once more to upload the built static site to GitHub.

   ```shell
   git commit -m "build"
   ```

### Serve localhost

```shell
hugo server -D
```

### Add new post

```shell
hugo new posts/new_post.md
```

### Clean up before rebuild

Hugo will not automatically clean up static site in the `docs/` directory for you, so
every time you rebuild, the new content will be mixed with the old content. Although it
rarely causes conflicts, it is better to run before rebuilding: `rm -rf docs/*` — with
cautious!

See this issue: ['hugo clean'](https://github.com/gohugoio/hugo/issues/2389)

### Link to post itself

Use the post's slug:

```
[Link to post]({{< ref "/posts/post-slug/" >}})

[Link to post](/posts/post-slug/)
```

### Link to a specific heading or paragraph:

```
## My Header {#my-header}

Some paragraph here. {#custom-para}
```

```
[Header](/posts/post/#my-header)

[Paragraph](/posts/post/#custom-para)
```
