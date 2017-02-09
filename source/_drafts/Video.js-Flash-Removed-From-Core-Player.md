---
title: Video.js removes flash from core player
tags:
  - video.js 6.0
  - Flash
  - html5
author:
  name: Michael Roca
  github: mrocajr
---

In August of 2016, we [announced] our intention of removing flash as a part of the core Video.js project.
As html5 video becomes the standard playback tech and flash fades into obsolescence, it is time 
to remove flash from the core player and move it to a separate code base.  This will give us the ability 
to allow developers to continue to support legacy browsers by adding the tech themselves, while allowing 
us to minimize legacy code in Video.js and decrease the footprint of the player.

This follows in the footsteps of Chrome, Safari and Firefox which are all taking steps to deprecate flash.

Chrome: [Chrome 56 disables flash by default]

Safari: [Safari makes Flash a legacy plugin]

Firefox: [Reducing Adobe Flash Usage in Firefox]

As of the Video.js 6.0 release, the dream of a flashless future will come closer to a reality.

In the meantime, the separate [videojs-flash] project has been created for Flash tech support.
When the [videojs-flash] plugin is added to the player, the Flash tech is added to the tech order.

```html
<link rel="stylesheet" href="path/video.js/dist/video-js.css">
<script src="path/video.js/dist/video.js"></script>
<script src="path/videojs-flash/dist/videojs-flash.js"></script>

<video  id='vid' class='video-js' controls height=300 width=600>
  <source src="video.mp4" type="video/mp4">
</video>

<script>
  var player = videojs('vid');
</script>
```

[videojs-flash]: https://github.com/videojs/videojs-flash
[announced]: http://blog.videojs.com/the-end-of-html-first/
[Chrome 56 disables flash by default]: https://blog.chromium.org/2016/12/roll-out-plan-for-html5-by-default.html
[Safari makes Flash a legacy plugin]: https://webkit.org/blog/6589/next-steps-for-legacy-plug-ins/
[Reducing Adobe Flash Usage in Firefox]: https://blog.mozilla.org/futurereleases/2016/07/20/reducing-adobe-flash-usage-in-firefox/
