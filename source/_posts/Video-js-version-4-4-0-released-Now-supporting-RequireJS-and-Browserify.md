---
title: Video.js version 4.4.0 released - Now supporting RequireJS and Browserify
tags:
  - version
author:
  name: Steve Heffernan
  github: heff
date: 2014-02-19 17:49:00
---

Version 4.4.0 is here with over 20 updates and fixes. The most notable addition may be support for AMD and CommonJS module loaders, meaning you can now include Video.js using [RequireJS](http://requirejs.org) or [Browserify](http://browserify.org). Video.js can be installed through [npm](http://www.npmjs.org/package/video.js) already, and we&rsquo;ll soon add support for [bower](http://bower.io) and [component(1)](http://component.io) as well.

### New plugins

The [plugin list](https://github.com/videojs/video.js/wiki/Plugins) continues to grow, with more in the works. Checkout the new [endcard plugin](http://theonion.github.io/videojs-endcard/) built by [The Onion](http://www.theonion.com).

*   [videojs-markers](https://github.com/spchuang/videojs-markers) [[demo]](http://jsbin.com/befob/7/edit) - Add customizable markers on the progress bar
*   [videojs-youtube-videowall](https://github.com/several27/videojs-youtube-videowall) - [[demo]](http://jsbin.com/tave/3) - Display video wall of related youtube videos after the video finishes
*   [videojs-endcard](http://theonion.github.io/videojs-endcard/) - Simple, customizable end card solution for VideoJS
*   [videojs-akamai-analytics](https://github.com/rsadwick/videojs-akamai-analytics) - Akamai Sola Analytics for video.js
*   [videojs-stereopanner](https://github.com/cladera/videojs-stereopanner) - Adds stereo channel panning functions

### Video.js in the wild

The [Brightcove Play 2014](https://play.brightcove.com) website recently went live with Video.js as the player. We&rsquo;re also hard at work building Brightcove&rsquo;s next-gen player with Video.js at the core, so stay tuned.

## Full list from the changelog

*   Made the poster updateable after initialization ([view](https://github.com/videojs/video.js/pull/838))
*   Exported more textTrack functions ([view](https://github.com/videojs/video.js/pull/815))
*   Moved player ID generation to support video tags with no IDs ([view](https://github.com/videojs/video.js/pull/845))
*   Moved to using QUnit as a dependency ([view](https://github.com/videojs/video.js/pull/850))
*   Added the util namespace for public utility functions ([view](https://github.com/videojs/video.js/pull/862))
*   Fixed an issue with calling duration before Flash is loaded ([view](https://github.com/videojs/video.js/pull/861))
*   Added player methods to externs so they can be overridden ([view](https://github.com/videojs/video.js/pull/878))
*   Fixed html5 playback when switching between media techs ([view](https://github.com/videojs/video.js/pull/887))
*   Fixed Firefox+Flash mousemove events so controls don&rsquo;t hide permanently ([view](https://github.com/videojs/video.js/pull/899))
*   Fixed a test for touch detection ([view](https://github.com/videojs/video.js/pull/962))
*   Updated the src file list for karma tests ([view](https://github.com/videojs/video.js/pull/948))
*   Added more tests for API properties after minification ([view](https://github.com/videojs/video.js/pull/906))
*   Updated projet to use npm version of videojs-swf ([view](https://github.com/videojs/video.js/pull/930))
*   Added support for dist zipping on windows ([view](https://github.com/videojs/video.js/pull/944))
*   Fixed iOS fullscreen issue ([view](https://github.com/videojs/video.js/pull/977))
*   Fixed touch event bubbling ([view](https://github.com/videojs/video.js/pull/992))
*   Fixed ARIA role attribute for button and slider ([view](https://github.com/videojs/video.js/pull/988))
*   Fixed and issue where a component&rsquo;s dispose event would bubble up ([view](https://github.com/videojs/video.js/pull/981))
*   Quieted down deprecation warnings ([view](https://github.com/videojs/video.js/pull/971))
*   Update seek handle to display the current time ([view](https://github.com/videojs/video.js/pull/902))
*   Added requirejs and browserify support (UMD) ([view](https://github.com/videojs/video.js/pull/998))

The new version is available on [videojs.com](http://www.videojs.com) and  has been added to the CDN.

Cheers,

-heff
![](http://feeds.feedburner.com/~r/video-js/~4/ZZPGdmp690s)
