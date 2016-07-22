---
title: Video.js 5.11.0 Prelease
author:
  name: Gary Katsevman
  github: gkatsev
date: 2016-07-22 15:46:51
tags:
   - news
   - releases
   - version
---
Today sees the prerelease of version 5.11.0. I wanted to take a moment to talk about some of the changes, additions, and known issues.

This is a pre-release only. It's available on npm under the `next` tag and also available on the CDN under the fully qualified version number: `//vjs.zencdn.net/5.11.0/video.js`. It'll stay in pre-release state for around a week or more to make sure that there aren't any glaring bugs, so, please give it a shot and [open issues](https://github.com/videojs/video.js/issues/new) if you find anything.

## Notable Changes
* In version 5.0, we wanted to deprecate `videojs.players` property in favor of of the `videojs.getPlayers()` getter. However, it's proved to be very useful and it is now being un-deprecated so it will no longer print deprecations in the console.
* If the player is created without a source set and a user hits play, video.js will now wait for the a source to be provided before starting playback. This eliminates an error that happens when we try to play an empty source.
* Our custom captions settings dialog was updated to be more accessible by using more aria attributes and better option names in drop downs.

## Known Issues
* In video.js 5.10, to be able to better respond to the source of the video element changing directly without going through video.js, we [disposed SourceHandlers on subsequent loadstarts](https://github.com/videojs/video.js/pull/3285) and [cleared out the current source](https://github.com/videojs/video.js/pull/3314). However, this causes an [issue with videojs-contrib-dash](https://github.com/videojs/video.js/issues/3428). A contributor investigated and found out that the two PRs mentioned before cause the issue. If a video is created with an MPEG-DASH source element, we end up seeing a `loadstart` event because of the source element and then when Dash.js kicks in and starts playback using MSE, we get another `loadstart` event. Since we see the second `loadstat` event we dispose of the SourceHandler, that is, videojs-contrib-dash and Dash.js, and the video doesn't play. A work around, while we figure out a correct solution, would be not to use source elements with DASH sources and only use the video.js API in the meantime.

## Raw Changelog
* @BrandonOCasey Document audio/video track usage ([view](https://github.com/videojs/video.js/pull/3295))
* @hartman Correct documentation to refer to nativeTextTracks option ([view](https://github.com/videojs/video.js/pull/3309))
* @nickygerritsen Also pass tech options to canHandleSource ([view](https://github.com/videojs/video.js/pull/3303))
* @misteroneill Un-deprecate the videojs.players property ([view](https://github.com/videojs/video.js/pull/3299))
* @nickygerritsen Add title to all clickable components ([view](https://github.com/videojs/video.js/pull/3296))
* @nickygerritsen Update Dutch language file ([view](https://github.com/videojs/video.js/pull/3297))
* @hartman Add descriptions and audio button to adaptive classes ([view](https://github.com/videojs/video.js/pull/3312))
* @MattiasBuelens Retain details from tech error ([view](https://github.com/videojs/video.js/pull/3313))
* @nickygerritsen Fix test for tooltips in IE8 ([view](https://github.com/videojs/video.js/pull/3327))
* @mboles added loadstart event to jsdoc ([view](https://github.com/videojs/video.js/pull/3370))
* @hartman added default print styling ([view](https://github.com/videojs/video.js/pull/3304))
* @ldayananda updated videojs to not do anything if no src is set ([view](https://github.com/videojs/video.js/pull/3378))
* @nickygerritsen removed unused tracks when changing sources. Fixes ##3000 ([view](https://github.com/videojs/video.js/pull/3002))
* @vit-koumar updated Flash tech to return Infinity from duration instead of -1 ([view](https://github.com/videojs/video.js/pull/3128))
* @alex-phillips added ontextdata to Flash tech ([view](https://github.com/videojs/video.js/pull/2748))
* @MattiasBuelens updated components to use durationchange only ([view](https://github.com/videojs/video.js/pull/3349))
* @misteroneill improved Logging for IE < 11 ([view](https://github.com/videojs/video.js/pull/3356))
* @vdeshpande updated control text of modal dialog ([view](https://github.com/videojs/video.js/pull/3400))
* @ldayananda fixed mouse handling on menus by using mouseleave over mouseout ([view](https://github.com/videojs/video.js/pull/3404))
* @mister-ben updated language to inherit correctly and respect the attribute on the player ([view](https://github.com/videojs/video.js/pull/3426))
* @sashyro fixed nativeControlsForTouch option ([view](https://github.com/videojs/video.js/pull/3410))
* @tbasse fixed techCall null check against tech ([view](https://github.com/videojs/video.js/pull/2676))
* @rbran100 checked src and currentSrc in handleTechReady to work around mixed content issues in chrome ([view](https://github.com/videojs/video.js/pull/3287))
* @OwenEdwards fixed caption settings dialog labels for accessibility ([view](https://github.com/videojs/video.js/pull/3281))
* @OwenEdwards removed spurious head tags in the simple-embed example ([view](https://github.com/videojs/video.js/pull/3438))
* @ntadej added a null check to errorDisplay usage ([view](https://github.com/videojs/video.js/pull/3440))
* @misteroneill fixed logging issues on IE by separating fn.apply and stringify checks ([view](https://github.com/videojs/video.js/pull/3444))
* @misteroneill fixed npm test from running coveralls locally ([view](https://github.com/videojs/video.js/pull/3449))
* @gkatsev added es6-shim to tests. Fixes Flash duration test ([view](https://github.com/videojs/video.js/pull/3453))
* @misteroneill corrects test assertions for older IEs in the log module ([view](https://github.com/videojs/video.js/pull/3454))
* @gkatsev fixed setting lang by looping through loop element variable and not constant tag ([view](https://github.com/videojs/video.js/pull/3455))

## Git diffstats
These are deltas between 5.11.0 and 5.10.7
```
CHANGELOG.md                                       |    33 +
build/grunt.js                                     |     3 +-
component.json                                     |     2 +-
docs/examples/simple-embed/index.html              |     3 -
docs/guides/audio-tracks.md                        |    69 +
docs/guides/languages.md                           |    12 +-
docs/guides/text-tracks.md                         |   184 +
docs/guides/tracks.md                              |   186 +-
docs/guides/video-tracks.md                        |    70 +
docs/index.md                                      |     2 +-
lang/en.json                                       |     1 +
lang/nl.json                                       |    19 +-
package.json                                       |     7 +-
src/css/_print.scss                                |     5 +
src/css/components/_adaptive.scss                  |     9 +-
src/css/components/_captions-settings.scss         |    26 +-
src/css/video-js.scss                              |     2 +
src/js/clickable-component.js                      |    12 +-
.../control-bar/time-controls/duration-display.js  |     8 +-
.../time-controls/remaining-time-display.js        |     1 +
src/js/menu/menu-button.js                         |     4 +-
src/js/modal-dialog.js                             |     2 +-
src/js/player.js                                   |    68 +-
src/js/tech/flash-rtmp.js                          |     3 +-
src/js/tech/flash.js                               |    21 +-
src/js/tech/html5.js                               |    73 +-
src/js/tech/tech.js                                |    16 +-
src/js/tracks/text-track-settings.js               |   176 +-
src/js/utils/browser.js                            |     3 +
src/js/utils/create-deprecation-proxy.js           |    50 -
src/js/utils/log.js                                |   124 +-
src/js/video.js                                    |    16 +-
test/globals-shim.js                               |     1 +
test/unit/button.test.js                           |     5 +-
test/unit/player.test.js                           |    22 +-
test/unit/plugins.test.js                          |    20 +-
test/unit/tech/flash.test.js                       |    51 +-
test/unit/tech/html5.test.js                       |    16 +-
test/unit/tech/tech.test.js                        |    17 +-
test/unit/tracks/text-track-settings.test.js       |    51 +-
test/unit/tracks/text-track.test.js                |    13 +-
test/unit/utils/create-deprecation-proxy.test.js   |    45 -
test/unit/utils/log.test.js                        |   104 +-
103 files changed, 971 insertions(+), 55554 deletions(-)
```
