---
title: Version 3.2 Update
tags:
  - version
author:
  name: Steve Heffernan
  github: heff
alias:
  - post/35883671470/version-3-2-update/index.html
  - post/35883671470/index.html
date: 2012-03-23 00:00:00
---

First of all, check out the new [video tag builder](/tag-builder/). The previous version of this site had an embed code builder, and people were [pretty disappointed](http://help.videojs.com/discussions/problems/873-whered-the-embed-builder-go) that the new site didn&rsquo;t. So I&rsquo;m happy to announce that it&rsquo;s available again, now as an HTML5 video tag builder that could probably be used outside of Video.js, it includes track tags, and even lets you test the settings. Let me know if you have any feedback on it.

The most notable change in this release is probably the completely overhauled &lt;track&gt; tag support.

[https://gist.github.com/4092295.js?file=html5-track-tag.html](https://gist.github.com/4092295.js?file=html5-track-tag.html)

The new version supports subtitles, captions, and even chapters. When you include tracks of different kinds, Video.js will automatically create menus in the player where users can select the language to display, or which chapter to jump to. Video.js also now supports the new [WebVTT](http://dev.w3.org/html5/webvtt/) text format, which is not far off from the previously supported WebSRT format but did take some tweaking in the parser. Not all WebVTT placement features are supported yet, but the basics of displaying text are, and we&rsquo;ll be working to get more WebVTT features built in.

Additionally there was work done to make some API methods accessible earlier. For any method that isn&rsquo;t a getter (returns a specific value from the player), if you call the method before the playback technology (HTML5/Flash) is ready, it will now cache the call until it is ready. So where previously you might have had to wait for the ready callback:

[https://gist.github.com/4092295.js?file=wait-for-ready.js](https://gist.github.com/4092295.js?file=wait-for-ready.js)

You could now do:

[https://gist.github.com/4092295.js?file=no-wait-for-ready.js](https://gist.github.com/4092295.js?file=no-wait-for-ready.js)

Again, that&rsquo;s just for methods where you are setting a value or triggering an action. If you try to get a value back like `myPlayer.duration()`, you&rsquo;ll get nothing until the player is ready.

One other feature that was requested in the forums was automatically translating relative video URLs to absolute URLs for the flash fallback. This was an issue with the CDN hosted version which involved loading a remote SWF file which didn&rsquo;t have the same context as the player. Before we would just tell people to use full URLs (http://) but that shouldn&rsquo;t be an issue anymore.

Thanks to everyone that&rsquo;s helped contribute code lately, and apologies for any long response times in the forums as I continue to try to push out code.

Here&rsquo;s the full change log for the release.

### 3.2.0 / 2012-03-20 / baxter

*   Updated docs with more options.
*   Overhauled HTML5 Track support.
*   Fixed Flash always autoplaying when setting source.
*   Fixed localStorage context
*   Updated &lsquo;fullscreenchange&rsquo; event to be called even if the user presses escape to exit fullscreen.
*   Automatically converting source URL to absolute for Flash fallback.
*   Created new 'loadedalldata&rsquo; event for when the source is completely downloaded
*   Improved player.destroy(). Now removes elements and references.
*   Refactored API to be more immediately available.

Cheers,
 -Heff
