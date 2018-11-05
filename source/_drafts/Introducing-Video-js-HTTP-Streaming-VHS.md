---
title: Introducing Video.js HTTP Streaming (VHS)
author:
  name: Garrett Singer
  github: gesinger
tags:
  - vhs
---

"How do I get my video to play with Video.js?"

This is one of the most frequent questions we get when working on Video.js. And it's a good question.

If someone checks out a copy of Video.js, their content may play on one browser and not another. To get cross browser support for a specific type of content, they must stumble upon a relevant source handler (for instance, [videojs-contrib-hls](https://github.com/videojs/videojs-contrib-hls) for HLS content). Only after including the source handler will their video play across all major browsers.

We want Video.js to be easy to use, and that isn't the easiest setup. So we decided that it's time to address the issue.

This was the motivation behind integrating [Video.js HTTP Streaming](https://github.com/videojs/http-streaming) (nicknamed VHS) inside of Video.js 7 by default, and we wanted to take a moment to describe what VHS is, and how it's different from including source handlers and plugins like before.

# What is VHS?

VHS is a source handler forked from the [videojs-contrib-hls](https://github.com/videojs/videojs-contrib-hls) repository. While videojs-contrib-hls was originally meant to add HLS playback on all browsers, we realized that the engine could also play other formats just as well.

To prove this, we added a [DASH manifest parser](https://github.com/videojs/mpd-parser), and with a few minor changes, VHS played DASH content with the same codebase and API as it used for HLS.

And with the changing landscape of video technologies, if a new format comes around and gains popularity, with the addition of a small manifest parser and maybe a few minor code changes, we expect it to play via VHS as well. One engine and API for all formats.

# Why is VHS included by default in Video.js?

Video.js was built to abstract away differences in video APIs and features between web browsers, creating one simple API as close to the web standards as possible, and filling in feature gaps where possible.

Over time, one area where browsers have diverged is in their support of different media formats. Some browsers may support native playback of DASH, others of HLS, and others of neither.

Previously, this would be managed by including source handlers and plugins, but we understand the importance of having a simple setup. By including VHS by default in Video.js, you no longer have to worry about what browser supports what streaming technology. It should just work.

# Video.js Core

If you would rather include a different source handler for HLS or DASH playback, Video.js still allows you to do that. [Video.js core](https://blog.videojs.com/video-js-7-is-here/#VHS-and-Video-js) is the Video.js you know and love, without the inclusion of VHS. Everything should work as it used to.

# Contributions and Feedback Welcome

As always, we’d love your contributions and feedback. The best way to reach us for comments, questions, requests, contributions, or just to say hello are the [VHS GitHub page](https://github.com/videojs/http-streaming) and the #playback channel on the [Video.js slack](http://slack.videojs.com/).
