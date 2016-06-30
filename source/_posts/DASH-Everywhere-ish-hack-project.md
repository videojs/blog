---
title: DASH Everywhere-ish (hack project)
tags:
  - news
  - html5 video
author:
  name: Steve Heffernan
  github: heff
alias:
  - post/92536319027/dash-everywhere-ish-hack-project/index.html
  - post/92536319027/index.html
date: 2014-07-22 11:01:00
---

A couple of times a year Brightcove has an internal hackweek where engineers work on any project they&rsquo;d like. In the latest hackweek (2014-07-14) [Tom Johnson](https://github.com/seniorflexdeveloper) decided to see if he could get DASH supported in as many places as possible, by combining a few of the existing DASH player implementations with Video.js.

[MPEG-DASH](http://dashif.org/mpeg-dash/) (Dynamic Adaptive Streaming over HTTP) is a streaming format similar to Apple&rsquo;s [HTTP Live Streaming (HLS)](https://developer.apple.com/streaming/). It allows you to provide multiple versions of a video at different bitrates, and then the player can switch between those versions depending on the user&rsquo;s bandwidth (which is more complicated than you might think).

The two DASH playback implementations used were [Dash.js](https://github.com/Dash-Industry-Forum/dash.js) and [Dash.as](https://github.com/castlabs/dashas). They were combined using video.js&rsquo;s playback tech architecture, which means you can include [plugins](https://github.com/videojs/video.js/wiki/Plugins) and custom skins and they&rsquo;ll work the same with either playback method.

[See the results.](http://mixxture.com/players/brightcove/dash/demo.html)

### Browser/Device Coverage

Using a combination of DASH.AS and DASH.JS will give us the following browser/device coverage:

#### DASH.JS (media source extensions support)

*   Internet Explorer: 11+
*   Chrome: 23+
*   FireFox: 25+ (upcoming version)
*   Safari (Desktop): 8+ (OSX Yosemite - Fall 2014)
*   iOS: No
*   Android: 4.2+ (Chrome)

#### DASH.AS

Fallback to any environment that supports Flash Player 10.3

### iOS

As you can see, the one remaining holdout is iOS and there&rsquo;s currently no word when or if that will happen. Seeing media source extensions support in Safari 8 gives some hope, but my understanding is the requirements for getting support built into iOS are much more significant. My guess is it will happen eventually, but not for a while (and hopefully in-line playback + the fullscreen API will be supported at the same time).

Today, to provide adaptive streaming everywhere, you still need either DASH + HLS, or just HLS (you can use the [video.js HLS plugin](https://github.com/videojs/videojs-contrib-hls) to support HLS in more browsers).

## Tom&rsquo;s Notes

The demo shows that for environments which support [Media Source Extensions](https://developer.mozilla.org/en-US/docs/Web/API/MediaSource) (MSE) we use a full Javscript implementation via [Dash.js](https://github.com/Dash-Industry-Forum/dash.js) and as a fallback we use Flash built around [OSMF](http://sourceforge.net/adobe/osmf/home/Home/) and [Dash.as](https://github.com/castlabs/dashas), an open-source plugin provided by [Castlabs](http://castlabs.com).

> _Tom has been working on [videojs-osmf](https://github.com/seniorflexdeveloper/videojs-osmf) on the side, which helped make this possible._

### DASH.AS Requirements

#### Player

*   Environment must support Flash Player 10.3+
*   Video.js OSMF Tech ([videojs-osmf](https://github.com/seniorflexdeveloper/videojs-osmf))
*   CastLabs Dash.AS plugin for OSMF ([dash.as](https://github.com/castlabs/dashas))

#### Server/Host

*   Since the requests are fired from within Flash, a crossdomain.xml file is required.*   Ability to serve byte range requests via query string (myFile.mp4?range=0-1000 || myFile.m4s?bytes=0-1000) is necessary because Flash Player restricts use of the ‘Range’ request header. Castlabs has an .htaccess file which uses mod_rewrite to achieve this. Akamai edge servers accept the bytes query string var as well.

#### Notes

*   Something we may want to modify is handling the request/response portions of the workload outside of the Flash Player similar to the Video.js HLS solution. This would remove the need for having a crossdomain.xml, which cannot be normally expected to exist in a DASH focused environment.
*   Akamai edgesuite appear to be an exception to the above rule, as those domains do in fact have the crossdomain from it’s use as a serving platform under Akamai HD. The Akamai param syntax is ‘myFile.m4s?bytes=XXXX-YYYY’.
*   In it’s current form the Dash.AS manifest parser is very rigid. We may want to look into implementing a version of the DASH.JS manifest parsing on the AS side, as it is a lot more flexible in terms of structure recognition.
*   Deeper class inspection shows that the Dash.AS plugin is based on using Netstream in data generation mode, similar to our HLS solution. There may be better way to share the codebase between the two to reduce code duplication.

### DASH.JS Requirements

#### Player

*   Environment must support [Media Source Extensions](https://developer.mozilla.org/en-US/docs/Web/API/MediaSource)
*   DASH Industry Forum DASH.JS library ([dash.js](https://github.com/Dash-Industry-Forum/dash.js))
*   Video.js Dash.js Tech ([videojs-tech-dashjs](https://github.com/Dash-Industry-Forum/dash.js/blob/development/contrib/videojs/videojs-tech-dashjs.js))

#### Server/Host

*   Open CORS headers: Access-Control-Allow-Origin: *
*   Accept use of the Range request header: Access-Control-Allow-Headers:Range, Options

#### Notes

*   Dash.js MPD parsing is significantly more robust in comparison to the Dash.AS solution.
*   Most streams tested are Akamai based, we should probably try more local and non-Akamai hosted options moving forward.*   In my testing I did see a Youtube DASH/MSE example, and those streams were confirmed to work within Dash.JS as well.
*   Dash.JS streaming lifecycle and segment loading lifecycle tend to be directly coupled and don’t necessarily fire the element media events at the time expected. For example is duration. While known at the parse complete stage of the manifest load cycle, is not reported to the player until the first of the segments is received.

### Tech Compatibility

The independant techs work well together. A slight modification had to be made to the Dash.AS library to insure that it only checked for resources which 1\. had a URL and 2\. url contained the file extension of either ‘mpd’ or ‘m4s’ for DASH manifests/segments.

**NOTE:** Dash.JS tech should be loaded into DOM prior to OSMF tech to insure the OSMF tech is the fallback scenario for DASH playback.
![](http://feeds.feedburner.com/~r/video-js/~4/Wn1NGaDuYmo)
