---
title: Version 3.1 Update
tags:
  - version
author:
  name: Steve Heffernan
  github: heff
alias:
  - post/35884369527/version-3-1-update/index.html
  - post/35884369527/index.html
date: 2012-01-30 00:00:00
---

This is the first release since the initial 3.0 launch, aside from some hotfixes that went out immediately. It includes a number of fixes for things that users in the [forums](http://help.videojs.com) found right off the bat.

One feature that&rsquo;s optional for testing in this release is iFrame Mode for Flash. One the of the unique things about Video.js is we haven&rsquo;t built any controls into our Flash player, and instead use HTML and CSS to create the controls for the Flash side as well. This keeps the experience consistent and the Flash player very lightweight, however there&rsquo;s a number of issues that you run into with Flash when you take an approach like this. If you&rsquo;ve ever tried to resize the parent of a Flash object, or hide a Flash object and then show it again, you&rsquo;ve probably run into the issue of Flash reloading in Firefox. This is a bug that&rsquo;s been in Firefox for [quite a long time](https://bugzilla.mozilla.org/show_bug.cgi?id=90268) however it looks like it might be fixed by version 13 (currently 9). To add on top of this, with the new [browser fullscreen API](https://wiki.mozilla.org/Gecko:FullScreenAPI), the other browsers now also reload Flash when you go to native fullscreen.

We&rsquo;ve found a bit of a fix where if you embed the Flash object in an iframe first, it can get around the reloading in some of these cases. So in the new version there&rsquo;s an option to turn this on and try it.

[https://gist.github.com/4092929.js?file=iframe-mode.js](https://gist.github.com/4092929.js?file=iframe-mode.js)

We&rsquo;ll be doing some more testing to make sure it&rsquo;s stable before we push it out to everyone.

Here&rsquo;s the full change log for the release.

#### 3.1.0 / 2012-01-30 / leonardo

*   Added CSS fix for Firefox 9 fullscreen (in the rare case that it&rsquo;s enabled)
*   Replaced swfobject with custom embed to save file size.
*   Added flash iframe-mode, an experimental method for getting around flash reloading issues.
*   Fixed issue with volume knob position. Improved controls fading.
*   Fixed ian issue with triggering fullscreen a second time.
*   Fixed issue with getting attributes in Firefox 3.0
*   Escaping special characters in source URL for Flash
*   Added a check for if Firefox is enabled which fixes a Firefox 9 issue
*   Stopped spinner from showing on &lsquo;stalled&rsquo; events since browsers sometimes don&rsquo;t show that they&rsquo;ve recovered.
*   Fixed CDN Version which was breaking dev.html
*   Made full-window mode more independent
*   Added rakefile for release generation

Cheers,
 -Heff
