---
title: 'Video.js version 4.5.0 released! Nothing to see here, move along'
tags:
  - version
author:
  name: Steve Heffernan
  github: heff
alias:
  - post/80918861872/videojs-version-450-released-nothing-to-see/index.html
  - post/80918861872/index.html
date: 2014-03-27 18:49:04
---

Well, sort of… this release is the result of a lot of hard work to speed up the version release process, meaning they&rsquo;ll be coming a lot faster now with smaller sets of changes.

In this release we&rsquo;ve added support for the [Component(1)](http://component.io) package manager, fixed the captions positioning when the controls are hidden, and helped Android devices know when they can support HLS. ;-) There were a few other changes as well but a lot of the work this round was done on the processes around the project. If you&rsquo;ve spent time in the github issues, you&rsquo;ll hopefully notice an improvement in the response rate on issues and the speed in which bugs get fixed.

### New plugins

*   [videojs-vast](https://github.com/theonion/videojs-vast-plugin): A VideoJS plugin to play pre-roll videos from a VAST feed
*   [videojs-comscore](https://github.com/brandonaaskov/videojs-comscore): Reports to comScore using their latest Streaming Tag SDK

### Video.js in the wild

[Toyota Europe!](http://www.toyota-europe.com/cars/new_cars/yaris/index.tmex) (click a video on that page) I was in Paris a few weeks ago and stopped by the Toyota store on Champs-Elysées. Next to the cars are multimedia touch screens that tell you details and allow you to watch videos. The video player looked kind of familiar, and sure enough it was video.js with a very nice custom skin. It turns out the Toyota Europe website uses video.js as well. Very cool.

If you find video.js on an interesting site somewhere, let us know in the comments.

## Full list from the change log

*   Added component(1) support ([view](https://github.com/videojs/video.js/pull/1032))
*   Captions now move down when controls are hidden ([view](https://github.com/videojs/video.js/pull/1053))
*   Added the .less source file to the distribution files ([view](https://github.com/videojs/video.js/pull/1056))
*   Changed src() to return the current selected source ([view](https://github.com/videojs/video.js/pull/968))
*   Added a grunt task for opening the next issue that needs addressing ([view](https://github.com/videojs/video.js/pull/1059))
*   Fixed Android 4.0+ devices&rsquo; check for HLS support ([view](https://github.com/videojs/video.js/pull/1084))

The new version is available on [videojs.com](http://www.videojs.com) and  has been added to the CDN.

Cheers,

-heff
![](http://feeds.feedburner.com/~r/video-js/~4/GpehQHGlbCM)
