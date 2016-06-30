---
title: Version 1.1.3 Release
tags:
  - version
author:
  name: Steve Heffernan
  github: heff
alias:
  - post/35887731208/version-1-1-3-release/index.html
  - post/35887731208/index.html
date: 2010-10-19 00:00:00
---

Version 1.1.3 of VideoJS is now available.

Probably the most notable change is a switch to using a big play button before the movie plays for the first time, as opposed to showing the control bar. The main reason for doing this is actually a bug in Safari. If you try to call any functions on the video other than play, before the video has loaded, Safari will error out. Some people mentioned that fullscreen mode did not work in Safari for them, and this is most likely what was happening for them. This would only happen if you were not preloading the video. You can still go back to showing the controls first through the showControlsAtStart option, but it&rsquo;s not recommended.

Other features and fixes include:

*   Feature: Width/Height functions for resizing the player
*   Feature: Made initial click &amp; hold trigger new value on progress and volume
*   Feature: Made controls not hide when hovering over them
*   Fix: Removed trailing comma that was breaking IE7
*   Fix: Removed some vars from global scope
*   Fix: Changed a document.onmousemove to an eventListener to prevent conflicts
*   Fix: Added a unique ID to FlowPlayer demo object to fix a FlowPlayer bug. Thanks @emirpprime.
*   Fix: Safari error on unloaded video

Let me know if you have any questions.
