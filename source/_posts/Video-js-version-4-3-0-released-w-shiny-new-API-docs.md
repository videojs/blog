---
title: Video.js version 4.3.0 released w/ shiny new API docs
tags:
  - version
author:
  name: Steve Heffernan
  github: heff
date: 2013-11-05 19:29:00
---

The biggest change in this update is actually an overhaul of the [API docs](https://github.com/videojs/video.js/tree/v4.3.0/docs/api). The best example of the new docs is the [Player doc](https://github.com/videojs/video.js/blob/v4.3.0/docs/api/vjs.Player.md), which is the API most video.js users will work with.

The new docs are now automatically generated from the code and code comments, making it easier to keep them up to date with what&rsquo;s currently in the codebase.

One interesting note about the doc-generator is that it uses [esprima](http://esprima.org), a tool that reads javascript files and gives back the &ldquo;abstract syntax tree&rdquo; of the code.

For the following javascript:

<pre class="prettify">
var hi;
</pre>

Esprima would generate:

<pre class="prettify">
{
    "type": "Program",
    "body": [
        {
            "type": "VariableDeclaration",
            "declarations": [
                {
                    "type": "VariableDeclarator",
                    "id": {
                        "type": "Identifier",
                        "name": "hi"
                    },
                    "init": null
                }
            ],
            "kind": "var"
        }
    ]
}
</pre>

We&rsquo;re using the AST of the video.js codebase to generate the majority of the information in the docs, which means it requires fewer comments and less work to keep the docs really great as we continue to build. If you&rsquo;re interested in seeing how we&rsquo;re handling that, check out the [doc-generator](https://github.com/videojs/doc-generator) repo (it&rsquo;s currently only useful with the video.js codebase, but it could be extended to support more).

## New CSS Options

Additional updates include new loading spinner icon options, and a new class for centering the big play button.

Many users have been clear that they&rsquo;d prefer the big play button in the center of the video. While we feel the trend is still moving towards getting the play button out of the way of the content, we wanted to make this feature easier to customize. You can now use the `vjs-big-play-centered` class on your video tag to center the play button.

To try the new spinner icon options, check out the [designer](http://designer.videojs.com) and change the icon name used by the spinner class.

## Even more plugins!

Finally, the most exciting developments are actually happening in the video.js community, with more and more plugins being built. We&rsquo;re up to 26 in the [plugins list](https://github.com/videojs/video.js/wiki/Plugins), with more on the way.

If you have some code you&rsquo;ve built on top of video.js that you think might be valuable to others, please share it on the plugins list, or post an issue on the video.js repo if you have questions about the plugin process.

## Full list from the changelog

*   Added Karma for cross-browser unit testing ([view](https://github.com/videojs/video.js/pull/714))
*   Unmuting when the volume is changed ([view](https://github.com/videojs/video.js/pull/720))
*   Fixed an accessibility issue with the big play button ([view](https://github.com/videojs/video.js/pull/777))
*   Exported user activity methods ([view](https://github.com/videojs/video.js/pull/783))
*   Added a classname to center the play button and new spinner options ([view](https://github.com/videojs/video.js/pull/784))
*   Added API doc generation ([view](https://github.com/videojs/video.js/pull/801))
*   Added support for codecs in Flash mime types ([view](https://github.com/videojs/video.js/pull/805))

The new version is available on [videojs.com](http://www.videojs.com) and  has been added to the CDN.

Cheers,

-heff
![](http://feeds.feedburner.com/~r/video-js/~4/HFDZaCSgXYI)
