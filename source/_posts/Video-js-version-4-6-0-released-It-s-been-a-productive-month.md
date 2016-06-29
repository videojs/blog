---
title: Video.js version 4.6.0 released! It's been a productive month.
tags:
  - version
author:
  name: Steve Heffernan
  github: heff
date: 2014-05-20 17:29:00
---

The video.js community has been in full force lately and it&rsquo;s resulted in a lot of great features and fixes, including UI updates, better error messages, and even a [Video.js Polymer element](https://github.com/addyosmani/video-js) built by [Addy Osmani](https://twitter.com/addyosmani) himself.

### New Live UI

![](http://66.media.tumblr.com/18c6ec3dd9a1e04440345bf5ca6630f9/tumblr_inline_n5ue8gvi1Y1qzc111.png)

Video.js has supported different forms of live video for a while, but a recent update has made the user experience a little clearer. Specifically, a &lsquo;LIVE&rsquo; badge is added to the controls and the seek bar is hidden when seeking isn&rsquo;t permitted.

### Clearer Errors

A big effort went into improving error situations that developers and viewers might encounter. In cases where the viewer&rsquo;s browser supports neither JavaScript nor HTML5 Video, a more helpful message is shown that informs the viewer how they can support video playback.

![](http://66.media.tumblr.com/ce08d85f3a1538eecd1d58eccddaa0b6/tumblr_inline_n5ue7u9l4H1qzc111.png)

In cases where a common media error occurs (e.g. the file doesn&rsquo;t exist or the network fails), an &ldquo;X&rdquo; icon is displayed showing that playback can&rsquo;t continue, and a message is shown describing the issue. Additionally an error message is logged to the javascript console.

![](http://67.media.tumblr.com/72b11aeeab66657ec39c09366bd19f0e/tumblr_inline_n5ue8z9ZFH1qzc111.png)

For developers, better logging functions have been added including `videojs.log.error()`, and they&rsquo;re being used throughout the code base to provide better information and help track down issues.

### IE11 Fullscreen

Video.js relies on native browser fullscreen even when Flash is used. IE11 is the first version of Internet Explorer to support native browser fullscreen, and video.js has now been updated to take advantage of that feature and provide a better fullscreen experience for those users.

### Playback Rate Switching

If you ever wanted to speed up or slow down the rate of a video, now you can! HTML5 video browsers have been adding support for playback rate switching, and video.js now has an optional UI component that will let you select the speed. Flash however does not support playback rate switching, so unfortunately it&rsquo;s not a feature that users on older browsers (e.g. IE8) can use.

[See a demo.](http://jsbin.com/vikun/1/edit)

### New community plugins

The latest plugins to be added to the [video.js plugins list](https://github.com/videojs/video.js/wiki/Plugins).

*   [videojs-vr](https://github.com/slawrence/videojs-vr): Project video onto different geometric shapes (Sphere, Cube, Cylinder) and view in 3d with optional Oculus Rift support
*   [video-speed](https://github.com/nicetip/videojs-speed): Adds customizable video-speed control
*   [OpenVideoAnnotation](https://github.com/CtrHellenicStudies/OpenVideoAnnotation) - create annotations in video-js using annotator
*   [videojs-overlay](https://github.com/brightcove/videojs-overlay): display simple HTML overlays during video playback
*   [video.js-polymer](https://github.com/addyosmani/video-js): A video.js element for the Polymer web components framework

If you&rsquo;d like a head-start on the scaffolding for a new video.js plugin, check out the [Yeoman video.js plugin generator](https://github.com/dmlap/generator-videojs-plugin).

## Full list from the change log

*   Updated the UI to support live video ([view](https://github.com/videojs/video.js/pull/1121))
*   The UI now resets after a source change ([view](https://github.com/videojs/video.js/pull/1124))
*   Now assuming smart CSS defaults for sliders to prevent reflow on player init ([view](https://github.com/videojs/video.js/pull/1122))
*   Fixed the title element placement in menus ([view](https://github.com/videojs/video.js/pull/1114))
*   Fixed title support for menu buttons ([view](https://github.com/videojs/video.js/pull/1128))
*   Fixed extra mousemove events on Windows caused by certain apps, not users ([view](https://github.com/videojs/video.js/pull/1068))
*   Fixed error due to undefined tech when no source is supported ([view](https://github.com/videojs/video.js/pull/1172))
*   Fixed the progress bar not finishing when manual timeupdate events are used ([view](https://github.com/videojs/video.js/pull/1173))
*   Added a more informative and styled fallback message for non-html5 browsers ([view](https://github.com/videojs/video.js/pull/1181))
*   Added the option to provide an array of child components instead of an object ([view](https://github.com/videojs/video.js/pull/1093))
*   Fixed casing on webkitRequestFullscreen ([view](https://github.com/videojs/video.js/pull/1101))
*   Made tap events on mobile less sensitive to touch moves ([view](https://github.com/videojs/video.js/pull/1111))
*   Fixed the default flag for captions/subtitles tracks ([view](https://github.com/videojs/video.js/pull/1153))
*   Fixed compilation failures with LESS v1.7.0 and GRUNT v0.4.4 ([view](https://github.com/videojs/video.js/pull/1180))
*   Added better error handling across the library ([view](https://github.com/videojs/video.js/pull/1197))
*   Updated captions/subtiles file fetching to support cross-origin requests in older IE browsers ([view](https://github.com/videojs/video.js/pull/1095))
*   Added support for playback rate switching ([view](https://github.com/videojs/video.js/pull/1132))
*   Fixed an issue with the loadstart event order that caused the big play button to not hide ([view](https://github.com/videojs/video.js/pull/1209))
*   Modernized the fullscreen API and added support for IE11 ([view](https://github.com/videojs/video.js/pull/1205))
*   Added cross-browser testing with SauceLabs, and added Karma as the default test runner ([view](https://github.com/videojs/video.js/pull/1187))
*   Fixed saucelabs integration to run on commits in TravisCI ([view](https://github.com/videojs/video.js/pull/1214))
*   Added a clearer error message when a tech is undefined ([view](https://github.com/videojs/video.js/pull/1210))
*   Added a cog icon to the font icons ([view](https://github.com/videojs/video.js/pull/1211))
*   Added a player option to offset the subtitles/captions timing ([view](https://github.com/videojs/video.js/pull/1212))

The new version is available on [videojs.com](http://www.videojs.com) and  has been added to the CDN.

Cheers,

-[heff](http://blog.heff.me)
![](http://feeds.feedburner.com/~r/video-js/~4/Ho_lv_Coqlc)
