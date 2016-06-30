---
title: 'Video.js 4.2.0 released! RTMP, CSS designer, and stability'
tags:
  - version
author:
  name: Steve Heffernan
  github: heff
alias:
  - post/60471080014/videojs-420-released-rtmp-css-designer-and/index.html
  - post/60471080014/index.html
date: 2013-09-06 15:51:00
---

Happy September! The 4.2.0 release of Video.js has a few interesting updates, and a bunch of stability and polish.

## RTMP Support

First of all, thanks to an impressive collaboration of community members, we now have RTMP support (in _beta_). [Check out the example](http://jsbin.com/cayake).

It&rsquo;s still pretty basic support for RTMP, but we think it will cover a lot of the general use cases. The feature support includes:

*   Single stream (no client-side adaptive support)
*   Flash only, HTML5 video doesn&rsquo;t support RTMP (but HLS is supported on iOS devices)
*   On-demand only. We haven&rsquo;t updated the UI to support live yet.

To load an RTMP stream in a Video.js player, you&rsquo;ll use a source tag in the same way you would other source types:

<pre class="prettify" style="word-wrap: break-word;">
&lt;source src="rtmp://your.streaming.provider.net/cfx/st/&amp;mp4:path/to/video.mp4" type="rtmp/mp4"&gt;
</pre>

The connection and stream parts are determined by splitting the URL on the first ampersand (&amp;) or the last slash (/).

<pre class="prettify">
[http://myurl.com/streaming&amp;/is/fun](http://myurl.com/streaming&amp;/is/fun) --&gt;
  connection: [http://myurl.com/streaming](http://myurl.com/streaming)
  stream: /is/fun

-or-

[http://myurl.com/streaming/is/fun](http://myurl.com/streaming/is/fun) --&gt;
  connection: [http://myurl.com/streaming/is](http://myurl.com/streaming/is)
  stream: fun
</pre>

The available source types include `rtmp/mp4` or `rtmp/flv`.

RTMP has been a much requested feature over the years and it&rsquo;s great to finally have it in the player. Thanks to everyone involved in that work.

## Player Skin Designer

If you missed the [previous blog post](http://blog.videojs.com/post/55553002104/new-player-skin-designer-for-video-js), be sure to check out the [new interface for designing the player skin](http://designer.videojs.com). It really shows off the customizability of the video.js controls, which are built completely in HTML and CSS.

With the 4.2 release the styles in the designer have been brought up-to-date with the latest player styles.

## Control Bar Updates

Also in [a previous post](http://blog.videojs.com/post/57828375480/hiding-and-showing-video-player-controls), I described a number of updates that were made to the control bar to fix cross browser/device issues and improve the overall functionality. As of 4.2.0 all of those updates have made it into the stable release.

## Other Updates

Along with previous updates there&rsquo;s been a number of patches and enhancements along the way. Here&rsquo;s a full list:

*   Added LESS as a CSS preprocessor for the default skin ([view](https://github.com/videojs/video.js/pull/644))
*   Exported MenuButtons for use in the API ([view](https://github.com/videojs/video.js/pull/648))
*   Fixed ability to remove listeners added with one() ([view](https://github.com/videojs/video.js/pull/659))
*   Updated buffered() to account for multiple loaded ranges ([view](https://github.com/videojs/video.js/pull/643))
*   Exported createItems() for custom menus ([view](https://github.com/videojs/video.js/pull/654))
*   Preventing media events from bubbling up the DOM ([view](https://github.com/videojs/video.js/pull/630))
*   Major reworking of the control bar and many issues fixed ([view](https://github.com/videojs/video.js/pull/672))
*   Fixed an issue with minifiying the code on Windows systems ([view](https://github.com/videojs/video.js/pull/683))
*   Added support for RTMP streaming through Flash ([view](https://github.com/videojs/video.js/pull/605))
*   Made tech.features available to external techs ([view](https://github.com/videojs/video.js/pull/705))
*   Minor code improvements ([view](https://github.com/videojs/video.js/pull/706))
*   Updated time formatting to support NaN and Infinity ([view](https://github.com/videojs/video.js/pull/627))
*   Fixed an `undefined` error in cases where no tech is loaded ([view](https://github.com/videojs/video.js/pull/632))
*   Exported addClass and removeClass for player components ([view](https://github.com/videojs/video.js/pull/661))
*   Made the fallback message customizable ([view](https://github.com/videojs/video.js/pull/638))
*   Fixed an issue with the loading spinner placement and rotation ([view](https://github.com/videojs/video.js/pull/694))
*   Fixed an issue with fonts being flaky in IE8

The latest version can be found on [videojs.com](http://www.videojs.com) through the download link or the CDN hosted version.

Cheers,

-heff
![](http://feeds.feedburner.com/~r/video-js/~4/V_IGILu3P_0)
