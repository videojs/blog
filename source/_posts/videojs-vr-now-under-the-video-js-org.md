---
title: videojs-vr Is Now Under The Video.js Org
tags: 'vr, 360, plugins'
author:
  name: Brandon Casey
  github: brandonocasey
date: 2018-02-21 11:36:00
---


Brightcove and Video.js have adopted the videojs-vr plugin as a first class plugin.
It's out now with support for Video.js 6!

## Where did it come from?
This plugin has a long history and has had many maintainers.
It started as [slawrence/videojs-vr](https://github.com/slawrence/videojs-vr). From there it became [metacdn/videojs-vr](https://github.com/metacdn/videojs-vr). Then there was a private collaberation between [StreamShark.io](https://streamshark.io), [Brightcove](https://www.brightcove.com), and [HapYak](http://corp.hapyak.com/). Eventually we decided to treat this plugin like a first class citizen. So we updated it to the [advanced plugin API](http://docs.videojs.com/tutorial-plugins.html#writing-an-advanced-plugin), fixed a few long-standing issues, documented other issues along the way, and moved it to the Video.js Organization.

## How do you know a video is 360/VR?
### On desktop players
In a non-browser environment this question is easily answered, because there is a generally agreed upon standard. The video itself should have metadata indicating that the video is a 360/VR video and how to display it. Even outside of the browser though it is a bit tricky because most programs (even ffmpeg) don't support the insertion of this metadata. In fact a lot of them will strip said metadata as they think it is invalid. That is where a project called [Spatial Media Metadata Injector](https://github.com/google/spatial-media) comes in. [Spatial Media Metadata Injector](https://github.com/google/spatial-media) injects  360/VR metadata into the video without changing anything else.
That allows players like VLC to know that a video is 360/VR.

### How does videojs-vr do it?
The browser does not expose video metadata in an API so we would have to parse it ourselves, which isn't really an option. So in videojs-vr we have a `projection` option that can be passed in during plugin initialization.

The first and default setting for `projection` is `'AUTO'`. Setting `projection` to `'AUTO'` tells videojs-vr to look at `player.mediainfo.projection`. `player.mediainfo.projection` will have to be set by some external plugin/script which is told by a server that the current video is 360/VR. The `player.mediainfo.projection` of a video can be any of the following:

* `'360'`, `'Sphere'`, or `'equirectangular'`: The video is a sphere
* `'Cube'` or `'360_CUBE'`: The video is a cube
* `'NONE'`: This video is not a 360 video, the videojs-vr plugin should do nothing. this would not have to be set as it will be assumed if no projection exists.
* `'360_LR'`: Used for side-by-side 360 videos
* `'360_TB'`: Used for top-to-bottom 360 videos

Otherwise, the `projection` can manually be set on plugin initialization to any of the above values. The plugin can then be disposed and re-initialized for each video with a different setting.

## What happens after after we know a video is 360/VR?
Using the current projection value we determine what we need to show on our [three.js](https://github.com/mrdoob/three.js) canvas. From there we use the polyfilled (via [webvr-polyfill](https://github.com/googlevr/webvr-polyfill) or real [VRDisplay API](https://developer.mozilla.org/en-US/docs/Web/API/VRDisplay) to set up controls. If the device has a VIVE, Oculus, etc plugged in we use that. If not we display the video in 360 mode where the mouse/keyboard/accelerometer control the video. On mobile devies there is also a button to go into cardboard mode which will show the video as two smaller videos separated by a line. This is so that the video can be viewed using pseudo VR headsets (such as google cardboard) and a phone. While the video is playing we use animation frames to animate the VideoTexture on the three.js canvas. That allows us to show the correct portion of the video based on the controls that the user is inputting.

## Why is it so huge?
We all agree that the plugin is huge, but due to the fact that all browsers do not natively support the VRDisplay API, we have to include a lot of big dependencies in our project. Mainly the size comes from the following:

1. [three.js](https://github.com/mrdoob/three.js) which we use to create the VR/360 canvas, and implementing it on our own would be painful
2. [webvr-polyfill](https://github.com/googlevr/webvr-polyfill) which polyfills VR features for browsers that don't currently support VR (outside of experimental flags and builds)

## Debugging
Every object from three.js and webvr manager (from webvr-boilerplate) is available on the instance of the VR plugin that will is returned by `player.vr()`.

## Known Issues
* Multiple players on the same page will share controls, panning in one video will pan in the other.

## The Future
* Implementing player level controls rather than window controls. This will allow individual videos to be controlled separately based on the current focus.
* A menu so that multiple VR displays can be used rather than just the first one that we find.

## To see the code, submit an issue, suggest a feature, or submit a pr
See the github repo [videojs-vr](https://github.com/videojs/videojs-vr)
