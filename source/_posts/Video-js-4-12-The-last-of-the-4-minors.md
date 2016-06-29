---
title: Video.js 4.12 - The last of the 4 minors
tags:
  - news
  - version
author:
  name: Matthew McClure
  github: mmcc
date: 2015-02-18 13:32:00
---

## Much improved WebVTT support

We added basic WebVTT support really early on, which we were pretty proud of. As browsers added support, however, our shim started to lag a bit, or at the very least, cause issues with browsers that required native support (here’s looking at you, iOS).

So, we’re happy to announce that our text track support is vastly improved with this version! Instead of continuing to try and build out our own WebVTT parser, we’ve pulled in Mozilla’s fantastic work, [VTT.js](https://github.com/mozilla/vtt.js). This is the WebVTT implementation used in Firefox / Gecko, so we’re confident that this is a substantial leap forward for Video.js and our accessibility efforts.

## Don’t use captions? Feeling disappointed?

You shouldn’t feel disappointed, this is a great day! This release includes a lot of fixes, and although fixes are always exciting to the people affected, other people can feel a hefty dose of “meh”. We’re excited to say, however, that this release is exciting for another, very important reason.

This is the **last minor** release in the version 4 family. There might be a few patches, but from now on, master is officially version 5.0-in-waiting. Party.

![party](http://media.giphy.com/media/d89VuJ4GflFfO/giphy.gif)

#### Notable updates and fixes for 4.12

*   The hide / show methods now toggle a class (`.vjs-hidden`) instead of applying inline styles. Make sure to update your stylesheet as well!
*   A new `.vjs-scrubbing` class gets applied while, you guessed it, scrubbing so menus can be prevented from erroneously showing on hover.
*   Improved url parsing in IE9.
*   Ever wondered what version you’re using? Now there’s a VERSION key. Woot.
*   New `.vjs-ended` class that’s added when playback reaches the end of the timeline (also known as: ended).
*   Player networkState and readyState properties available across techs
*   Video.js is now exported as a named AMD module
*   Mobile scrolling improvements
*   And more! Fixes galore!

## Translation party continues

We’re now up to **19** community submitted translations! Entirely new translations in this release include:

*   Traditional Chinese
*   Vietnamese
*   Czech
*   Catalan
*   Bulgarian
*   Turkish

## Video.js in the wild

To be terribly honest, there is a level of starstruck over a few of these. Normally we just do one, but hey…

#### Social Media

*   [Instagram](http://instagram.com/p/aye9QFmB96)
*   [Twitter](https://twitter.com/ActuallyNPH/status/560049149836808192)
*   [Tumblr](http://sh1ps-testing.tumblr.com/post/111334784868/lorem-ipsum-and-stuff)

#### Global News Outlets

*   [Wall Street Journal](http://graphics.wsj.com/data-mining-of-emotions/)
*   [The Guardian](http://www.theguardian.com/film/video/2015/feb/17/whiplash-should-win-best-picture-oscars-2015-video)
*   [Bloomberg](http://www.bloomberg.com/news/videos/2015-02-13/a-harvard-mad-scientist-invented-ice-cream-that-has-skin)
*   [Dagbladet](http://www.dbtv.no/3827291177001#Dette_har_statsbudsjettet_%C3%A5_si_for_din_%C3%B8konomi)

## New plugins from the community

*   [videojs-Background](https://github.com/matthojo/videojs-Background): A plugin that allows for videos to be displayed as a full background to any element.
*   [videojs-brightcoveAnalytics](https://github.com/space87/videojs-BrightCove-tracking): Allow tracking of views/impressions &amp; engagement data in videojs for Brightcove videos
*   [videojs-caption](https://github.com/spchuang/videojs-caption): Have the most flexibility and power for displaying caption videojs
*   [videojs-transcript](https://github.com/walsh9/videojs-transcript): Display an interactive transcript from caption or subtitle tracks.
*   [videojs-autoplay-toggle](https://github.com/theonion/videojs-autoplay-toggle): adds an autoplay toggle which will persist to cookies or localstorage.
*   [videojs-offset](https://github.com/cladera/videojs-offset): Allows you to play a segment of the video.
*   [videojs-youtube-progress](https://github.com/mente/videojs-youtube-progress): Preserve progress seeker when control bar is hidden.
*   [projectorjs](https://github.com/FbF/projectorjs): A small no-dependencies JavaScript library that enables the display of overlays on native HTML5 video elements, or (optionally) video elements powered by videojs.
*   [videojs-record](https://github.com/collab-project/videojs-record): A video.js plugin for recording audio/video files.
*   [videojs-dashjs](https://github.com/Dash-Industry-Forum/dash.js/tree/v1.2.0/contrib/videojs): Loads dash.js as a tech to support MPEG-DASH videos.
*   [videojs-silverlight](https://github.com/Afterster/videojs-silverlight): Allows you to play WMV, WMA, MP4, MP3, WAV and FLAC medias through Silverlight within Video.js.
*   [videojs-vlc](https://github.com/Afterster/videojs-vlc): Allows you to play all media through VLC web plug-in within Video.js.
*   [videojs-aurora](https://github.com/Afterster/videojs-aurora): Allows you to play WAV, OGG, MP3, M4A, AAC and FLAC medias through Aurora.js within Video.js.
*   [videojs-java](https://github.com/Afterster/videojs-java): Allows you to play AIFF, AVI, GSM, MID, MPG, MP2, MOV, AU and WAV medias through Java within Video.js.

## Raw list of changes

*   @PeterDaveHello added a Traditional Chinese translation ([view](https://github.com/videojs/video.js/pull/1729))
*   @mmcc updated the hide/show functions to use a class instead of inline styles ([view](https://github.com/videojs/video.js/pull/1681))
*   @mister-ben added better handling of the additional videojs() arguments when the player is already initialized ([view](https://github.com/videojs/video.js/pull/1730))
*   @anhskohbo added a Vietnamese translation ([view](https://github.com/videojs/video.js/pull/1734))
*   @Sxmanek added a Czech translation ([view](https://github.com/videojs/video.js/pull/1739))
*   @jcaron23 added the vjs-scrubbing CSS class and prevented menus from showing while scrubbing ([view](https://github.com/videojs/video.js/pull/1741))
*   @dmlap fixed URL parsing in IE9 ([view](https://github.com/videojs/video.js/pull/1765))
*   @gkatsev Fixed issue where ManualTimeUpdatesOff was not de-registering events ([view](https://github.com/videojs/video.js/pull/1793))
*   @brycefisher Added a guide on player disposal ([view](https://github.com/videojs/video.js/pull/1803))
*   @toniher added a Catalan translation ([view](https://github.com/videojs/video.js/pull/1794))
*   @mmcc added a VERSION key to the videojs object ([view](https://github.com/videojs/video.js/pull/1798))
*   @mmcc fixed an issue with text track hiding introduced in #1681 ([view](https://github.com/videojs/video.js/pull/1804))
*   @dmlap exported video.js as a named AMD module ([view](https://github.com/videojs/video.js/pull/1844))
*   @dmlap fixed poster hiding when the loadstart event does not fire ([view](https://github.com/videojs/video.js/pull/1834))
*   @chikathreesix fixed an object delete error in Chrome ([view](https://github.com/videojs/video.js/pull/1858))
*   @steverandy fixed an issue with scrolling over the player on touch devices ([view](https://github.com/videojs/video.js/pull/1809))
*   @mmcc improved tap sensitivity ([view](https://github.com/videojs/video.js/pull/1830))
*   @mister-ben added a vjs-ended class when playback reaches the end of the timeline ([view](https://github.com/videojs/video.js/pull/1857))
*   @dmlap Add network and ready state properties ([view](https://github.com/videojs/video.js/pull/1854))
*   @woollybogger exported the hasClass function ([view](https://github.com/videojs/video.js/pull/1839))
*   @DevGavin fixed the Chinese translation ([view](https://github.com/videojs/video.js/pull/1841))
*   @iSimonWeb added font-path variable ([view](https://github.com/videojs/video.js/pull/1847))
*   @shoshomiga added a Bulgarian translation ([view](https://github.com/videojs/video.js/pull/1849))
*   @ragecub3 added a Turkish translation ([view](https://github.com/videojs/video.js/pull/1853))
*   @gkatsev greatly improved text track support and implemented vtt.js as the webvtt parser ([view](https://github.com/videojs/video.js/pull/1749))
*   @gkatsev fixed captions showing by default in Chrome and Safari ([view](https://github.com/videojs/video.js/pull/1865))
*   @mister-ben fixed a woff warning in Firefox ([view](https://github.com/videojs/video.js/pull/1870))![](http://feeds.feedburner.com/~r/video-js/~4/RLR00vEFKVQ)
