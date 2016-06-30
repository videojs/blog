---
title: Make sites serve you HTML5 video in Safari
tags:
  - code
author:
  name: Steve Heffernan
  github: heff
alias:
  - post/35887203978/make-sites-serve-you-html5-video-in-safari/index.html
  - post/35887203978/index.html
date: 2010-11-19 00:00:00
---

John Gruber of Daring Fireball has a cool post on [tricking websites to display HTML5 video](http://daringfireball.net/2010/11/masquerading_as_mobile_safari) when they say they can only play Flash. Many sites will tell you they require Flash, even when they actually support [HTML5 video](http://videojs.com/html5-video/) for iOS devices.

To add to Gruber&rsquo;s post, there&rsquo;s two reasons a site might do this:

1.  They&rsquo;re trying to protect their content from being easily downloaded, which HTML5 video can&rsquo;t do very well yet.
2.  They assume everyone with a browser has Flash, and just serve HTML5 video to iOS devices.

So for this first, this is actually a hack to download content some publishers might not want you to. I probably shouldn&rsquo;t be publishing this since security of the content is one of the main reasons HTML5 video will still take time to spread. But for that purpose, it&rsquo;s really not hard to figure out.

For the second, it&rsquo;s kind of lazy implementation of fallbacks. It&rsquo;s pretty easy to check if Flash is supported, and if not, fall back to HTML5, instead of checking for a specific user agent string. So for sites using VideoJS, this method wouldn&rsquo;t work, but there also wouldn&rsquo;t be a need because VideoJS would fallback appropriately to HTML5.

Gruber has disabled Flash in Safari for better performance with video. That says a lot. Most people probably won&rsquo;t go to same effort he has, but I know he&rsquo;s not the only one.
