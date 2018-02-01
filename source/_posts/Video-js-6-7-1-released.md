---
title: Video.js 6.7.1 released
author:
  name: Gary Katsevman
  github: gkatsev
date: 2018-02-01 15:40:55
tags:
---

Video.js 6.7.1 was released this week. This comes a month and a half since the first [6.6.0 release][6.6.0] (which we forgot to blog about) and only a week since the latest patch release ([6.6.3][], which is now latest).

6.7 has two awesome new features and a couple helpers:
- a working `playerresize` event
- a new mediator type for middleware
- `getPlayer` and `getAllPlayers` helper on `videojs`.

## Netlify

Recently, we also switched over all our online properties to run on [Netlify][]. This is great because it gives us HTTPS via [Let's Encrypt][encrypt] but also allows us much better automation for the website, docs website, and blog. Since the docs website is tied to the main Video.js repo and has a build per PR, we also had Netlify generate an example page for the PR based on the sandbox examples. [Here's the example page][example] for a [recent PR][].

## `playerresize`

This new event will fire each time the player is resized. It will fire when going fullscreen and when exiting fullscreen and when resizing the player via the dimension methods or if the player is in fluid mode and the window is resized.
It uses the new [ResizeObserver][] in Chrome 64 and wherever it is available. If it isn't available, a polyfill can be passed in or it will use its fallback. The fallback uses an absolutely positioned, hidden iframe that's the size of the player and then retriggers the iframe's `resize` event on a debounced handler.
Also, I wanted to note that the `resize` player event does not refer to the size of the player itself but rather to the native [`resize` event][resize], which is triggered when the videoHeight or videoWidth has changed.

## Mediator type for middleware

This is the first major feature for middleware since they were released in 6.0. The  main reason for the mediator is for middleware to be able to cancel requests to `play()`. Thus, they are currently limited to `play()` and `pause()` calls currently. Once we iron out the details we hope to enable it for further functionality.
The mediator middleware allows you to intercept the calls to the mediator methods, like `play()`, and then prevent the calls from occuring on the tech. This is important for methods like `play()` because while calling `pause()` immediately after `play()` may succeed in "cancelling" playback, it also has some unintended side-effects in a few cases. The mediator middleware can decide whether `play()` should go through, thus eliminating the need to call `pause()`. Afterwards, all middleware will be notified whether the call was terminated or it will be given the return value. In the case of `play()`, it will be the play promise itself.

Here's a simple example:
```js
videojs.use('*', (player) => ({
  setSource(src, next) {
    next(null, src);
  }

  callPlay() {
    // return this value to terminate the method call
    return videojs.middleware.TERMINATOR;
  }

  play(terminated, playPromise) {
    if (terminated) {
      return console.error(terminated, 'play was terminated');
    }

    playPromise
    .then(() => console.log('play succeeded!'))
    .catch(() => console.log('play failed :('));
  }
});
```

## Player helpers

These are to make it easier to manage your players and reduce unintended side-effects. A lot of times, a player is being created automatically via `data-setup` but then to refer to it via code, `videojs()` is called. In some cases, this will actually initialize the player because it ended up running before the auto setup. Now, `videojs.getPlayer()` can be used instead and it will never create the player. It is the preferred way of getting the player once it has been created.
`videojs.getAllPlayers()` is just a nice way of getting a list of all the players that are currently available on the page.

## Committers
* Gary Katsevman [@gkatsev][gkatsev]
* Pat O'Neill [@misteroneill][misteroneill]
* [@ldayananda][ldayananda]
* [@mister-ben][mister-ben]

## Raw CHANGELOG

<a name="6.7.1"></a>
## [6.7.1](https://github.com/videojs/video.js/compare/v6.7.0...v6.7.1) (2018-01-31)

### Bug Fixes
* **middleware**: do a null check in mediator methods ([#4913](https://github.com/videojs/video.js/issues/4913)) ([7670db6](https://github.com/videojs/video.js/commit/7670db6))

<a name="6.7.0"></a>
# [6.7.0](https://github.com/videojs/video.js/compare/v6.6.3...v6.7.0) (2018-01-30)

### Features

* Add `getPlayer` method to Video.js. ([#4836](https://github.com/videojs/video.js/issues/4836)) ([a15e616](https://github.com/videojs/video.js/commit/a15e616))
* Add `videojs.getAllPlayers` to get an array of players. ([#4842](https://github.com/videojs/video.js/issues/4842)) ([6a00577](https://github.com/videojs/video.js/commit/6a00577))
* add mediator middleware type for play() ([#4868](https://github.com/videojs/video.js/issues/4868)) ([bf3eb45](https://github.com/videojs/video.js/commit/bf3eb45))
* playerresize event in all cases ([#4864](https://github.com/videojs/video.js/issues/4864)) ([9ceb4e4](https://github.com/videojs/video.js/commit/9ceb4e4))

### Bug Fixes

* do not patch canplaytype on android chrome ([#4885](https://github.com/videojs/video.js/issues/4885)) ([f03ac5e](https://github.com/videojs/video.js/commit/f03ac5e))

### Chores

* generate a test example on netlify for PRs ([#4912](https://github.com/videojs/video.js/issues/4912)) ([8b54737](https://github.com/videojs/video.js/commit/8b54737))
* **package:** update dependencies ([#4908](https://github.com/videojs/video.js/issues/4908)) ([dcab42e](https://github.com/videojs/video.js/commit/dcab42e))

### Documentation

* Update COLLABORATOR_GUIDE.md and CONTRIBUTING.md to include label meanings ([#4874](https://github.com/videojs/video.js/issues/4874)) ([a345971](https://github.com/videojs/video.js/commit/a345971))

### Tests

* add project and build names to browserstack ([#4903](https://github.com/videojs/video.js/issues/4903)) ([41fd5cb](https://github.com/videojs/video.js/commit/41fd5cb))


[6.6.3]: https://github.com/videojs/video.js/releases/tag/v6.6.3
[6.6.0]: https://github.com/videojs/video.js/releases/tag/v6.6.0
[ResizeObserver]: https://github.com/WICG/ResizeObserver
[Netlify]: https://www.netlify.com/
[example]: https://deploy-preview-4916--videojs-docs.netlify.com/test-example/
[recent PR]: https://github.com/videojs/video.js/pull/4916
[gkatsev]: https://github.com/gkatsev
[misteroneill]: https://github.com/misteroneill
[mister-ben]: https://github.com/mister-ben
[ldayananda]: https://github.com/ldayananda
[resize]: https://html.spec.whatwg.org/multipage/media.html#event-media-resize
[encrypt]: https://letsencrypt.org/
