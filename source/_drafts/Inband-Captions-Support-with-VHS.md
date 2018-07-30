---
title: Inband Captions Support with VHS
author:
  name: Lahiru Dayananda
  github: ldayananda
tags:
---

With the release of [videojs-http-streaming](https://github.com/videojs/http-streaming) v1.2.0 on July 16th 2018, we now have in-built support for CEA-608 captions carried in FMP4 segments. This means that closed captions are automatically parsed out and made available to video.js players for MPEG-DASH content and HLS streams using FMP4 segments.

Here's an example of what it looks like:

```js
example will be here
```

videojs-http-streaming is included by default with videojs v7.2.0, which is currently released as `next` on npm.

To get v7.2.0 via npm:

```sh
npm install video.js@next
```