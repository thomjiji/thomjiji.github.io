# Publishing Process

1. After making any changes (such as adding new posts, adjusting styles, etc.), first 
   perform a commit.

   ```shell
   git commit -m ""
   ```

1. Next, build the entire site with the new changes. The static site will be built into
   the `docs/` directory (for the GitHub page). Hugo default is `public/`.

   ```shell
   hugo
   ```

2. Finally, commit once more to upload the built static site to GitHub.

   ```shell
   git commit -m "build: <message>"
   ```

   I use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).

# Serve localhost

```shell
hugo server -D
```

# Add new post

```shell
hugo new posts/new_post.md
```

# Clean up before rebuild

Hugo will not automatically clean up static site in the `docs/` directory for you, so
every time you rebuild, the new content will be mixed with the old content. Although it
rarely causes conflicts, it is better to run before rebuilding: `rm -rf docs/*` — with 
cautious!

See this issue: ['hugo clean'](https://github.com/gohugoio/hugo/issues/2389)

# About commits messages (deprecated)

A commit message preceded by a ↓ means that this is a build commit, and this build
includes all commits since the last ↓.

```
9449266 ↓ Add content to about page, Changed menu order build  <----|
1cd1e90 Add content to about page, Changed menu order          <----|
6a9577e ↓ Menu Archive => Posts                         <----|
3505a41 Menu Archive => Posts                           <----|
78aae05 ↓ ajust dolby vision posts heading weight build        <----|
907c2c2 ajust dolby vision posts heading weight                     |
177f1a6 Reduce page title size                                 <----|
```

# Link to post itself

Use the post's slug:

```
[Link to post]({{< ref "/posts/post-slug/" >}})

[Link to post](/posts/post-slug/)
```

# Link to a specific heading or paragrah:

```
## My Header {#my-header}

Some paragraph here. {#custom-para}
```

```
[Header](/posts/post/#my-header)

[Paragraph](/posts/post/#custom-para)
```