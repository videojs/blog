---
title: "Autoplay Best Practices with Video.js"
author:
  name: "Pat O'Neill"
  github: misteroneill
tags:
---

There has been a lot of chatter about autoplay lately and we wanted to take a few minutes to share some best practices when using autoplay with Video.js.

This post is meant to be a source of information - not an editorial or positional statement on autoplaying video. A lot of users hate autoplay. And a lot of publishers providing content free of charge rely on autoplay for the preroll ads that finance their businesses. Browser vendors (and open source library authors) have to weigh these concerns based on the interests of their users, publishers across the web, and their own business. And users have to choose the browser that best reflects their preferences.

This post is not about ways to circumvent autoplay policies. That's not possible and no one should attempt it. One positional statement everyone working on Video.js should be comfortable making is: actively circumventing the browser's built-in user experience or user preferences is harmful.

## Autoplay in the Big Browsers
I'm not going to go into exhaustive detail on each browser's idiosyncrasies because that defeats the point: **never assume autoplay will work.**

### Chrome
Back in September 2017, Google announced that [Chrome's autoplay policy would change in April 2018 with Chrome 66](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes). In short, muted autoplay is allowed, but autoplay with sound will be subject to a series of rules depending on platform and previous video engagement (referred to as the "Media Engagement Index") with the specific domain.

### Safari
As of Safari 11, which shipped in September 2017, Apple has [updated their autoplay policies](https://webkit.org/blog/7734/auto-play-policy-changes-for-macos/) to prevent autoplay with sound on most websites.

### Firefox
Firefox takes a simpler approach, allowing users to disable autoplay across the board with a configuration change - the `media.autoplay.enabled` setting in `about:config` can be set to `false`.

### IE11 and Edge
Autoplay always works in Microsoft's browsers.

## How to Request Autoplay in Video.js
Note how the title of this section uses the word "request." That's intentional because it should be thought of as a request. Remember, **never assume autoplay will work.**

One of Video.js' strengths is that it is designed around the native `<video>` element interface; so, it will defer to the underlying playback technology (in most cases, HTML5 video) and the browser. In other words, _if the browser allows it, Video.js allows it_.

There are two ways to enable autoplay when using Video.js:

1. Use the `autoplay` attribute on either the `<video>` element or the new `<video-js>` element.
   
   ```html
   <video autoplay src="https://path/to/source.mp4"></video>
   <video-js autoplay src="https://path/to/source.mp4"></video-js>
   ```

1. Use the `autoplay` option:

   ```js
   videojs('player', {autoplay: true});
   ```

## Recommended Practice
By default, you can defer to the default behavior in Video.js. If autoplay succeeds, the Video.js player will begin playing. If autoplay fails, the Video.js player will behave as if autoplay were off - i.e. it will display the "big play button" component over the poster image (or a black box).

If that works for you, your job is done!

For those who *do* care whether or not autoplay was successful, both Google and Apple have recommended the same practice for detecting autoplay success or rejection:

* Use the `muted` attribute (or the `muted` option, with Video.js) when using autoplay. This will lead to a successful autoplay in more cases.
* Listen to the `Promise` returned by the `play()` method to determine if autoplay was successful or not.

Further, specifically when using Video.js, we recommend the following guidelines:

* Do not refer to the `player.autoplay()` return value until the player is ready (using a `ready()` callback or on the `"ready"` event) - it will often be `undefined` before that point. Really, most things should wait for the player to be ready.

Otherwise, just like the native `<video>` element, the Video.js player will return a `Promise` from the `play()` method - if the browser returns one - or `undefined` if it doesn't. So, the Google/Apple recommended practice applies with very slight modification:

```html
<video-js id="player" src="https://path/to/source.mp4"></video-js>
```

```js
var player = videojs('player');

player.ready(function() {
  var promise = player.play();

  if (promise !== undefined) {
    promise.then(function() {
      // Autoplay started!
    }).catch(function(error) {
      // Autoplay was prevented.
    });
  }
});
```

**NOTE:** Even if the browser handles the actual autoplay operation, calling `play()` while it's playing does not seem to cause current browsers to trigger an extra `"play"` event in either Chrome or Firefox. Oddly, it does seem that Safari triggers `"playing"` and `"pause"` events each time `play()` is called.

## Example Use-Case for Autoplay Detection
At Brightcove, one thing we've done to improve the user experience of our Video.js-based player is to hide the "big play button" for players requesting autoplay until the autoplay either succeeds or fails. This prevents a periodic situation where the "big play button" flashes on the player for a moment before autoplay kicks in.

While our actual implementation has more complexity due to specific circumstances with our player, the gist of it is the same as [this functional JSBin demonstration.](https://jsbin.com/quqodek/edit?html,js,output)
