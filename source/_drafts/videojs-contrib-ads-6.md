---
title: videojs-contrib-ads 6
author:
  name: Greg Smith
  github: incompl
tags:
---

A major update to videojs-contrib-ads has been released. In this post, we'll take a look at what videojs-contrib-ads offers and what has improved in this latest version.

## What is contrib-ads?

Videojs-contrib-ads is a framework for creating Video.js ad plugins. Seamlessly integrating ad support into a video player can be a daunting task, especially if you have other plugins that may be effected. For example, playing ads may result in additional [media events](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Media_events). Imagine an analytics system that listens for "loadstart" events: it would start seeing double when extra `loadstart` events result from preroll ads. contrib-ads has a feature called [Redispatch](https://github.com/videojs/videojs-contrib-ads#redispatch) that makes sure ads do not trigger extra media events. This keeps things simple as possible without breaking other plugins. [Additional benefits of using contrib-ads are listed in the readme](https://github.com/videojs/videojs-contrib-ads#benefits).

## Version 6

Version 6 of contrib-ads is a maintenance release that includes a major code refactor. The core of the project's behavior is handled by a state machine that advances through various states as the player plays content, preroll ads, midroll ads, and postroll ads. The initial state machine was designed to implement a specific set of functionality when the project was first conceptualized. Over the years, the project became much more fully-featured and many new scenarios and edge-cases arose. In this refactor, the state machine was updated to precisely match the modern feature-set. This has resulted in a more maintainable and reliable codebase.

## How it works

For a history of how the state machine has evolved over time, [there is detailed information in this Github issue](https://github.com/videojs/videojs-contrib-ads/issues/320). In this article we're going to focus on how it works in version 6.

Here is a diagram of the current states:

![](./ad-states.png)

There are two types of states: content states (blue) and ad states (yellow). `player.ads.isInAdMode()` returns true if the current state is an ad state. [Ad mode](https://github.com/videojs/videojs-contrib-ads#ad-mode-definition) is defined as _if content playback is blocked by the ad plugin_. This does not necessarily mean that an ad is playing. For example, if there is a preroll ad, after a play request we show a loading spinner until ad playback actually begins. This is considered part of ad mode.

Let's walk through the states for a simple preroll scenario. The plugin begins in BeforePreroll state. This is considered a content state because content playback hasn't been requested yet and so ads are not blocking playback. However, the ad plugin can still asynchronously request ads from an ad server during this time. The ad plugin triggers the `adsready` event to say that it's ready. Contrib-ads knows that once the play button is pressed, ads are already prepared.

Now, the user clicks play. At this point ad mode begins and contrib-ads moves forward to the Preroll state. The Preroll state shows a loading spinner while ads load. Once the ad break begins and an ad plays, the control bar turns yellow and will not allow seeking. If the ad takes too long to begin playing, a timeout will occur and content will resume without further adieu. When the ad is complete, contrib-ads restores the content video. Only when content playback results in a `playing` event does ad mode end.

Now we're in the ContentPlayback state. At this point, the ad plugin could play a midroll ad, causing a brief foray into the Midroll state, or content could continue without interruption.

When content ends, normally we would see an `ended` event right away. Instead, contrib-ads sends a `contentended` event, which is the ad plugin's chance to play postroll ads. Perhaps the ad plugin has decided not to play postroll ad, so it responds by triggering `nopostroll`. contrib-ads knows that now we're really done, so an `ended` event is triggered.

Finally, since content has ended for the first time, we now transition to the AdsDone state. There will be no more ads, even if the user seeks backwards and plays through the content again.

## Conclusion

Ads may not be your favorite part of watching video online, but contrib-ads makes sure that they don't break your site and that content plays even if ads fail. If you're interested in learning more or contributing to the project, [check it out on Github](https://github.com/videojs/videojs-contrib-ads)!
