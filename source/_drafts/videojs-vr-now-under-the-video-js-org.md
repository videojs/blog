---
title: videojs-vr now under the video.js org
author:
  name: Brandon Casey
  github: brandonocasey
tags: vr, 360, plugins
---

We now support a VR/360 video plugin Video.js 6!

## Where did it come from?
This plugin has a long history and has had many maintainers.
It started as https://github.com/slawrence/videojs-vr. From there it became https://github.com/metacdn/videojs-vr. Over time work was done on it internally.  Finally it we decided that we should treat this plugin like a first class citizen. So we planned to migrate it to the video.js organization. Before doing so I updated it to the advanced plugin API and fixed a few long-standing issues.

## How do you know a video is 360/VR
### On desktop players
In a non-browser environment this question is easily answered, because their is a generally agreed upon standard. The video itself should have metadata indicating that the video is a 360/VR video and how to display it. Even outside of the browser though it is a bit tricky because most programs (even ffmpeg) don't support the insertion of this metadata. In fact a lot of them will strip said metadata as they think it is invalid. That is where a project called (Spatial Media Metadata Injector)[https://github.com/google/spatial-media] comes in. (Spatial Media Metadata Injector)[https://github.com/google/spatial-media] injects  360/VR metadata into the video without changing anything else.
That allows players like VLC to know that a video is 360/VR.

### How this plugin does it
The browser does not expose video metadata in an API so we would either have to parse it ourselves. Which isn't really an option or we would have to do something else. So in videojs-vr we have two settings to determine if a video is VR/360.

The first and default is setting 'projection' to 'AUTO' when initializing the plugin. By settings the projection to 'AUTO' we are telling videojs-vr to look at `player.mediainfo.projection`. `player.mediainfo.projection` will have to be set some external plugin/script that gets told by the server that a video is 360/VR. The mediainfo projection of a video can be any of the following:

* `'360'`, `'Sphere'`, or `'equirectangular'`: The video is a sphere
* `'Cube'` or `'360_CUBE'`: The video is a cube
* `'NONE'`: This video is not a 360 video, this does not have to be set as it will be assumed if no projection exists
* `'360_LR'`: Used for side-by-side 360 videos
* `'360_TB'`: Used for top-to-bottom 360 videos

The other second setting for 'projection' can be any of the above (other than 'NONE'). This will treat every video that is played by a player as the type of video that the projection is set as. The drawback is that you will only want to play VR/360 videos in such a player. Normal videos will still try to play back as if they were VR/360 in such a player.

## What happens after after we know a video is 360/VR
Using the current projection value we determine what we need to show on our [three.js](https://github.com/mrdoob/three.js) canvas. From their we use the polyfilled (via [webvr-polyfill](https://github.com/googlevr/webvr-polyfill) or real [VRDisplay API](https://developer.mozilla.org/en-US/docs/Web/API/VRDisplay) to set up controls. If the device has a VIVE, Oculus, etc plugged in we use that. If not we display the video in 360 mode where the mouse/keyboard/accelerometer control the video. On mobile devies their is also a button to go into cardboard mode which will show the video as two smaller videos separated by a line. This is so that the video can be viewed using pseudo VR headsets (such as google cardboard) and a phone. While the video is playing we use animation frames to animate the VideoTexture on the three.js canvas. That allows us to show the correct portion of the video based on the controls that the user is inputting.

## Why is it so huge?
We all agree that the plugin is huge, but due to the fact that browsers do not all natively support the VRDisplay API. We have to include a lot of big dependencies in our project. Mainly the size comes from the following:

1. [three.js](https://github.com/mrdoob/three.js) which we use to create the VR/360 canvas, and implementing it on our own would be painful
2. [webvr-polyfill](https://github.com/googlevr/webvr-polyfill) which polyfills vr features for browsers that don't currently support vr (outside of experimental flags and builds)
3. [webvr-boilerplate](https://github.com/borismus/webvr-boilerplate) this was used to get the plugin off of the ground quicker but will probably be replaced in the future with a smaller in house implementation. It basically does a bit mory polyfill for us.

## Debugging
Every object from three.js and webvr manager (from webvr-boilerplate) are available on the instance of the vr plugin that will is returned by `player.vr()`.

## Known Issues
*  Multiple players on the same page will share controls, panning in once video will pan in the other.
* There is no way to use videos in 360 mode when native VR is supported

## The Future
* We plan on cutting out webvr-bolierplate to save lots of bytes
* Implementing player level controls rather than window controls. This will allow individual videos to be controlled separately based on the current focus.
* A menu so that multiple vr displays can be used rather than just the first one that we find.

## To see the code, submit an issue, suggest a feature, or submit a pr
See the github repo [videojs-vr](https://github.com/videojs/videojs-vr)
