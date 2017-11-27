---
title: Video.js 6.5.0 Release
author:
  name: Gary Katsevman
  github: gkatsev
date: 2017-11-27 17:16:06
tags:
---


November 17th marked the pre-release of Video.js 6.5.0. It comes shortly after the [release of 6.4.0][640], which has now been promoted to latest! This is a pretty exciting release because we finally got our own element! I like to call it the I Can't Believe It's Not Custom Elements because it isn't an actual custom element but it's not a standard HTML element either. Also, a nice and smooth progress bar thanks to a first time contributor; thanks [@vhmth][vhmth]!. A pretty major memory leak fix, and many code refactors and bug fixes.
I'd like to thank everyone who contributed and the four first time contributors to Video.js: [@vhmth][vhmth], [@FirefoxMetzger][FirefoxMetzger], [@EhsanCh][EhsanCh], and [@shahlabs][shahlabs].
This post comes a week late because it was Thanksgiving week in the US after release and I was on break, if you celebrate Turkey day 🦃, hope you had a good one!


## Noteable Changes
* I Can't Believe It's Not Custom Elements with a `<video-js>` element. It works exactly like the `<video>` embed but without requiring you to use the `<video>` element and the benefits and drawbacks that come with it. It also adds the `video-js` class name automatically for you, so, there's no need to add it.
* A smooth progress bar!
![before the change](./before.gif)
![after the change](./after.gif)
* After much spelunking in the code base and developer tools, we've plugged most of the memory leaks with retained DOM elements in Video.js!
* Many code refactorings from our old code style to the new one by [@kocoten1992][kocoten1992]!
* Previously, you could accidentally seek or toggle playback using the middle or right clicks. Now, just go ahead and try it!
* An accessibility fix with regards to title tooltips and menu items.
* Better handling of `play()` in our new asynchronous world, especially immediately after changing the source.

## Committers
* Vinay [@vhmth][vhmth] First PR
* Ilya Piatrenka [@odisei369][odisei369]
* Brandon Casey [@brandonocasey][brandonocasey]
* [@FirefoxMetzger][FirefoxMetzger] First PR
* Ehsan Chavoshi [@EhsanCh][EhsanCh] First PR
* Chuong [@kocoten1992][kocoten1992]
* Martin Bachwerk [@arski][arski]
* Pat O'Neill [@misteroneill][misteroneill]
* Labdhi Shah [@shahlabs][shahlabs] First PR

## Raw Changelog
<a name="6.5.0"></a>
### [6.5.0](https://github.com/videojs/video.js/compare/v6.4.0...v6.5.0) (2017-11-17)

### Features

* add a version method to all advanced plugin instances ([#4714](https://github.com/videojs/video.js/issues/4714)) ([acf4153](https://github.com/videojs/video.js/commit/acf4153))
* allow embeds via <video-js> element ([#4640](https://github.com/videojs/video.js/issues/4640)) ([d8aadd5](https://github.com/videojs/video.js/commit/d8aadd5))

### Bug Fixes

* Avoid empty but shown title attribute with menu items and clickable components ([#4746](https://github.com/videojs/video.js/issues/4746)) ([dc588dd](https://github.com/videojs/video.js/commit/dc588dd))
* **Player#play:** Wait for loadstart in play() when changing sources instead of just ready. ([#4743](https://github.com/videojs/video.js/issues/4743)) ([26b0d2c](https://github.com/videojs/video.js/commit/26b0d2c))
* being able to toggle playback with middle click ([#4756](https://github.com/videojs/video.js/issues/4756)) ([7a776ee](https://github.com/videojs/video.js/commit/7a776ee)), closes [#4689](https://github.com/videojs/video.js/issues/4689)
* make the progress bar progress smoothly ([#4591](https://github.com/videojs/video.js/issues/4591)) ([acc641a](https://github.com/videojs/video.js/commit/acc641a))
* only allow left click dragging on progress bar and volume control ([#4613](https://github.com/videojs/video.js/issues/4613)) ([79b4355](https://github.com/videojs/video.js/commit/79b4355))
* only print element not in DOM warning on player creation ([#4755](https://github.com/videojs/video.js/issues/4755)) ([bbea5cc](https://github.com/videojs/video.js/commit/bbea5cc))
* trigger timeupdate during seek ([#4754](https://github.com/videojs/video.js/issues/4754)) ([1fcd5ae](https://github.com/videojs/video.js/commit/1fcd5ae))

### Chores

* **lang:** update Persian translations ([#4741](https://github.com/videojs/video.js/issues/4741)) ([95d7832](https://github.com/videojs/video.js/commit/95d7832))

### Code Refactoring

* player.controls() ([#4731](https://github.com/videojs/video.js/issues/4731)) ([d447e9f](https://github.com/videojs/video.js/commit/d447e9f))
* player.listenForUserActivity ([#4719](https://github.com/videojs/video.js/issues/4719)) ([c16fedf](https://github.com/videojs/video.js/commit/c16fedf))
* player.userActive() ([#4716](https://github.com/videojs/video.js/issues/4716)) ([6cbe3ed](https://github.com/videojs/video.js/commit/6cbe3ed))
* player.usingNativeControls() ([#4749](https://github.com/videojs/video.js/issues/4749)) ([eb909f0](https://github.com/videojs/video.js/commit/eb909f0))

### Documentation

* **readme:** fixed a typo ([#4730](https://github.com/videojs/video.js/issues/4730)) ([46a7df2](https://github.com/videojs/video.js/commit/46a7df2))

### Performance Improvements

* null out els on dispose to minimize detached els ([#4745](https://github.com/videojs/video.js/issues/4745)) ([2da7af1](https://github.com/videojs/video.js/commit/2da7af1))

### Tests

* clean up test warnings ([#4752](https://github.com/videojs/video.js/issues/4752)) ([3aae4b2](https://github.com/videojs/video.js/commit/3aae4b2))
* update tests to use qunit 2 assert format ([#4753](https://github.com/videojs/video.js/issues/4753)) ([06641e8](https://github.com/videojs/video.js/commit/06641e8))
* warning, if the element is not in the DOM ([#4723](https://github.com/videojs/video.js/issues/4723)) ([c213737](https://github.com/videojs/video.js/commit/c213737))



[640]: https://blog.videojs.com/video-js-6-4-0-release/
[vhmth]: https://github.com/vhmth
[FirefoxMetzger]: https://github.com/FirefoxMetzger
[EhsanCh]: https://github.com/EhsanCh
[shahlabs]: https://github.com/shahlabs
[kocoten1992]: https://github.com/kocoten1992
[odisei369]: https://github.com/odisei369
[brandonocasey]: https://github.com/brandonocasey
[arski]: https://github.com/arski
[misteroneill]: https://github.com/misteroneill
