---
title: New Player Skin Designer for Video.js
tags:
  - html5 video
author:
  name: Steve Heffernan
  github: heff
alias:
  - post/55553002104/new-player-skin-designer-for-videojs/index.html
  - post/55553002104/index.html
date: 2013-07-15 19:44:00
---

[![](http://65.media.tumblr.com/e1ce78f8543d5e1fffd65e35cfd41b3e/tumblr_inline_mq1y4zEiqe1qz4rgp.png)](http://designer.videojs.com "Video Player Skin Designer")

Last week Brightcove had an internal hack week where everyone could work on any project they wanted. One of the projects that came out of that was a new [video.js skin designer](http://designer.videojs.com "Video Player Skin Designer").

The designer allows you to watch changes happen to the skin live as you edit the CSS, making it easier to create a custom look.

Check out [this familiar looking example](http://codepen.io/heff/pen/wtrHL) that was done in just a few minutes.

Try creating your own and let us know what you think. Better yet, create your own, share it on CodePen.io and post a link in the comments. (It&rsquo;s probably easiest if you start by forking [this unedited example](http://codepen.io/heff/pen/EarCt "Video.js Default Skin").)

One of my favorite things about video.js is that the skins are built in HTML and CSS, while working across both HTML5 _and_ Flash video. I think this designer does a nice job of showing off how easy that makes it to customize a player&rsquo;s skin.

**Some notes on how we built it&hellip;**

As a starting point we used Brian Frichette&rsquo;s awesome [LESS2CSS](https://github.com/brian-frichette/less-preview/), which gave us a huge head start. Brian has offered to help with the skin designer as well, so that&rsquo;s great!

We haven&rsquo;t added a CSS preprocessor to video.js before because we didn&rsquo;t want the extra layer of abstraction, or the extra step in the build process. When looking at the CSS in the new designer however, it became clear how valuable things like variables can be for helping people understand what&rsquo;s happening in the CSS. Still, we&rsquo;re trying to find the balance between using LESS features and keeping the CSS easily readable by anyone who just knows CSS. That means avoiding some of the more advanced LESS features like conditional statements (though we do use one for big play button positioning).

We chose to use [LESS](http://lesscss.org) because of the ability to parse the LESS markup in javascript in the browser. I&rsquo;m not aware of any up-to-date in-browser [SASS](http://sass-lang.com) parsers. The completeness of LESS2CSS also influenced that decision. We&rsquo;re using a small enough subset of features that it doesn&rsquo;t really matter which one we use otherwise, though I do like the idea of using $ for variables over @.

It&rsquo;s hosted on [Nodejitsu](https://www.nodejitsu.com), and we&rsquo;re taking advantage of their free hosting for open source. I have to say, it was pretty simple to get the app deployed with their command line tool.

Let us know if you have any thoughts. The code for the designer can be found here: [https://github.com/videojs/designer](https://github.com/videojs/designer)

Cheers,
-heff
![](http://feeds.feedburner.com/~r/video-js/~4/-k5HhU559kM)
