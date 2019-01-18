---
title: In-band Captions Support with videojs-http-streaming
tags:
  - vhs
  - videojs-http-streaming
  - playback
  - captions
  - cea-608
  - 608
author:
  name: Lahiru Dayananda
  github: ldayananda
date: 2019-01-16 18:36:32
---


With the release of [videojs-http-streaming](https://blog.videojs.com/introducing-video-js-http-streaming-vhs/) (VHS) v1.2.0 on July 16th 2018, Video.js has built-in support for CEA/CTA-608 captions carried in FMP4 segments. This means that closed captions are automatically parsed out and made available to Video.js players for MPEG-DASH content and HLS streams using FMP4 segments. Here's a [sample player](https://jsfiddle.net/ugnkw65y/) that shows the captions (English with the "CC" button is the track with CEA-608 captions).

{% iframe https://jsfiddle.net/ugnkw65y/1/embedded/result,html/light 100% 550 %}

If you are curious about CEA-608 captions and the approach we used to parse them out of fmp4s, or a general overivew, you can watch my [talk from Demuxed 2018](https://www.twitch.tv/videos/326082416?collection=u1vmyYMIYBXvlQ).

{% youtube 0yTYNIajBQM %}

Caption Parsing is handled by the [mux.js](https://github.com/videojs/mux.js) library and interacts with VHS to feed parsed captions back to Video.js.

## Usage

Create a CaptionParser:

```js
  import { CaptionParser } from 'mux.js/lib/mp4';

  const captionParser = new CaptionParser();

  // initalize the CaptionParser to ensure that it is ready for data
  if (!captionParser.isInitialized()) {
    captionParser.init();
  }
```

When working with HLS and MPEG-DASH with fmp4 segments, it's likely that not all the information needed to parse out captions are included in the media segments themselves, and metadata from the `init` segment needs to be passed to the CaptionParser. For this reason, the video trackIds and timescales defined in the `init` segment should be passed into the CaptionParser.

```js
  import mp4probe from 'mux.js/lib/mp4/probe';

  // Typed array containing video and caption data
  const data = new Uint8Array();
  // Timescale = 90000
  const timescales = mp4probe.timescale(data);
  // trackId = 1
  const videoTrackIds = mp4probe.videoTrackIds(data);

  // Parsed captions are returned
  const parsed = captionParser.parse(
    data,
    videoTrackIds,
    timescales
  );
```

Calling `captionParser.parse` with data containing CEA-608 captions will result in an object with this structure:

```js
  {
    captions: [
      {
        // You can ignore the startPts and endPts values here
        startPts: 90000,
        endPts: 99000,
        // startTime and endTime can be used for caption times in the TextTrack API
        startTime: 1,
        endTime: 1.1,
        text: 'This is a test caption',
        // This is the CEA-608 "channel" the caption belongs to
        stream: 'CC1'
      }
    ],
    // Includes any (or none) of: CC1, CC2, CC3, CC4
    // This should match the captions returned in the above caption array
    captionStreams: {
      CC1: true
    }
  };
```

You can then take one caption and create a `VTTCue` to add to a `TextTrack` with Video.js APIs

```js
  const player = videojs('video');
  const track = player.addRemoteTextTrack({
    kind: 'captions',
    label: parsed.captions[0].stream,
    default: true,
    language: 'en'
  }, false);

  track.addCue(parsed.captions[0]);
```

VHS is included by default with videojs v7.x.
