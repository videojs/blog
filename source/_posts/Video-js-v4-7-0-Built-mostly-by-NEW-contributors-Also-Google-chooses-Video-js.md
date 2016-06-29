---
title: >-
  Video.js v4.7.0 - Built mostly by NEW contributors! Also Google chooses
  Video.js
tags:
  - news
  - version
author:
  name: Steve Heffernan
  github: heff
date: 2014-08-06 14:42:00
---

We&rsquo;re continuing to work hard on improving the contributor experience around the Video.js project and it&rsquo;s paying off. Over half of the changelog is thanks to brand new contributors! Issues and pull requests are getting addressed faster than ever, and I was even allowed to give [a talk at OSCON](http://www.oscon.com/oscon2014/public/schedule/speaker/104522) on some of the strategies we&rsquo;re using. If you&rsquo;re instersted in getting involved, join the #videojs IRC room or post an issue to let us know.

## Google Chooses Video.js for Google Media Framework

Google recently [announced a new framework](http://googleadsdeveloper.blogspot.com/2014/07/google-media-framework-making-online.html) for building video experiences and monetization. There are versions of the framework for native iOS and Android apps, and for the browser they chose to use Video.js. Check out [their video.js plugin](https://github.com/googleads/videojs-ima), and as it says in their announcement, &ldquo;Stay tuned as well for a deeper dive into Video.js with IMA soon!&rdquo;

## Localization

In this release we&rsquo;ve built the infrastructure for displaying text in other languages. Examples of text include error messages and text used for accessibility. This feature can extend to plugins as well.

Today you can include other languages by including the JSON translations object from the language you want with the player, like in this example for Spanish (es).

`videojs.options.languages['es'] = { [translations object] }`

You can find translations files in [the lang folder](https://github.com/videojs/video.js/tree/master/lang) of the project. We don&rsquo;t have many translations yet, but we&rsquo;re looking for translators if you&rsquo;d like to help!

## Multiple buffered regions

With HTML5 video you can skip ahead in the video and the browser will start downloading the part of the file needed for the new position, which is different from how Flash video works by default. Flash will download from the start to the end of the file so you can only skip ahead once it has download that part of the video.

In the HTML5 video API we&rsquo;re given the `buffered` property which returns a list of time ranges that the browser has downloaded data for. Early on in HTML5 video, browsers only ever reported one time range, but now we have a direct view of what&rsquo;s been downloaded.

In the newest version of the video.js skin you can see the specific regions.

![](http://67.media.tumblr.com/1420d1b8ee1c54b4eb876b95a5417fd7/tumblr_inline_n9usu442h91qzc111.png)

We&rsquo;ve kept it subtle so it&rsquo;s not too big of a change. We&rsquo;d love to hear your thoughts on it.

## DASH Everywhere-ish

If you haven&rsquo;t seen it yet, check out [the post](http://blog.videojs.com/post/92536319027/dash-everywhere-ish-hack-project) on Tom Johson&rsquo;s work getting DASH supported in Video.js, using Flash or the new Media Source Extensions. MPEG-DASH is an adaptive streaming format that Netflix and YouTube are using to stream video to cutting-edge browsers. It has the potential to replace Apple&rsquo;s HTTP Live Streaming format as the main format used for adaptive streaming.

## Video.js on Conan!

Conan O'Brien&rsquo;s TeamCoco site is using Video.js with a nicely customized skin and ads integration. [Check it out!](http://teamcoco.com)

[![](http://65.media.tumblr.com/383082cd857bce0c6abc825e0f4878a8/tumblr_inline_n9utibJ00S1qzc111.png)](http://teamcoco.com/video/conan-dave-franco-tinder-remote)

## New Skin by Cabin

The team at [Cabin](http://madebycabin.com) put together a simple and clean [new skin for video.js](https://github.com/cabin/videojs-sublime-skin).

![](http://67.media.tumblr.com/484bf82546666982023e64d49d4a6096/tumblr_inline_n9usohnTgG1qzc111.png)

## New Plugins

A lot of great new plugins have been released!

*   [videojs-ima](https://github.com/googleads/videojs-ima): Easily integrate the Google IMA SDK into Video.js to enable advertising on your video content.
*   [videojs-brightcoveAnyaltics](https://github.com/space87/videojs-BrightCove-tracking): Allow tracking of views/impressions &amp; engagement data in videojs for Brightcove videos
*   [videojs-logobrand](https://github.com/Mewte/videojs-logobrand): Add a logo/brand image to the player that appears/disappears with the controls. (also useful as a basic plugin template for learning how Video.JS plugins work.)
*   [videojs-seek](https://github.com/aervans/videojs-seek): Seeks to a specific time point specified by a query string parameter.
*   [videojs-preroll](https://github.com/dirkjanm/videojs-preroll): Simple preroll plugin that displays an advertisement before the main video
*   [videojs-framebyframe](https://github.com/erasche/videojs-framebyframe): Adds buttons for stepping through a video frame by frame
*   [videojs-loopbutton](https://github.com/CharlotteDunois/videojs-loopbutton): Adds a loop button to the player
*   [videojs-ABdm](https://github.com/Catofes/videojsABdm): Use CommentCoreLibrary to show comments (which is called as DanMu) during playing.
*   [videojs-hotkeys](https://github.com/ctd1500/videojs-hotkeys): A plugin for Video.js that enables keyboard hotkeys when the player has focus.

## New Release Schedule

As part of improving the contributor experience we&rsquo;re moving to scheduled releases. We&rsquo;ll now put out a release every other Tuesday as long as there&rsquo;s new changes to release. This will help give everyone a better idea of when specific features and fixes will become available.

## Full list from the change log

*   Added cross-browser isArray for cross-frame support. fixes #1195 ([view](https://github.com/videojs/video.js/pull/1218))
*   Fixed support for webvtt chapters. Fixes #676\. ([view](https://github.com/videojs/video.js/pull/1221))
*   Fixed issues around webvtt cue time parsing. Fixed #877, fixed #183\. ([view](https://github.com/videojs/video.js/pull/1236))
*   Fixed an IE11 issue where clicking on the video wouldn&rsquo;t show the controls ([view](https://github.com/videojs/video.js/pull/1291))
*   Added a composer.json for PHP packages ([view](https://github.com/videojs/video.js/pull/1241))
*   Exposed the vertical option for slider controls ([view](https://github.com/videojs/video.js/pull/1303))
*   Fixed an error when disposing a tech using manual timeupdates ([view](https://github.com/videojs/video.js/pull/1312))
*   Exported missing Player API methods (remainingTime, supportsFullScreen, enterFullWindow, exitFullWindow, preload) ([view](https://github.com/videojs/video.js/pull/1328))
*   Added a base for running saucelabs tests from grunt ([view](https://github.com/videojs/video.js/pull/1215))
*   Added additional browsers for saucelabs testing ([view](https://github.com/videojs/video.js/pull/1216))
*   Added support for listening to multiple events through a types array ([view](https://github.com/videojs/video.js/pull/1231))
*   Exported the vertical option for the volume slider ([view](https://github.com/videojs/video.js/pull/1378))
*   Fixed Component trigger function arguments and docs ([view](https://github.com/videojs/video.js/pull/1310))
*   Now copying all attributes from the original video tag to the generated video element ([view](https://github.com/videojs/video.js/pull/1321))
*   Added files to be ignored in the bower.json ([view](https://github.com/videojs/video.js/pull/1337))
*   Fixed an error that could happen if Flash was diposed before the ready callback was fired ([view](https://github.com/videojs/video.js/pull/1340))
*   The up and down arrows can now be used to control sliders in addition to left and right ([view](https://github.com/videojs/video.js/pull/1345))
*   Added a player.currentType() function to get the MIME type of the current source ([view](https://github.com/videojs/video.js/pull/1320))
*   Fixed a potential conflict with other event listener shims ([view](https://github.com/videojs/video.js/pull/1363))
*   Added support for multiple time ranges in the load progress bar ([view](https://github.com/videojs/video.js/pull/1253))
*   Added vjs-waiting and vjs-seeking css classnames and updated the spinner to use them ([view](https://github.com/videojs/video.js/pull/1351))
*   Now restoring the original video tag attributes on a tech change to support webkit-playsinline ([view](https://github.com/videojs/video.js/pull/1369))
*   Fixed an issue where the user was unable to scroll/zoom page if touching the video ([view](https://github.com/videojs/video.js/pull/1373))
*   Added &ldquo;sliding&rdquo; class for when slider is sliding to help with handle styling ([view](https://github.com/videojs/video.js/pull/1385))

Cheers,

-heff

[Discuss on Twitter](https://twitter.com/videojs/status/497112744500285440) | [Discuss on Hacker News](https://news.ycombinator.com/item?id=8144578)
![](http://feeds.feedburner.com/~r/video-js/~4/xPnVQ6QJIk0)
