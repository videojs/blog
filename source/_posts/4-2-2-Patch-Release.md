---
title: 4.2.2 Patch Release
tags:
  - version
author:
  name: Steve Heffernan
  github: heff
alias:
  - post/64150919930/index.html
date: 2013-10-15 18:26:00
---

Two bugs have been squashed with this patch:

*   An issue most commonly seen in Firefox where video playback would break when a race condition would occur during video loading ([#756](https://github.com/videojs/video.js/issues/756))
*   An issue where the duration would get stuck at 0:00 when loading the player dynamically ([#775](https://github.com/videojs/video.js/issues/775))

[See the changes made](https://github.com/videojs/video.js/pull/776/files)

This version can be downloaded on [videojs.com](http://www.videojs.com), is available on the CDN, and the existing /4.2/ CDN version has been  updated to 4.2.2\. (may take time to propagate to your area)

Cheers,

-heff
![](http://feeds.feedburner.com/~r/video-js/~4/I0mpf2qGi7g)
