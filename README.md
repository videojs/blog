# video.js blog
The new video.js blog based on [hexo][].

Lead Maintainer: Gary Katsevman [@gkatsev](https://github.com/gkatsev])

Maintenance Status: Stable

## Table of Contents

* [New Posts](#new-posts)
  * [Step 1: Writing the post and opening a PR](#step-1-writing-the-post-and-opening-a-pr)
  * [Step 2: When the your writing is done](#step-2-when-the-your-writing-is-done)
* [Running the blog locally](#running-the-blog-locally)
* [Editing Published posts](#editing-published-posts)
* [Running other hexo commands](#running-other-hexo-commands)

## New Posts

### Step 1: Writing the post and opening a PR

To write a new post on the blog. Create a new fork/branch of [videojs/blog][videojs-blog] repo and then run:

```sh
npm run draft -- "blog title"
```

> Note: At any point after this you can start a server that will show drafts by running `npm run start`

This will create a folder and a markdown file. In the example they would be:

```sh
source/_drafts/blog-title/
source/_drafts/blog-title.md
```

Your actual blog post is the markdown file and the folder is where you can store any assets for that post. For instance to add an image `videojs.png` to our example blog post you would add the image file to `source/_drafts/blog-title/videojs.png`. Then you can reference that image using markdown syntax in `source/_drafts/blog-title.md` as `[!videojs image](videojs.png)`.

Don't forget to fill out the yaml front-matter at the top of your blog post. Especially the author name and github username. These will allow the post to display your information as the author of the blog post.

```yml
author:
  name: Your Name
  github: githubusername
```

### Step 2: When the your writing is done
When you are done writing your blog post, open a pull request against the master branch of [videojs/blog][videojs-blog]

Then get a review and approval from someone in the Videojs organization.

> Note: Your pr will have a [netlify][] preview built for it so that you can preview your draft. Check out the status checks in your PR.

After your post is approved, you have publish your post. In hexo this basically means moving your markdown file and folder into a folder in `_posts`. To do that you run:

```sh
npm run publish -- "blog title"
```

This will also update the timestamp and other metadata. After moving your post be sure to verify that everything looks okay with `npm run start`.

You should then commit all the published changes and push it up to your fork/branch. At this point your post will be released by [netlify][] as soon as it is merged into master!

## Running the blog locally

To see your drafts and create a local server that watches your changes, just run

```sh
npm run start
```

## Editing Published posts

Published posts are located in `source/_posts` with their post name as the blog title. Feel free to make an edit in a fork/branch and open a pull request against the master branch of [videojs/blog][videojs-blog]

## Running other hexo commands

You can run other hexo commands with the local npm binary or by using the the `hexo` npm script:

    npm run hexo -- help

Make sure that the `--` is there and is separate from the rest of the commands.

[hexo]: https://www.npmjs.com/package/hexo
[videojs-blog]: https://github.com/videojs/blog
[netlify]: https://www.netlify.com/
