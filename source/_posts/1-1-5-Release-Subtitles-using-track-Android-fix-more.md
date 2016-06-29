---
title: '1.1.5 Release - Subtitles using track, Android fix & more'
tags:
  - version
author:
  name: Steve Heffernan
  github: heff
date: 2010-11-09 00:00:00
---

*   Feature: Switched to track method for setting subtitles. Now works like spec.
*   Feature: Created &ldquo;players&rdquo; concept for defining fallbacks and fallback order
*   Fix: Android playback bug.
*   Fix: Massive reorganization of code to make easier to navigate

I&rsquo;ve switched the subtitles to use the new track element defined in the [HTML5 spec](http://www.whatwg.org/specs/web-apps/current-work/multipage/video.html). You can now add subtitles by creating a track element pointing to your [WebSRT](http://www.delphiki.com/websrt/)subtitles source.

<pre>&lt;video ...&gt;
  &lt;track kind="subtitles" src="../demo-subtitles.srt" srclang="en-US" label="English"&gt;&lt;/track&gt;
&lt;/vide&gt;</pre>

The closing track tag is needed, otherwise Safari thinks everything else is a child of the track, even with a self-closing track tag. Not sure why that is, but it&rsquo;s kind of annoying.

Also a fix for Android playback was added. Android HTML5 video is pretty rough. The canPlayType function returns nothing on Android so VideoJS has a check to see if the source is mp4/m4v, and assumes it&rsquo;ll play. Then VideoJS adds a click event to the video so it&rsquo;ll play when you touch it. Also the Android will show the poster image, but no indication that it&rsquo;s a video and not just an image. Hopefully this will be improved in the next Android version.

Beyond that, I did a massive reorganization of the code, so it should be easier to navigate if you&rsquo;re planning to hack at it or contribute.

[Download version 1.1.5](http://videojs.com/downloads/video-js-1.1.5.zip)
