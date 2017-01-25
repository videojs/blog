---
title: Video.js 5's fluid mode and playlist picker
tags:
  - videojs 6
  - flash
  - html5
author:
  name: Michael Roca
  github: mrocajr
date: 2016-06-01 14:50:06
---

In August of 2016, we announced our intention of removing flash as a part of the core video.js project. 
As html5 video becomes the standard playback tech and flash fades into obsolescence, it has become time 
to remove flash from the core player and move it to a separate code base.  This will give us the ability 
to allow developer to continue to support legacy browsers by adding the tech themselves, while allowing 
us to minimize legacy code in video.js and decrease the footprint of the player.

The [videojs-flash] project has been created for the Flash tech to be used.

```html
 <link rel="stylesheet" href="path/video.js/dist/video-js.css">
 <script src="path/video.js/dist/video.js"></script>
 <script src="path/videojs-flash/dist/videojs-flash.js"></script>
<video  id='vid' class='video-js' controls height=300 width=600>
 <source src="video.mp4" type="video/mp4">
</video>
<script> var player = videojs('vid', {techOrder: ['flash']}); </script>
```

[videojs-flash]: https://github.com/videojs/videojs-flash
[announced]: https://github.com/videojs/blog/blob/master/source/_posts/the-end-of-html-first.md
