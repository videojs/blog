---
title: 'Bugpost: Disconnects and Reconnects'
author:
  name: Garrett Singer
  github: gesinger
tags:
  - vhs
  - videojs-http-streaming
  - http-streaming
  - playback
  - bugs
---

In my old apartment, whenever I nuked a bowl of oats in the microwave, my WIFI would die. It was an annoyance, but I was hungry, the apartment was too small to move the router further from the microwave, and I didn’t want to buy anything that operated on a different frequency. So I’d start the microwave, watch whatever video was streaming on my laptop until the buffer ran out, then watch the loading spinner until the microwave gave the triple ding.

Thankfully, video players are usually pretty robust against such issues as network connectivity (or rogue microwaves), and the video resumed after the network recovered. But recently, we found that for some content played in Video.js, the video wasn’t resuming. Even worse, the loading spinner simply disappeared, leaving a frozen frame on screen.

You can see this behavior yourself by checking out Video.js 7.3.0, loading https://dash.akamaized.net/akamai/bbb_30fps/bbb_30fps.mpd as the source, and using Chrome Dev Tools to disconnect and reconnect the network.

Here’s a quick tour of what happened and how we fixed it.

## The Loading Spinner

To approach the bug, we started with the most glaring problem, the loading spinner disappearing. The spinner is managed by Video.js. Upon looking at [the code](https://github.com/videojs/video.js/blob/v7.3.0/src/js/player.js#L1598), it can be seen that the `vjs-waiting` class is added to the HTML element when the tech triggered a `waiting` event, and removed the `vjs-waiting` class on the next `timeupdate` event.

```
/**
 * Retrigger the `waiting` event that was triggered by the {@link Tech}.
 *
 * @fires Player#waiting
 * @listens Tech#waiting
 * @private
 */
handleTechWaiting_() {
  this.addClass('vjs-waiting');
  /**
   * A readyState change on the DOM element has caused playback to stop.
   *
   * @event Player#waiting
   * @type {EventTarget~Event}
   */
  this.trigger('waiting');
  this.one('timeupdate', () => this.removeClass('vjs-waiting'));
}
```

After logging the events emitted by the player, the issue became clear. Some browsers would emit a `timeupdate` event immediatley after the `waiting` event.

So `timeupdate` events themselves were not reliable, but what else could be used to make a better determination of if we're waiting? We checked `player.currentTime()` on the `timeupdate` events, and found that when we caught the last `timeupdate` event, the one after the `waiting` event, the player was at the same time as the `timeupdate` event prior to the `waiting` event (and the same time as when the `waiting` event was triggered).

```
`timeupdate` triggered, player.currentTime = 11
`timeupdate` triggered, player.currentTime = 11.250
`waiting` triggered, player.currentTime = 11.250
`timeupdate` triggered, player.currentTime = 11.250
```

The fix was pretty easy. Simply record the player's time when it gets the `waiting` event, and instead of removing the `vjs-waiting` class on the next `timeupdate`, remove the `vjs-waiting` class on the next `timeupdate` that has a different time from the one we recorded.

You can see the PR for that [here](https://github.com/videojs/video.js/pull/5533), and it is available in Video.js 7.4.0.

## Resuming Playback after Disconnect

With the loading spinner back, the next issue was to determine why, when the network reconnected, the video did not resume. First, we tested a few different pieces of content. The HLS source we tested did reconnect, but the DASH source didn't. Worse, when using the DASH source, Chrome's debug console froze, as it racked up request and console errors as fast as the computer could process them.

### Console Crash

Imagine your console filled with the following two messages repeated thousands of times:
```
GET https://dash.akamaized.net/akamai/bbb_30fps/bbb_a64k/bbb_a64k_8.m4a net::ERR_INTERNET_DISCONNECTED
VIDEOJS: WARN: Problem encountered with the current HLS playlist. Trying again since it is the final playlist.
```

When your console freezes, debugging can be tough. It's like trying to watch a video when the microwave is on. So we tackled that issue first.

Looking at the error message, we were retrying the last rendition over and over. That is expected behavior, as we added that in https://github.com/videojs/videojs-contrib-hls/pull/1039 (a long time ago). However, DASH uses a different playlist loader than HLS, and we never added the same logic for reloading the final DASH playlists.

After [adding the throttle](https://github.com/videojs/http-streaming/pull/277), debugging became much easier, and we were able to use the console once again and dig into why DASH didn't reconnect while HLS did.

### Multiple Requests Multiple Problems

The last problem proved to be the most difficult to debug. Initially, the thought was that demuxed content was the problem, as having a separate audio segment loader to load from the audio playlist could cause trouble when experiencing errors at the same time as the main segment loader tried to load from the video playlist. However, HLS can also have demuxed content, and after an example source proved to resume just fine, that approach was ruled out.

But there were more than just two requests for DASH. By examining Chrome's network log, the DASH content, after a certain point, would stop requesting video segments, and only request audio segments. Further, the video segments included two requests: one for the init segment, and the other for the segment itself.

Requests in VHS are managed by a module called `media-segment-request`, which is responsible for requesting everything required for a segment, whether it be the key, the init segment, the segment itself, or all of them, before letting the segment loader know that its requests are done. And for the video requests, the `media-segment-request` was never calling the done callback after the disconnect.

It turned out that if we received one error, we would abort the other requests in that group, and we waited to call the `done` callback until those requests reported that they had been aborted. By the [standards](https://xhr.spec.whatwg.org/#the-abort()-method) though, the abort algorithm may not be run if the request was unsent. In this case, when the network was disconnected, the first request would report an error before the second had a chance to be sent, and so after we aborted the second request, we'd be stuck waiting for an error that would never come.

We fixed that by [immediately calling back with an error on the first error we encounter](https://github.com/videojs/http-streaming/pull/286) instead of waiting for each subsequent request to finish.

The problem should also have existed in HLS, but only in cases where the segments had an EXT-X-KEY or EXT-X-MAP tag to go along with the media request. In the content we tried, neither of those was present, so it appeared to be isolated to DASH content.

## Bug Solved

We hope that was somewhat useful in describing some of the bugs we encounter, how we go about investigation, and what the resolutions look like. We'd be happy to hear from you as well. We're always around on the #playback channel in the [Video.js Slack](http://slack.videojs.com/), or you can find us navigating Issues and PRs at https://github.com/videojs/http-streaming.

Now to enjoy my oats and video, without any concerns about the playback resuming.
