---
title: "Autoplay Best Practices with Video.js"
author:
  name: "Pat O'Neill"
  github: misteroneill
tags:
---

## tl;dr
* Never assume autoplay will work.
* Using the `muted` attribute/option will improve the chances that autoplay will succeed.
* Prefer programmatic autoplay via the `player.play()` method, avoiding the `autoplay` attribute/option.

## Introduction
There has been a lot of chatter about autoplay lately and we wanted to take a few minutes to share some best practices when using autoplay with Video.js.

This post is meant to be a source of information - not an editorial or positional statement on autoplaying video. A lot of users hate autoplay because it's annoying or it consumes precious bandwidth. And a lot of publishers providing content free of charge rely on autoplay for the preroll ads that finance their businesses. Browser vendors (and open source library authors) have to weigh these concerns based on the interests of their users, publishers across the web, and their own business. And users have to choose the browser that best reflects their preferences.

This post is not about ways to circumvent autoplay policies. That's not possible and no one should waste their time attempting it. We think it's uncontroversial to say that actively circumventing the browser's built-in user experience or user preferences is harmful.

## Autoplay Policies in the Big Browsers
I'm not going to go into exhaustive detail on each browser's specific algorithms because they are subject to change and it would defeat the core point of this post: **never assume autoplay will work.**

### Safari
As of Safari 11, which shipped in September 2017, Apple has [updated their autoplay policies](https://webkit.org/blog/7734/auto-play-policy-changes-for-macos/) to prevent autoplay with sound on most websites.

### Chrome
Back in September 2017, Google announced that [Chrome's autoplay policy would change in April 2018 with Chrome 66](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes), subject to a series of rules you can read about in the linked article.

### Firefox
With Firefox, Mozilla has taken the position of not having a firm autoplay policy for the time being. That may change in the future. That said, Firefox today [does allow users to disable autoplay](https://support.mozilla.org/en-US/questions/1150702) with a few configuration changes.

### IE11 and Edge
Microsoft's browsers have no specific/known autoplay policy - at the time of writing, autoplay just works in IE11 and Edge.

## Refresher on How to Request Autoplay in Video.js
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

## Recommended Practices
By default, you can defer to the default behavior in Video.js. If autoplay succeeds, the Video.js player will begin playing. If autoplay fails, the Video.js player will behave as if autoplay were off - i.e. it will display the "big play button" component over the poster image (or a black box).

If that works for you, your job is done!

**TIP:** If you want to use autoplay and improve the chances of it working, use the `muted` attribute (or the `muted` option, with Video.js).

Beyond that, there are some general practices that may be useful when dealing with autoplay: detecting whether autoplay is supported at all, or detecting whether autoplay succeeds or fails.

### Detecting Autoplay Support
Similar to detecting successful or failed autoplay requests, it may be useful to perform autoplay feature detection on the browser before creating a player at all. In these cases, the [can-autoplay](https://github.com/video-dev/can-autoplay) library is the best solution. It provides a similar `Promise`-based API to the native `player.play()` method:

```js
canAutoplay.video().then(function(obj) {
  if (obj.result === true) {
    // Can autoplay
  } else {
    // Can not autoplay
  }
});
```

### Programmatic Autoplay and Success/Failure Detection
For those who care whether or not an autoplay request was successful, both Google and Apple have recommended the same practice for detecting autoplay success or rejection: listen to the `Promise` returned by the `player.play()` method (in browsers that support it) to determine if autoplay was successful or not.

This can be coupled with the `autoplay` attribute/option or performed programmatically by calling `player.play()`, but **we recommend avoiding the `autoplay` attribute/option altogether and requesting autoplay programmatically**:

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

**NOTE:** It's important to understand that using this approach, the Video.js `player.autoplay()` method will return `undefined` or `false`. If you expect your users or integrators to rely on this method, consider the following section.

#### Programmatic Autoplay with the `autoplay` Attribute/Option
When building a player for others to use, you may not always have control over whether your users include the `autoplay` attribute/option on their instance of your player. Thankfully, combining this with programmatic autoplay doesn't seem to have a significant effect on the behavior of playback.

Based on our experimentation, even if the browser handles the actual autoplay operation, calling `player.play()` after it's begun playing (or failed to play) does not seem to cause current browsers to trigger an extra `"play"` event in either Chrome or Firefox. It does seem that Safari 11.1 triggers `"playing"` and `"pause"` events each time `player.play()` is called and autoplay fails.

Still, if you have full control, **we recommend avoiding the `autoplay` attribute/option altogether and requesting autoplay programmatically**.

**NOTE:** Even if using the `autoplay` attribute/option *with* programmatic autoplay, the `player.autoplay()` method will return `undefined` until the player is ready.

#### Example Use-Case
At Brightcove, one thing we've done to improve the user experience of our Video.js-based player is to hide the "big play button" for players requesting autoplay until the autoplay either succeeds or fails. This prevents a periodic situation where the "big play button" flashes on the player for a moment before autoplay kicks in.

While our actual implementation has more complexity due to specific circumstances with our player, the gist of it is the same as [this functional JSBin demonstration.](https://jsbin.com/quqodek/edit?html,js,output)

## Conclusion
There may be decision points in your code related to whether autoplay succeeds or not; however, the practical reality is that all the paragraphs before this boil down to a single, essential concept:

**Never assume that your request to autoplay will succeed.**

Keep that in mind and you're golden. Even if the browsers stop allowing autoplay entirely, our hope is that recommending this approach will be somewhat future-proof.
