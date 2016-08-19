# video.js blog
The new video.js blog based on [hexo][].

[hexo]: https://www.npmjs.com/package/hexo

# Writing New Posts
To write a new post on the blog, first run
```
npm run draft "blog title"
```
This will create a markdown file with the filename as a slug of the blog title in `source/_drafts`. For example: `source/_drafts/blog-title.md`.

## Assets
When generating a draft, a folder with the same slug as the blog post is generated as a sibling of the blog post: `source/_drafts/blog-title/`.
You can place various assets in there and then refer to them locally.
For example, for an image name `title.png`, you can use the following embed code `![title](title.png)` and hexo will resolve this for you.

# Running the blog locally
To see your drafts and create a local server that watches your changes, just run
```
npm run dev
```

# Publishing Posts
When you're ready to publish a post, you'll want to run
```
npm run publish "blog title"
```
This will move the post and the assets folder from `_drafts/` to `_posts` and also update the timestamp and other metadata.

# Releasing the blog
You've published your posts, now to publish the blog to the public, you can run
```
npm run release
```

# Running other hexo commands
You can either install `hexo` globally to run other commands, or you can just use the `hexo` npm script:
```
npm run hexo -- help
```
Make sure that the `--` is there and is separate from the rest of the commands.
