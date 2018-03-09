---
title: Video.js 7 Roadmap
tags:
  - IE8
  - IE
  - google analytics
  - VHS
  - HLS
  - DASH
  - webpack
  - rollup
author:
  name: Gary Katsevman
  github: gkatsev
date: 2018-03-09 16:33:56
---

![](woodland-road-falling-leaf-natural-38537.jpeg)

Today, I’m excited to announce the roadmap to Video.js 7! While this is a major version update, there is very little that is actually breaking. The two main changes are the addition of videojs-http-streaming, VHS for short, and the removal of support for older versions of Internet Explorer.

![VJS 7](vjs 7 small.png)

## Video.js HTTP Streaming

![](VHS small.png)

VHS used to be videojs-contrib-hls, but as MPEG-DASH became more popular, we realized that we wanted to support it as well and that we can share a lot of the code between HLS and DASH implementations. Look for more details in an announcement post for VHS soon!
VHS will ship with Video.js by default because one of the guiding principles of Video.js is to make it easy for users to just put it on the page and have a player that works across browsers. With HLS and DASH becoming so popular, we thought it was about time for a plug and playable Video.js for the most common streaming formats.
In addition to having it be included by default, we’ll make sure to provide builds that exclude VHS for those that don’t need it or are using another playback engine like HLS.js.

## Old IE

![IE8](IE8 small.png)

Video.js has a long history of making best effort to support IE, starting with the Flash fallback originally created for IE8. When Video.js 5 was released, we had a lot of code in place to support IE8 and when Video.js 6 came around, with Flash on its way out, we moved Flash support to a separate project, videojs-flash. Now, based on usage data, we are planning to remove support for IE8, IE9, and IE10.
According to the data we’ve gathered, IE in total has a share of about 4%. Of those 4%, IE8, IE9, and IE10 combined make up barely 5%, and IE11 has around 91% usage. This means that combined IE8, IE9, and IE10 are barely 0.002% of the total usage of Video.js. Based on this incredibly small footprint, we believe it isn't worth the substantial effort required to maintain support for these browsers. The good news is, for those concerned, Video.js 6 isn’t going away anytime soon and will be available for your old IE support needs.
In addition to the statistics, our tests currently take between 5 and 10 minutes to run on IE8. Removing this will allow us to greatly decrease the duration of our test suite. Even worse, sometimes the tests timeout and retry for up to 40 minutes but when they restart they pass on the first try. Not having to deal with these types of issues will allow us to increase our testing infrastructure and provide a better product.

{% pullquote right %}
![](browserstack logo small.png)
Thanks [Browserstack](https://www.browserstack.com/) for sponsoring browser-based testing for Video.js!
{% endpullquote %}

## Video.js 5.x

When Video.js 7 is released it’ll be the end-of-life for Video.js 5.x from a support perspective. Of course, we are not going to unpublish this version from npm or remove the files from the CDN, so you are free to continue using it. However, we urge you to update at your earliest convenience as we will no longer be accepting fixes.

## Google Analytics

{% pullquote right %}
![](fastly small.png)
Thanks [Fastly](https://www.fastly.com/) for sponsoring vjs.zencdn.net
{% endpullquote %}

Video.js provides CDN hosted files for Video.js, sponsored by [Fastly](https://www.fastly.com/) -- thanks Fastly! These files currently have a stripped down pixel tracking for Google Analytics (GA) that sends limited information to a GA account we own. We did a terrible job communicating that this is happening and how users can opt out -- set window.HELP_IMPROVE_VIDEOJS to false before loading in Video.js -- or use another CDN like unpkg or CDNJS. Recently we made a change that makes this tracking pixel honor the Do Not Track flag set by users in the browser, however, this isn’t going to affect previously released versions of Video.js
![](remove GA.png)
In addition, starting with Video.js 7.0 (and probably back ported to newer versions of 6.x) we will *no longer* include this tracking pixel in our CDN builds. Instead, we are investigating our options for using the CDN logs via Fastly. We expect to share more details as we get closer to the Video.js 7 release date and have a better idea of what we will be doing.

## Build Tools

![](rollup to webpack.png)

Currently, Video.js uses rollup to combine all the Video.js files into two files for different build systems. One that excludes dependencies, for use in bundlers, and another that includes dependencies for a UMD file that can be used on a page as a `<script>`. We are looking to transition our build and test tooling from rollup and browserify to Webpack 4.0 to provide more flexibility in our builds and to do a better job at being able to build with VHS (as many people remember [the infamous issue 600 on the contrib-hls repo](https://github.com/videojs/videojs-contrib-hls/issues/600)).
In addition, we’ll be looking into allowing custom builds when bundling Video.js. For example, if one doesn’t want VHS or doesn’t want visible controls, a surprisingly large part of the player, they can set `VJS_VHS=false` `VJS_CONTROLS=false` when they’re bundling their application to keep these pieces out of the build.

## Timeline

We are still working on a lot of what was mentioned above but our hope is to have the first pre-release of 7.0 before the end of March.

We’re pretty excited about this not-so-major update and hope you will be too!

