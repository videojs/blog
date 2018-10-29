---
title: 'Video.js 7.3: Responsive Layout, Fill Mode, createLogger'
author:
  name: Gary Katsevman
  github: gkatsev
tags:
---

Another month, another Video.js release: [v7.3.0][]. This release brings a long wanted feature: Responsive Layout.
In addition, Fill Mode has been promoted to a full mode, and createLogger was added to make debugging and logging easier.

This is currently out as a pre-release and will be promoted to latest in about a week. Please try it out and [report any issues you find][issues].

## Thanks
I'd like to thank everyone that was involved in making changes that landed in this release. Any and all changes are really appreciated.

- [@BrandonOCasey](https://github.com/BrandonOCasey)
- [@chrisrng](https://github.com/chrisrng)
- [@syranez](https://github.com/syranez)
- [@misteroneill](https://github.com/misteroneill)
- [@OwenEdwards](https://github.com/OwenEdwards)

## Responsive Mode
[Responsive Mode][] will make the player adjust UI components to the size of the player. It uses the [`playerresize`][] event that was added in [v6.7.0][] of Video.js, which allows us to know when the player changes sizes.

Responsive mode will set and change certain breakpoint classes like `vjs-layout-small` when the player size changed, these can be [configured][breakpoints], then, depending on which class is currently on the player, the control bar and other UI elements can adapt. For example, with `vjs-layout-small`, the time controls will not show because the time tooltips on the progress bar are available and the captions button is more important. At a larger size, both can be shown without a problem.

You can find out how to enable [Responsive Mode][] and more in our [docs page][Responsive Mode].

## Fill Mode
[Fill Mode][] is allows the Video.js player to resize dynamically but stay contained within in bounds of the parent container. This is analogous to [Fluid Mode][] but sometimes you the container is already being sized properly and you don't want Video.js to resize itself because it may break out of the container.

Fill Mode is not a brand new mode, the class `vjs-fill` has been available in Video.js for quite [a while][commit]. This finally makes it a full mode to go along with Fluid Mode.

## createLogger
This is a new method on `videojs.log` that allows you to create a new logger with a specific name. It then creates a chain of names to make it easier to track which component logged this particular message.
For example, a new usage of it is `player.log` which logs the player ID in addition to `VIDEOJS`:

```js
var player = videojs('myid');

player.log('foo');
// VIDEOJS: myid: foo
```

You can also go further and create a sub logger:

```js
var player = videojs('myid');
var mylog = player.log.createLogger('myLogger');

mylog.log('foo');
// VIDEOJS: myid: myLogger: foo
```

[v6.7.0]: https://github.com/videojs/video.js/releases/tag/v6.7.0
[v7.3.0]: https://github.com/videojs/video.js/releases/tag/v7.3.0
[`playerresize`]: https://github.com/videojs/video.js/pull/4864
[breakpoints]: https://docs.videojs.com/tutorial-layout.html#customizing-breakpoints
[Responsive Mode]: https://docs.videojs.com/tutorial-layout.html#responsive-mode
[Fill Mode]: https://docs.videojs.com/tutorial-layout.html#fill-mode
[Fluid Mode]: https://docs.videojs.com/tutorial-layout.html#fluid-mode
[commit]: https://github.com/videojs/video.js/commit/2fc8968002cf2f40128c39699c3ffbaac73fc9ed#diff-6be43b1f61c2cbcb90c6cb4a762ad527R64
[issues]: https://github.com/videojs/video.js/issues/new
