---
title: Video.js 6.4.0 Release
author:
  name: Gary Katsevman
  github: gkatsev
date: 2017-11-02 16:01:38
tags:
---

Hello everyone, it's been a while! Yesterday, I pre-released Video.js 6.4.0. It's a pretty big release with 27 merged Pull Requests from 13 authors and 7 of them were contributors!
I'd like to thank everyone who contributed but particularly those seven. Six of them made their first PRs with Video.js: [@estim][estim], [@seggev319][seggev319], [@nicolaslevy][nicolaslevy], [@MarcAMo][MarcAMo], [@knilob][knilob], [@odisei369][odisei369]. And [@kocoten1992][kocoten1992] returned with some great PRs to refactor things.

In this release, we fixed a bunch of bugs, refactored some things, updated some tests. We also added some new features.

As with other releases, this is released as `next` for a short while before being promoted to `latest`.
You can get these releases on [GitHub Releases](https://github.com/videojs/video.js/releases/tag/v6.4.0) or from [npm](https://www.npmjs.com/package/video.js)

## Noteable Changes
- Hebrew translations are now available. Russian and Polish translations were updated as well.
- The Progress Control can now be disabled. This is useful for when you don't want users to be able to interact with it but still want to show play progress.
  - The Progress Control will also be fully filled out when the video ends so that we aren't slightly short or long due to weirdnesses in duration and currentTime.
- It's now possible to add a `hook` that gets automatically removed once called with `hookOnce`. This mirrors our `on` and `once` event methods.
- If controls are disabled before the modal dialog is opened, the controls stay closed when the dialog is closed.
- `player.src()` will now return an empty string when no source is set to match `player.currentSrc()` and the native video element.
- Video.js will now warn when the element it is given isn't in the DOM. This was done as part of a "first-timers-only" issue which we hope to do more off in the future.

## Google Analytics note
We've updated the [README][readme] of the project to be explicit about our usage of Google Analytics on the `vjs.zencdn.net` hosted version of Video.js. It is a stripped down version of the [Google Analytics pixel](https://github.com/videojs/cdn/blob/master/src/analytics.js) and sends data on %1 of those loads.
It can be opted out of by adding the following to the page before the `zencdn`-hosted version of Video.js is loaded
```html
<script>window.HELP_IMPROVE_VIDEOJS = false;</script>
```
Our GitHub releases and npm releases *do not* include Google Analytics. Neither do 3rd-party CDNs like unpkg or CDNjs.

## Committers
- Gary Katsevman [@gkatsev][gkatsev]
- Pat O'Neill [@misteroneill][misteroneill]
- Brandon Casey [@BrandonOCasey][BrandonOCasey]
- [@estim][estim] First PR
- [@seggev319][seggev319] First PR
- [@mister-ben][mister-ben]
- Chuong [@kocoten1992][kocoten1992]
- Nicolas Levy [@nicolaslevy][nicolaslevy] First PR
- Marc A. Modrow [@MarcAMo][MarcAMo] First two PRs
- Matt McClure [@mmcc][mmcc]
- Bo Link [@knilob][knilob] First PR
- Joe Forbes [@forbesjo][forbesjo]
- Ilya Piatrenka [@odisei369][odisei369] First PR


## Full Changelog
<a name="6.4.0"></a>
### [6.4.0](https://github.com/videojs/video.js/compare/v6.3.3...v6.4.0) (2017-11-01)

### Features

* **lang:** add Hebrew translation ([#4675](https://github.com/videojs/video.js/issues/4675)) ([32caf35](https://github.com/videojs/video.js/commit/32caf35))
* **lang:** Update for Russian translation ([#4663](https://github.com/videojs/video.js/issues/4663)) ([45e21fd](https://github.com/videojs/video.js/commit/45e21fd))
* Add videojs.hookOnce method to allow single-run hooks. ([#4672](https://github.com/videojs/video.js/issues/4672)) ([85fe685](https://github.com/videojs/video.js/commit/85fe685))
* add warning if the element given to Video.js is not in the DOM ([#4698](https://github.com/videojs/video.js/issues/4698)) ([6f713ca](https://github.com/videojs/video.js/commit/6f713ca))
* allow progress controls to be disabled ([#4649](https://github.com/videojs/video.js/issues/4649)) ([a3c254e](https://github.com/videojs/video.js/commit/a3c254e))
* set the play progress seek bar to 100% on ended ([#4648](https://github.com/videojs/video.js/issues/4648)) ([5e9655f](https://github.com/videojs/video.js/commit/5e9655f))

### Bug Fixes

* **css:** update user-select none ([#4678](https://github.com/videojs/video.js/issues/4678)) ([43ddc72](https://github.com/videojs/video.js/commit/43ddc72))
* aria-labelledby attribute has an extra space ([#4708](https://github.com/videojs/video.js/issues/4708)) ([855adf3](https://github.com/videojs/video.js/commit/855adf3)), closes [#4688](https://github.com/videojs/video.js/issues/4688)
* Don't enable player controls if they where disabled when ModalDialog closes. ([#4690](https://github.com/videojs/video.js/issues/4690)) ([afea980](https://github.com/videojs/video.js/commit/afea980))
* don't throttle duration change updates ([#4635](https://github.com/videojs/video.js/issues/4635)) ([9cf9800](https://github.com/videojs/video.js/commit/9cf9800))
* Events#off threw if Object.prototype had extra enumerable properties, don't remove all events if off receives a falsey value ([#4669](https://github.com/videojs/video.js/issues/4669)) ([7963913](https://github.com/videojs/video.js/commit/7963913))
* make parseUrl helper always have a protocl ([#4673](https://github.com/videojs/video.js/issues/4673)) ([bebca9c](https://github.com/videojs/video.js/commit/bebca9c)), closes [#3100](https://github.com/videojs/video.js/issues/3100)
* Make sure we remove vjs-ended from the play toggle in all appropriate cases. ([#4661](https://github.com/videojs/video.js/issues/4661)) ([0287f6e](https://github.com/videojs/video.js/commit/0287f6e))
* player.src() should return empty string if no source is set ([#4711](https://github.com/videojs/video.js/issues/4711)) ([9acbcd8](https://github.com/videojs/video.js/commit/9acbcd8))

### Chores

* **gh-release:** no console log on success ([#4657](https://github.com/videojs/video.js/issues/4657)) ([e8511a5](https://github.com/videojs/video.js/commit/e8511a5))
* **lang:** Update Polish ([#4686](https://github.com/videojs/video.js/issues/4686)) ([ee2a49c](https://github.com/videojs/video.js/commit/ee2a49c))
* **package:** update babelify to version 8.0.0 ([#4684](https://github.com/videojs/video.js/issues/4684)) ([db2f14c](https://github.com/videojs/video.js/commit/db2f14c))
* add comment about avoiding helvetica font ([#4679](https://github.com/videojs/video.js/issues/4679)) ([cb638d0](https://github.com/videojs/video.js/commit/cb638d0))
* add GA note to primary readme ([#4481](https://github.com/videojs/video.js/issues/4481)) ([e2af322](https://github.com/videojs/video.js/commit/e2af322))
* Add package-lock.json file. ([#4641](https://github.com/videojs/video.js/issues/4641)) ([ec5b603](https://github.com/videojs/video.js/commit/ec5b603))

### Code Refactoring

* component.ready() ([#4693](https://github.com/videojs/video.js/issues/4693)) ([b40858b](https://github.com/videojs/video.js/commit/b40858b))
* player.dimension() ([#4704](https://github.com/videojs/video.js/issues/4704)) ([ad1b47b](https://github.com/videojs/video.js/commit/ad1b47b))
* player.hasStarted() ([#4680](https://github.com/videojs/video.js/issues/4680)) ([cde8335](https://github.com/videojs/video.js/commit/cde8335))
* player.techGet_() ([#4687](https://github.com/videojs/video.js/issues/4687)) ([a1748aa](https://github.com/videojs/video.js/commit/a1748aa))

### Documentation

* **lang:** update translations needed doc ([#4702](https://github.com/videojs/video.js/issues/4702)) ([93e7670](https://github.com/videojs/video.js/commit/93e7670))

### Tests

* fix modal dialog test for showing controls ([#4707](https://github.com/videojs/video.js/issues/4707)) ([45a6b30](https://github.com/videojs/video.js/commit/45a6b30)), closes [#4706](https://github.com/videojs/video.js/issues/4706)
* get rid of redundant test logging ([#4682](https://github.com/videojs/video.js/issues/4682)) ([983a573](https://github.com/videojs/video.js/commit/983a573))


[gkatsev]: https://github.com/gkatsev
[misteroneill]: https://github.com/misteroneill
[BrandonOCasey]: https://github.com/BrandonOCasey
[estim]: https://github.com/estim
[seggev319]: https://github.com/seggev319
[mister-ben]: https://github.com/mister-ben
[kocoten1992]: https://github.com/kocoten1992
[nicolaslevy]: https://github.com/nicolaslevy
[MarcAMo]: https://github.com/MarcAMo
[mmcc]: https://github.com/mmcc
[knilob]: https://github.com/knilob
[forbesjo]: https://github.com/forbesjo
[odisei369]: https://github.com/odisei369
[readme]: https://github.com/videojs/video.js#quick-start
