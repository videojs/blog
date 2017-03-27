---
title: 'Feature Spotlight: Middleware'
author:
  name: Gary Katsevman
  github: gkatsev
tags:
  - middleware
  - video.js 6.0
---

Middleware is one of the cool new features that is coming to Video.js in version 6.0

With middleware, you are now able to interact with and change how the player and the tech talk to each other. The tech is Video.js's abstraction from the player, separating the player API from the playback technology. With techs, we can plug things like a Flash fallback or a Youtube embed into Video.js without changing the external API or the look-and-feel of the player.

{% pullquote right %}
Video.js middleware are like Express middleware but routes are based on video MIME types.
{% endpullquote %}

A lot of Video.js users may be familiar with middleware from projects like Express. Video.js middleware isn't that different from those. In both cases you register your middleware against a particular route to call down the chain when the route is triggered. In Express, routes are based on the url paths. In Video.js, these routes are based on the video MIME type. And, like Express, there are "star" (`*`) middleware that match all routes.

There are two important pieces to be aware of with middleware:
1. dynamic source handling
2. intercepting the player and tech interactions.

## A Video Catalog

With the dynamic source handling, you could load video with a custom type and source and resolve it asynchronously. A good example for this is a video catalog system. The page can be rendered with a specific catalog ID and a special MIME type, like so:
```html
<video controls class="video-js">
  <source src="123" type="video/my-catalog">
</video>
```
Then, you could register a middleware for that route -- the type `video/my-catalog`.
```js
// middleware methods get the player instance as an argument
videojs.use('video/my-catalog', function(player) {

  // middleware are expected to return an object with the methods on it.
  // It can be a plain object or an instance of something.
  return {

    // setSource allows you to tell Video.js whether you're going to be handling the source or not
    setSource(srcObj, next) {
      const id = srcObj.src;

      videojs.xhr({
        uri: '/getVideo?id=' + id
      }, function(err, res, body) {

        // pass null as the first argument to say everything is going fine and we can handle it.
        next(null, {
          src: body.sourceUrl,
          type: body.sourceType
        })
      });
    }
  };
});
```
Then, when Video.js initializes, it'll go through and call the middleware that are set for `video/my-catalog`.

## Server-Side Ad Insertion

Server Side Ad Insertion (SSAI) is a great fit for middleware. It showcases the ability to intercept the play and tech interactions. For example, you have a 30 seconds ad followed by a five minute video in your HLS manifest. You want the timeline to display the ad time and the content time appropriate when each is playing. Right now, the duration displayed will be the combined duration of five minutes and 30 seconds (`5:30`). The solution is to add a middleware that knows when the ad is being played and tells the player that the duration is 30 seconds and when the content is playing that the duration is five minutes.
```js
// register a star-middleware because HLS has two mimetypes
videojs.use('*', function(player) {
  return {
    setSource(srcObj, next) {
      const type = srcObj.type;

      if (type !== 'application/x-mpegurl' && type !== 'application/vnd.apple.mpegurl') {

        // call next with an error to signal you cannot handle the source
        next(new Error('Source is not an HLS source'));
      } else {

        // in here we know we're playing back an HLS source.
        // We don't want to do anything special for it, so, pass along the source along with a null.
        next(null, srcObj);
      }
    },

    // this method gets called on the tech and then up the middleware chain providing the values as you go along
    duration(durationFromTech) {
      if (areWeCurrentlyPlayingAnAd(durationFromTech)) {
        // since we're now in an ad, return the ad duration
        // in a real example you'd calculate this based on your playlist
        // rather than hardcode a value in here
        return 30;
      } else {
        // we're playing back content, so, return that duration
        return 5 * 60;
      }
    }
  }
});
```

## Playbackrate Adjustment - A Case Study

A simple but interesting middleware to look at is the [playbackrate-adjuster][pra]. This middleware will change the times of the controls depending on the current rate. For example, if you're playing back a 20 minute video and change the rate to 2x, the controls will adjust to display 10 minutes. Let's take a look at the code.
```js
videojs.use('*', function(player) {
  /* ... */

  return {
    setSource(srcObj, next) {
      next(null, srcObj);
    },

    duration(dur) {
      return dur / player.playbackRate();
    },

    /* ... */
  };
});
```
So, here, we attach a star-middleware because we want to have it applied to any video, regardless of the MIME type. In `setSource`, we also call `next` directly with `null` and the `srcObj` because we want to use this middleware with any and all sources. We also set up our `duration` method to take in the duration from the previous middleware and divide it by the playback rate we get from the player.

If you look at the [code][] you can see some other methods next to duration. They're there to make sure other methods that rely on timing get updated. The two methods to notice are `currentTime` and `setCurrentTime`. `currentTime` gets called when we want to know what the current time is. `setCurrentTime` is called when we're seeking. Because the user is seeking in the shifted time, we want to apply our change operation in reverse. Instead of dividing it, we want to multiply it.
```js
    currentTime(ct) {
      return ct / player.playbackRate();
    },

    setCurrentTime(ct) {
      return ct * player.playbackRate();
    },
```

If you were to apply what we've done so far, you'll notice that the nothing changes, the control bar is still showing a duration of 20 minutes. This is because as far as Video.js knows, nothing has changed. So, we need to tell Video.js that the duration has changed. We can do that by storing the tech that Video.js gives us after source selection is complete.
```js
videojs.use('*', function(player) {
  let tech;

  return {
    setTech(newTech) {
      tech = newTech;
    }

    /* ... */
  };
});
```

And then, when the `ratechange` event triggers, we tell Video.js that the duration has changed and Video.js will update the controls accordingly:
```js
videojs.use('*', function(player) {
  let tech;

  player.on('ratechange', function() {
    tech.trigger('durationchange');
    tech.trigger('timeupdate');
  });

  return {
   /* ... */
  }
});
```

You can see a [live example here][live] and the [complete code here][code].

[pra]: https://github.com/videojs/videojs-playbackrate-adjuster
[live]: https://videojs.github.io/videojs-playbackrate-adjuster/
[code]: https://github.com/videojs/videojs-playbackrate-adjuster/blob/master/src/js/index.js
