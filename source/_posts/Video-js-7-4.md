---
title: Video.js 7.4
tags:
  - a11y
  - responsive
  - languages
  - live-ui
  - focus-visible
author:
  name: Gary Katsevman
  github: gkatsev
date: 2019-01-11 16:24:34
---


It's time to have an overview of Video.js 7.4, first released early December. The big new feature for this release is a UI that allows you to seek during live streams. We updated focus-visible to work with our Menus, added more translations, added a replay option to the Play/Pause button, and many, many fixes, multiple of which are accessibility related.

We also dropped our usage of Grunt from our build process. We owe a lot to Grunt as it has served us well but it was time to move on.

Video.js 7.4.1 is currently the latest release with 7.4.2 out as a pre-release until next week.

## Thanks

Before continuing, I'd like to thank everyone that was involved, we really appreciate your contributions! There were a total of 14 first time contributors, I think this is a historic high for us and I hope this continues!

- [@BrandonOCasey](https://github.com/BrandonOCasey)
- [@gesinger](https://github.com/gesinger)
- [@gjanblaszczyk](https://github.com/gjanblaszczyk) (first time contributor!)
- [@valse](https://github.com/valse) (first time contributor!)
- [@gstrat88](https://github.com/gstrat88)
- [@eranshmil](https://github.com/eranshmil) (first time contributor!)
- [@carlmorris](https://github.com/carlmorris) (first time contributor!)
- [@bartlomein](https://github.com/bartlomein) (first time contributor!)
- [@DanielRuf](https://github.com/DanielRuf) (first time contributor!)
- [@Quenty31](https://github.com/Quenty31) (first time contributor!)
- [@fketchakeu](https://github.com/fketchakeu) (first time contributor!)
- [@OwenEdwards](https://github.com/OwenEdwards)
- [@alex-barstow](https://github.com/alex-barstow)
- [@vitaliytv](https://github.com/vitaliytv) (first time contributor!)
- [@oaprograms](https://github.com/oaprograms) (first time contributor!)
- [@xjoaoalvesx](https://github.com/xjoaoalvesx) (first time contributor!)
- [@webdeveloperpr](https://github.com/webdeveloperpr) (first time contributor!)
- [@smbea](https://github.com/smbea) (first time contributor!)
- [@dustin71728](https://github.com/dustin71728) (first time contributor!)

## Live UI
Video.js has supported live streams for a while, either natively, or via [videojs-http-streaming (VHS)](https://github.com/videojs/http-streaming). However, this UI was very minimal, it disabled the progress bar and basically only allowed pausing, though, there was an indicator that this was a live stream.

![Video.js, with the new Live UI, playing a live stream](live ui.png)

The new UI looks pretty similar to the previous live UI and the regular control bar. The "live" indicator moves to the right side of the progress bar and also indicates whether we are playing back live or we are behind live. Clicking this button will seek to the "live point". The time displays and tooltips will show time with respect to the live point; so, current time at the live point will be `0` and if you seek back 30 seconds it'll show `-00:30`.

This feature is still somewhat experimental, so, it is behind an option that is off by default. We hope that you try it out and give us feedback and that we can enable it by default in a future release.

To enable the feature, pass in `liveui: true` to the player:
In JavaScript:
```js
var player = videojs('my-id', {
  liveui: true
  }
);
```
Or HTML:
```html
<video-js id="my-id" data-setup='{"liveui": true}'}>
  <source src="https://example.com/live.m3u8" type="application/x-mpegurl">
</video-js>
```

## Languages
We've had a bunch of language additions and updates in the 7.4 release line. In addition, we now copy the JSON files into the dist/lang folder for easier inclusion your projects.

### Languages added and updated
- Occitan (oc)
- Russian (ru)
- Welsh/Cymraeg (cy)
- Ukrainian (uk)
- Serbian (sr)
- Swedish (sv)

## Accessibility
As always, we aim to make Video.js as accessible and usable as we can. To that end, we've had a bunch of accessibility related fixes in these releases.

- Time displays accessibility with VoiceOver
- remove hidden control text in progress bar
- make the seek-to-live button announce itself to screen readers properly
- Make the `-` in the remaining time display be visual only and not readable by screen readers

## Other Features Updates
### Responsive Captions Settings Dialog
This uses the new `responsive` setting and breakpoints to make the dialog respond to the size of the player and improves the user experience.
### replay option for PlayToggle
The PlayToggle button changes the icon to a replace icon when the video ends, most users don't mind it and it was a highly requested feature previously. This adds an option to turn it off.
### focus-visisble menu backgrounds
Like outlines for other buttons, we use a different background color to represent the focus in menus. We should respect focus-visible there like we do for outlines in buttons.
### playerreset event
When the player is reset with the `reset()` method, it'll now trigger a `playerreset` event to let components and users know.

## Other Fixes
- Fix fullscreen event triggering twice (7.4.2)
- PlayToggle cursor pointer
- select default subtitles on loadedmetadata not loadstart
- don't apply user preference to subtitles if no language is set
- make sure that vjs-waiting is only removed if we started playback again
- allow duration to be set to NaN, making Video.js more spec compliant
- fix locking menus when the menu button is pressed
- remove child component from old parent when moving the component to a new parent
- remove vjs-ended when seeking after video has ended
- don't autohide the control bar when hovering with the mouse

## Full CHANGELOG for 7.4.0, 7.4.1, and 7.4.2
<a name="7.4.2"></a>
## [7.4.2](https://github.com/videojs/video.js/compare/v7.4.1...v7.4.2) (2019-01-08)

### Bug Fixes

* Control-bar autohide when cursor placed over it [#5258](https://github.com/videojs/video.js/issues/5258) ([#5692](https://github.com/videojs/video.js/issues/5692)) ([6ebc772](https://github.com/videojs/video.js/commit/6ebc772))
* css animation shorthand property order ([#5687](https://github.com/videojs/video.js/issues/5687)) ([0e69ce9](https://github.com/videojs/video.js/commit/0e69ce9))
* **fs:** make sure there's only one fullscreenchange event ([#5686](https://github.com/videojs/video.js/issues/5686)) ([2bc90a1](https://github.com/videojs/video.js/commit/2bc90a1)), closes [#5685](https://github.com/videojs/video.js/issues/5685)
* **lang:** adds sv translation used by liveui component ([#5704](https://github.com/videojs/video.js/issues/5704)) ([f38726e](https://github.com/videojs/video.js/commit/f38726e))
* **package:** update [@videojs](https://github.com/videojs)/http-streaming to version 1.6.0 🚀 ([#5705](https://github.com/videojs/video.js/issues/5705)) ([3d093ed](https://github.com/videojs/video.js/commit/3d093ed))
* **player:** remove vjs-ended class on seeked ([#5728](https://github.com/videojs/video.js/issues/5728)) ([f1637cd](https://github.com/videojs/video.js/commit/f1637cd)), closes [#5654](https://github.com/videojs/video.js/issues/5654)
* **remaining-time-display:** make the '-' be visual and not readable by screen readers ([#5671](https://github.com/videojs/video.js/issues/5671)) ([05513f8](https://github.com/videojs/video.js/commit/05513f8)), closes [#5168](https://github.com/videojs/video.js/issues/5168)
* **seekbar:** don't disable if live tracker's seekable is infinity ([#5721](https://github.com/videojs/video.js/issues/5721)) ([7f507df](https://github.com/videojs/video.js/commit/7f507df))
* remove child from old parent when moving to new parent via addChild ([#5702](https://github.com/videojs/video.js/issues/5702)) ([8a3e2a7](https://github.com/videojs/video.js/commit/8a3e2a7))

### Chores

* **package:** update babel to version 7.2.2 ([#5697](https://github.com/videojs/video.js/issues/5697)) ([30d0b98](https://github.com/videojs/video.js/commit/30d0b98)), closes [#5689](https://github.com/videojs/video.js/issues/5689)
* **package:** update rollup to version 0.68.0 🚀 ([#5690](https://github.com/videojs/video.js/issues/5690)) ([f0ba1f5](https://github.com/videojs/video.js/commit/f0ba1f5))

### Documentation

* **liveui:** Add a guide for the live ui and live api ([#5677](https://github.com/videojs/video.js/issues/5677)) ([c147581](https://github.com/videojs/video.js/commit/c147581))

<a name="7.4.1"></a>
## [7.4.1](https://github.com/videojs/video.js/compare/v7.4.0...v7.4.1) (2018-12-11)

### Bug Fixes

* **a11y:** current time and duration display accessibility with VoiceOver ([#5653](https://github.com/videojs/video.js/issues/5653)) ([8932611](https://github.com/videojs/video.js/commit/8932611)), closes [/www.w3.org/TR/html-aam-1.0/#details-id-124](https://github.com//www.w3.org/TR/html-aam-1.0//issues/details-id-124)
* **a11y:** fix hidden Control Text in Progress bar (Fixes [#5251](https://github.com/videojs/video.js/issues/5251)) ([#5655](https://github.com/videojs/video.js/issues/5655)) ([70a71ae](https://github.com/videojs/video.js/commit/70a71ae))
* **a11y:** make seek-to-live better announce itself to screen reader users ([#5651](https://github.com/videojs/video.js/issues/5651)) ([165c120](https://github.com/videojs/video.js/commit/165c120))
* **lang:** append UKR translations and fix check translations command ([#5642](https://github.com/videojs/video.js/issues/5642)) ([b7aafdc](https://github.com/videojs/video.js/commit/b7aafdc))
* **lang:** improves sv lang file ([#5673](https://github.com/videojs/video.js/issues/5673)) ([b9d8744](https://github.com/videojs/video.js/commit/b9d8744))
* **lang:** Update sr.json ([#5657](https://github.com/videojs/video.js/issues/5657)) ([98b4a1c](https://github.com/videojs/video.js/commit/98b4a1c))
* **liveui:** make edge detection less strict, add docs for option ([#5661](https://github.com/videojs/video.js/issues/5661)) ([dce4a2c](https://github.com/videojs/video.js/commit/dce4a2c))
* **liveui:** seek to live should be immediate and other tweaks ([#5650](https://github.com/videojs/video.js/issues/5650)) ([831961b](https://github.com/videojs/video.js/commit/831961b))
* **package:** update [@videojs](https://github.com/videojs)/http-streaming to version 1.5.1 🚀 ([#5658](https://github.com/videojs/video.js/issues/5658)) ([8c9702a](https://github.com/videojs/video.js/commit/8c9702a))

### Chores

* **package:** update autoprefixer to version 9.4.2 ([#5647](https://github.com/videojs/video.js/issues/5647)) ([19f3465](https://github.com/videojs/video.js/commit/19f3465))
* **package:** update rollup-plugin-node-resolve to version 4.0.0 🚀 ([#5666](https://github.com/videojs/video.js/issues/5666)) ([d07b6c2](https://github.com/videojs/video.js/commit/d07b6c2))

### Documentation

* remove grunt and update usage of build scripts ([#5656](https://github.com/videojs/video.js/issues/5656)) ([62f9e78](https://github.com/videojs/video.js/commit/62f9e78))

### Tests

* verify null-checks with player and control bar children set to false ([#5670](https://github.com/videojs/video.js/issues/5670)) ([13b42ad](https://github.com/videojs/video.js/commit/13b42ad))

<a name="7.4.0"></a>
# [7.4.0](https://github.com/videojs/video.js/compare/v7.3.0...v7.4.0) (2018-12-03)

### Features

* add 'replay' option to the PlayToggle component. ([#5531](https://github.com/videojs/video.js/issues/5531)) ([f178458](https://github.com/videojs/video.js/commit/f178458)), closes [#4802](https://github.com/videojs/video.js/issues/4802)
* **lang:** Add the Occitan locale ([#5578](https://github.com/videojs/video.js/issues/5578)) ([0fb637d](https://github.com/videojs/video.js/commit/0fb637d))
* **lang:** Add Welsh/Cymraeg (cy) translations ([#5561](https://github.com/videojs/video.js/issues/5561)) ([b2c1077](https://github.com/videojs/video.js/commit/b2c1077))
* **lang:** copy language JSON files into dist dir ([#5549](https://github.com/videojs/video.js/issues/5549)) ([eb5de19](https://github.com/videojs/video.js/commit/eb5de19)), closes [#5092](https://github.com/videojs/video.js/issues/5092)
* **player:** add playerreset event ([#5335](https://github.com/videojs/video.js/issues/5335)) ([0e5442f](https://github.com/videojs/video.js/commit/0e5442f))
* make menu background respect :focus-visible ([#5558](https://github.com/videojs/video.js/issues/5558)) ([e5e1e29](https://github.com/videojs/video.js/commit/e5e1e29))
* responsive caption settings ([#5534](https://github.com/videojs/video.js/issues/5534)) ([b67fe27](https://github.com/videojs/video.js/commit/b67fe27))
* support seeking during live playback via liveui option ([#5511](https://github.com/videojs/video.js/issues/5511)) ([2974ad3](https://github.com/videojs/video.js/commit/2974ad3))

### Bug Fixes

* add correct cursor pointer for the play toggle  ([#5463](https://github.com/videojs/video.js/issues/5463)) ([aed337a](https://github.com/videojs/video.js/commit/aed337a))
* default subtitles not enabled ([#5608](https://github.com/videojs/video.js/issues/5608)) ([8329e64](https://github.com/videojs/video.js/commit/8329e64))
* **tracks:** don't select tracks based on user pref if no langauge is set ([#5556](https://github.com/videojs/video.js/issues/5556)) ([c1cbce3](https://github.com/videojs/video.js/commit/c1cbce3)), closes [#5553](https://github.com/videojs/video.js/issues/5553)
* Don't remove vjs-waiting until time changes ([#5533](https://github.com/videojs/video.js/issues/5533)) ([0060747](https://github.com/videojs/video.js/commit/0060747))
* **lang:** add  is loading ru translation ([#5630](https://github.com/videojs/video.js/issues/5630)) ([0090b75](https://github.com/videojs/video.js/commit/0090b75))
* **lang:** Occitan: harmonisation plural/singular ([#5602](https://github.com/videojs/video.js/issues/5602)) ([4842201](https://github.com/videojs/video.js/commit/4842201))
* **package:** update [@videojs](https://github.com/videojs)/http-streaming to version 1.4.2 🚀 ([#5543](https://github.com/videojs/video.js/issues/5543)) ([dbaca33](https://github.com/videojs/video.js/commit/dbaca33))
* **package:** update [@videojs](https://github.com/videojs)/http-streaming to version 1.5.0 🚀 ([#5587](https://github.com/videojs/video.js/issues/5587)) ([d95ef6f](https://github.com/videojs/video.js/commit/d95ef6f))
* duration reset and allow duration NaN or 0 for duration display ([#5348](https://github.com/videojs/video.js/issues/5348)) ([ab0e29a](https://github.com/videojs/video.js/commit/ab0e29a)), closes [#5347](https://github.com/videojs/video.js/issues/5347)
* not inline volume slider showing up after mouse hovering on it ([#5503](https://github.com/videojs/video.js/issues/5503)) ([7d127c8](https://github.com/videojs/video.js/commit/7d127c8)), closes [#5502](https://github.com/videojs/video.js/issues/5502) [#5505](https://github.com/videojs/video.js/issues/5505)
* vjs-lock-showing class gets removed from menu when no longer hovering on menu-button. ([#5465](https://github.com/videojs/video.js/issues/5465)) ([58f638e](https://github.com/videojs/video.js/commit/58f638e)), closes [#1690](https://github.com/videojs/video.js/issues/1690)

### Chores

* fix lint on pre-commit with lint-staged, use npm-merge-driver ([#5591](https://github.com/videojs/video.js/issues/5591)) ([be9e9a9](https://github.com/videojs/video.js/commit/be9e9a9))
* fix travis build ([#5627](https://github.com/videojs/video.js/issues/5627)) ([6c1056b](https://github.com/videojs/video.js/commit/6c1056b)), closes [#5626](https://github.com/videojs/video.js/issues/5626) [#5616](https://github.com/videojs/video.js/issues/5616)
* Move a11y, lang, browserify, and webpack out of grunt ([#5589](https://github.com/videojs/video.js/issues/5589)) ([db6e376](https://github.com/videojs/video.js/commit/db6e376))
* move copy, zip, and clean tasks to npm scripts ([#5544](https://github.com/videojs/video.js/issues/5544)) ([2d682a4](https://github.com/videojs/video.js/commit/2d682a4))
* remove grunt move to npm scripts ([#5592](https://github.com/videojs/video.js/issues/5592)) ([d72786f](https://github.com/videojs/video.js/commit/d72786f))
* switch from cross-var to cross-env ([#5600](https://github.com/videojs/video.js/issues/5600)) ([ab740bc](https://github.com/videojs/video.js/commit/ab740bc))
* switch to videojs-generate-karma-config ([#5528](https://github.com/videojs/video.js/issues/5528)) ([2e70450](https://github.com/videojs/video.js/commit/2e70450))
* update all the dev deps to their latest versions ([#5645](https://github.com/videojs/video.js/issues/5645)) ([db1369a](https://github.com/videojs/video.js/commit/db1369a)), closes [#5644](https://github.com/videojs/video.js/issues/5644) [#5643](https://github.com/videojs/video.js/issues/5643)
* update deps, remove coveralls, fix audit issues ([#5555](https://github.com/videojs/video.js/issues/5555)) ([11f1fb8](https://github.com/videojs/video.js/commit/11f1fb8))
* use relative urls in index.html ([#5586](https://github.com/videojs/video.js/issues/5586)) ([dec31e4](https://github.com/videojs/video.js/commit/dec31e4))
* **netlify:** make docs build properly ([#5636](https://github.com/videojs/video.js/issues/5636)) ([a8828cd](https://github.com/videojs/video.js/commit/a8828cd))
* **package:** update conventional-changelog-cli to version 2.0.11 ([#5552](https://github.com/videojs/video.js/issues/5552)) ([f236176](https://github.com/videojs/video.js/commit/f236176))
* **package:** update grunt-cli to version 1.3.2 ([#5550](https://github.com/videojs/video.js/issues/5550)) ([2d27b6a](https://github.com/videojs/video.js/commit/2d27b6a))
* **package:** update husky to version 1.1.3 ([#5551](https://github.com/videojs/video.js/issues/5551)) ([937e2bf](https://github.com/videojs/video.js/commit/937e2bf))
* **package:** update npm-run-all to 4.1.5 to remove event-stream ([#5614](https://github.com/videojs/video.js/issues/5614)) ([3e52c4f](https://github.com/videojs/video.js/commit/3e52c4f))
* **package:** update remark-stringify to version 6.0.1 ([#5539](https://github.com/videojs/video.js/issues/5539)) ([d46828a](https://github.com/videojs/video.js/commit/d46828a))
* **package:** update rollup to version 0.67.1 ([#5580](https://github.com/videojs/video.js/issues/5580)) ([209d9f9](https://github.com/videojs/video.js/commit/209d9f9))
* **package:** update videojs-generate-karma-config to version 5.0.0 🚀 ([#5595](https://github.com/videojs/video.js/issues/5595)) ([2162239](https://github.com/videojs/video.js/commit/2162239))
* **player:** fix linting for a comment ([#5588](https://github.com/videojs/video.js/issues/5588)) ([b5e6bdc](https://github.com/videojs/video.js/commit/b5e6bdc))
* **travis:** remove unused secret variables ([#5577](https://github.com/videojs/video.js/issues/5577)) ([15beea7](https://github.com/videojs/video.js/commit/15beea7))

### Documentation

* **media-error:** Correct error type documentation ([#5566](https://github.com/videojs/video.js/issues/5566)) ([441f0e1](https://github.com/videojs/video.js/commit/441f0e1))
* update starter template ([#5570](https://github.com/videojs/video.js/issues/5570)) ([287b267](https://github.com/videojs/video.js/commit/287b267)), closes [1000#0](https://github.com/1000/issues/0) [#5562](https://github.com/videojs/video.js/issues/5562)
* Update urls in README.md to point to v7.3.0 ([#5536](https://github.com/videojs/video.js/issues/5536)) ([79edf5b](https://github.com/videojs/video.js/commit/79edf5b))
