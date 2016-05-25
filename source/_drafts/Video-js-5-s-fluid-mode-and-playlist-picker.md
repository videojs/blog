---
title: Video.js 5's fluid mode and playlist picker
tags:
  - videojs 5
  - fluid
---

## How it works
In videojs 5.0, we added support for truely fluid layouts with video.js.
You can see an example of it on the [videojs website][website].

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
This works great if you only have the player by itself. What if you are trying to a attach a playlist to the video element and keep it at the same height? Like we did on the [advanced example page][advanced-example] on the videojs website. Like so:
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

### How to make it fluid
As you can see in the preceeding screenshot, it isn't aligned correctly with the player like in the screenshot above.
Video.js calculates the aspect ratio and then adds a stylesheet to the page:
```css
.preview-player-dimensions.vjs-fluid {
  padding-top: 41.66666666666667%;
}
```
That percentage is because the aspect ratio for our oceans clip is 2.4.

So, to make sure that the playlist picker is the same height, we can just add the player dimensions class to it:
```html
<ol class="vjs-playlist preview-player-dimensions vjs-fluid"></ol>
```

### How to make it line up
One of the easiest ways of making these two things line up correctly is to use flexbox. It'll let you make the player and playlist picker grow to fill up as much space as needed and shrink down. It'll also let you have the playlist picker collapse underneath the player if the width of the page is too small.
Flexbox is [available][available-flex] on a lot of platforms currently but it is probably best to run this css through something like [autoprefixer][].

First, we set `display` to `flex` and add some properties for wrapping and sizing:
```css
.main-preview-player {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
```
`flex-wrap` allows playlist picker to wrap to the next line if the width of the container is too small.
Then we want to position the player and playlist picker relatie to the container and set some default and minimum sizes:
```css
.video-js,
.vjs-playlist {
  position: relative;
  min-width: 300px;
  min-height: 150px;
  height: 0;
}
```
And finally, we want to apply the flex setting to the player and playlist picker:
```css
.video-js {
  flex: 3 1 70%;
}

.vjs-playlist {
  flex: 1 1 30%;
}
```
This tells the player to grow and take up 3x the space as the playlist picker and defaults to 70% of the width. The playlist picker itself is defaults to 30% of the width and is allowed to grow and shrink as necessary.
Now if we load this in a browser we see a problem. The playlist isn't the right height:
![](videojs-playlist-small.png)
This is because the playlist-ui plugin sets its own padding on the element that ends up overriding the `preview-player-dimensions` `padding-top`. We can fix this by forcing the `padding-top` we want. However, while this solves our height problem, where are our items? Oh, you need to scroll to get them. That seems less than ideal.
![](videojs-playlist-padding.png)
This happens because our `padding-top` is inside the playlist picker; it pushed all the elements down requiring scrolling to get to them.

### The final solution
Ultimately, what we need to do is wrap the playlist element in a container that flexes so that the `padding-top` doesn't push the playlist items down.
```html
<div class="playlist-container  preview-player-dimensions vjs-fluid">
  <ol class="vjs-playlist"></ol>
</div>
```
We also need to change the `vjs-playlist` references above to be `playlist-container`. We also want to absolutely position the playlist inside its container:
```css
.playlist-container {
  position: relative;
  min-width: 300px;
  min-height: 150px;
  height: 0;
}

.playlist-container {
  flex: 1 1 30%;
}

.vjs-playlist {
  margin: 0;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
```
Now we have what we were looking for:
![](videojs-with-playlist.png)

### Altogether now
![](responsive-playlist.gif)
The HTML:
```html
<section class="main-preview-player">
  <video id="preview-player" class="video-js vjs-fluid" controls preload="auto" crossorigin="anonymous">
    <p class="vjs-no-js">To view this video please enable JavaScript, and consider upgrading to a web browser that <a href="http://videojs.com/html5-video-support/" target="_blank">supports HTML5 video</a></p>
  </video>

  <div class="playlist-container  preview-player-dimensions vjs-fluid">
    <ol class="vjs-playlist"></ol>
  </div>
</section>
```
And the CSS:
```css
.main-preview-player {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.video-js,
.playlist-container {
  position: relative;
  min-width: 300px;
  min-height: 150px;
  height: 0;
}

.video-js {
  flex: 3 1 70%;
}

.playlist-container {
  flex: 1 1 30%;
}

.vjs-playlist {
  margin: 0;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
```

[website]: //videojs.com
[advanced-example]: //videojs.com/advanced/
[ratios]: http://alistapart.com/article/creating-intrinsic-ratios-for-video
[playlist]: https://github.com/brightcove/videojs-playlist
[playlist-ui]: https://github.com/brightcove/videojs-playlist-ui
[autoprefixer]: https://github.com/postcss/autoprefixer
[available-flex]: http://caniuse.com/#feat=flexbox
