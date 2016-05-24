---
title: Video.js 5's fluid mode and playlist picker
tags:
  - videojs 5
  - fluid
---

## How it works
In videojs 5.0, we added support for truely fluid layouts with video.js.

It is done by using [intrinsic ratios][ratios]. In short, you make the video element take up 100% `width` and make the `height` be `0`.  We can set the player `width` to a particular size, say 50%; this way, as we resize the the browser window, the player should stay 50% the same of the window but keep it's aspect ratio for the video. The main trick is that in addition to setting the `width` on the player, we also apply a padding. Specifically, we want to set the following:
```css
.video-js {
  padding-top: 56.25%
}
```
This percentage is the aspect ratio of height with respect to width in a 16:9 video, the standard aspect ratio for most content now days.  What makes this work is that padding is set with respect to the width of the element, so the height will always be 56.25% of the width maintaining the aspec ratio that we want.

## How to use it in video.js
In video.js, to make a player fluid, you can either set the `fluid` option
```js
let player = videojs('preview-player', {
  fluid: true
});
```
Or you can add one of the fluid classes to the player: `.vjs-fluid`, `.vjs-4-3`, `.vjs-16-9`:
```html
<video id="preview-player" class="video-js vjs-fluid" controls data-setup={}>
```
`.vjs-4-3` maintains a 4:3 aspect ratio for the video and `.vjs-16-9` maintains a 16:9 one.  `.vjs-fluid` is a bit more special. It waits for the video metadata to load and then uses the video width and video height to calculate the correct aspect ratio to use for the video.

## Playlist picker
This works great if you only have the player by itself. What if you are trying to a attach a playlist to the video element and keep it at the same height? Like so:
![](videojs-with-playlist.png)

We *could* calculate how much the padding top should be depending on the width of the playlist picker or the container element but then each time a video changes we would need to recalculate the height of the playlist picker. Instead, we can rely on videojs to do all the work.

### Attaching the playlist picker
For this example, I'm using the [videojs-playlist-ui][playlist-ui] and [videojs-playlist][playlist] plugins for the playlist functionality.
I then wrap the player in a container and put the playlist-ui element in there as well.
```html
<section class="main-preview-player">
  <video id="preview-player" class="video-js vjs-fluid" controls preload="auto" crossorigin="anonymous">
    <p class="vjs-no-js">To view this video please enable JavaScript, and consider upgrading to a web browser that <a href="http://videojs.com/html5-video-support/" target="_blank">supports HTML5 video</a></p>
  </video>

  <ol class="vjs-playlist"></ol>
</section>
```

Now we can relatively quickly and make them align together with some CSS:
```css
.video-js {
  width: 70%;
  float: left;
}
.vjs-playlist {
  width: 30%;
  float: right;
}
```
Note: this is super simplistic and probably shouldn't be used in real pages

![](videojs-playlist-not-fluid.png)


[ratios]: http://alistapart.com/article/creating-intrinsic-ratios-for-video
[playlist]: https://github.com/brightcove/videojs-playlist
[playlist-ui]: https://github.com/brightcove/videojs-playlist-ui
