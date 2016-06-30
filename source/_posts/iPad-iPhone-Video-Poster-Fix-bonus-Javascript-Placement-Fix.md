---
title: iPad & iPhone Video Poster Fix (bonus Javascript Placement Fix)
tags:
  - code
author:
  name: Steve Heffernan
  github: heff
alias:
  - post/35888138568/ipad-iphone-video-poster-fix-bonus-javascript-placement/index.html
  - post/35888138568/index.html
date: 2010-09-20 00:00:00
---

### Bug #1 - Poster Attribute

If you include the poster attribute on the video tag when you&rsquo;re using &lt;source&gt; tags, the video won&rsquo;t work on iPads &amp; iPhones using iOS 3\. You&rsquo;ll see a broken play button, or no play button at all. On the iPad specifically, playback in inconsistent. Sometimes it&rsquo;ll work and sometimes it won&rsquo;t. This is [documented on the Video for Everybody site](http://camendesign.com/code/video_for_everybody#notes).

### Bug #2 - Javascript in the Head

This is a fun one&hellip; If you include Javascript in the head of your page, it&rsquo;ll break playback on the iPad (also inconsistent). If you move the Javascript to the bottom of the page, and still include a stylesheet, the iPad will work, but the iPhone 3 won&rsquo;t. I first read about this in a [post on the No.inc blog](http://blog.noinc.com/2010/05/13/html5-video-tag-iphone-ipad-ihaveheadache), and then ran into it myself when redesigning the VideoJS site. Their original solution was to put the JS at the bottom of the page for iPads only (fun).

Apple&rsquo;s iOS4 seems to fix both of these problems on the iPhone, but until iOS4 is available on the iPad, and everyone in the world upgrades their devices, we have to deal with this.

### Fix for iOS 3

The problem seems to be some kind of race condition, that trips up the devices. The solution that has fixed both of these for me is to add the playable source directly to the video tag, and tell the video to load (all through Javascript).

I&rsquo;ve added all this to VideoJS 1.1.2, but here&rsquo;s the basics of how it works.

<pre>var video = document.getElementById("your_video");
    var children = video.children;
    for (var i=0,j=children.length; i&lt;j; i++) {
      if (children[i].tagName.toUpperCase() == "SOURCE") {
        var canPlay = video.canPlayType(children[i].type);
        if(canPlay == "probably" || canPlay == "maybe") {
          video.src = children[i].src;
          video.load();
          break; // or return or whatever
        }
      }
    }</pre>

So loop through the source elements, find the one that&rsquo;s compatible, and add that source to the video src. Then trigger the video&rsquo;s load() function.

That seems to fix both issues. **This will not make the poster show up in either device.**It just makes makes the video playable. Sometimes you&rsquo;ll see it flash the controls and then go back to the big play button.

Any feedback is appreciated.

Let&rsquo;s go Apple, get iOS4 out to everybody!
