---
title: Version 1.1.4 Release - CSS Loading Spinner & More
tags:
  - version
author:
  name: Steve Heffernan
  github: heff
date: 2010-11-06 00:00:00
---

The most notable update in this version is a loading indicator (spinner), for when the video is buffering or seeking. The spinner works pretty well, however it&rsquo;s limited by how accurately each browser tells us what it&rsquo;s currently doing through triggered events. The spinner works best in Firefox so far. Safari/Chrome are less consistent with what events are triggered and when. For this reason I&rsquo;m using a more careful approach that makes sure the spinner gets hidden, as opposed to making sure the spinner always shows when the player is busy. There&rsquo;s a few bugs I had to account for too, like when Safari throws a &ldquo;waiting&rdquo; event, but then no other event to let us know it&rsquo;s not waiting anymore.

The spinner icon is made with CSS3 effects (border-radius and transform). The spinning animation is created with a little javascript for now. In webkit browsers the animation could have been done with CSS3 animation, but I&rsquo;d rather keep it consistent between browsers. The technique comes from [Kilian Valkhof](http://kilianvalkhof.com/2010/css-xhtml/css3-loading-spinners-without-images/) with initial integration into VideoJS by [Janez Troha](http://twitter.com/#!/dz0ny) ([dz0ny](https://github.com/dz0ny))

Other features and fixes include:

*   Feature: Added loading spinner.
*   Feature: Improved styles loaded checking.
*   Feature: Added volume() function to get and set volume through the player. - Fix: Fix issue where FF would loop video in background when ended.
*   Fix: Bug in Chrome that shows poster &amp; plays audio if you set currentTime too quickly.
*   Fix: Bug in Safari where waiting is triggered and shows spinner when not needed
*   Fix: Updated to show links if only unplayable sources and no Flash.
*   Fix: Issue where if play button was loaded after play, it wouldn&rsquo;t hide.

[Download version 1.1.4](http://videojs.com/downloads/video-js-1.1.4.zip)
