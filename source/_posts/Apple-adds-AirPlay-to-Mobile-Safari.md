---
title: Apple adds AirPlay to Mobile Safari
tags:
  - news
  - code
author:
  name: Steve Heffernan
  github: heff
date: 2011-01-28 00:00:00
---

AirPlay is a video feature on iOS devices that lets you play video from your device on your TV, by streaming it to an Apple TV on the same wireless network. Before now this was only available in the iPod and YouTube apps, but with iOS 4.3 (now in beta) it will be available through Mobile Safari as well.

![AirPlay Feature in Mobile Safari](http://macosrumors.com/wp-content/uploads/2011/01/IMG_0169.png)

In order to allow AirPlay as an option, video publishers have to add an attribute to the HTML5 video tag. &lt;video width=&ldquo;480&rdquo; height=&ldquo;300&rdquo; controls x-webkit-airplay=&ldquo;allow&rdquo;&gt; &hellip; We&rsquo;ll be adding this as a default attribute in the next VideoJS version.

[Read the whole story on MacRumors](http://macosrumors.com/2011/01/12/first-look-mobile-safari-airplay/).
