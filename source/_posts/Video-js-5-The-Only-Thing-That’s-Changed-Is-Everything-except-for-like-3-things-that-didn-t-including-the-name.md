---
title: >-
  Video.js 5: The Only Thing That’s Changed Is Everything...except for like 3
  things that didn't (including the name).
tags: []
author:
  name: Steve Heffernan
  github: heff
date: 2015-09-29 14:00:16
---

First and foremost, **THANK YOU** to the 25 contributors who completed and merged **146 pull requests** and updated just about every line of code in the project. And thank you to the hundreds of issue commenters and plugin authors who helped shape this latest version. For a widget, we've got a pretty awesome community.

For 5.0 we have some interesting new features, and we made A LOT of new technology choices. This will include an exhaustive dive into those choices, because... why not?

*   Redesigned and rebuilt the UI
    *   Using a flex-box-based controls layout for easier add-ons
    *   Improved accessibility of the controls
    *   Switched from LESS to SASS
    *   Switched to Material Icons
*   Rebuilt the library in ES6/Babel/Browserify including Modules and Classes
*   Added support for responsive layouts including auto-sizing to the video content
*   Added support for HLS in desktop browsers **without Flash**
*   Improved audio-only support
*   Added 6 more language translations bringing the total to 25
*   Switched from Closure Compiler to UglifyJS (we STOPPED mangling object properties)
*   Switched to JSDoc for documentation
*   Switched to BrowserStack for automated browser testing
*   Switched to Fastly for our CDN
*   New definition around plugins
*   New [player skin designer](http://codepen.io/heff/pen/EarCt/left/?editors=010) on Codepen
*   New definition around playback technologies ("techs")
*   New project governance model
*   New Website and Logo!

If you're a Video.js user or lover of [semver](http://semver.org/), you're probably looking at the [changelog](https://github.com/videojs/video.js/blob/872459837b21c1e3d05fa86df4022f5a2b17ea1c/CHANGELOG.md#500-2015-09-29) and dying inside (or out). You're probably already thinking about how little you want to do the mega-upgrade dance every time the major version is bumped. To be clear, there will be at least _some_ upgrade cost between major versions because that's just how things go. **However**, from 5.0 on, we plan on being a lot more liberal with major versions to avoid stop-the-world mega releases like this.

The fact is, this release [cleaned up a lot of technical debt](https://github.com/videojs/video.js/wiki/5.0-Change-Details) and cruft over years of maintaining a popular open source library. Working with the codebase is fun again (not like it wasn't before, Judgy McJudgerson) and we think we've bought ourselves at least 6 months before we have to upgrade to ES9000\. In all seriousness, we plan on being quicker with releases, both breaking and non, in order to make incremental upgrades less painful and, well, quicker. Besides, Chrome and Firefox are going to be in the thousands soon for their releases, and we want some of that fun.

## Redesigned and rebuilt UI

[![](http://66.media.tumblr.com/7936bfddeddf68058784eb6d815cd631/tumblr_inline_nveuwhsdFu1qzc111_540.png)](http://www.videojs.com)

Video.js has had an all-CSS skin (including for Flash) since it was created in 2010 (the [first version](http://videojs.github.io/video.js/) was _literally_ all CSS, no images, fonts, or svgs. And still works today!) Over those years we've seen some very creative customizations and learned a lot about what users are hoping to do with player design when they're able to use native web techologies. For 5.0 we've both simplified the default layout and added more flexibility than ever before.

### Flex Box Layout

One of our biggest challenges with the layout of the controls is keeping the control bar flexible and able to accomodate any new buttons that a plugin author might want to add. In 4.0 we used CSS floats to allow a new button to be appended to the control bar and flow into space. This however led to a very awkward tab order for anyone navigating the controls with the tab key. In 5.0 we were finally able to take advantage of [flex box](https://css-tricks.com/snippets/css/a-guide-to-flexbox/), which improves on the flexibility of the previous version while also maintaining the right tab order. In IE8 (because yeah, we still support that) we fall back to `display:table`, which works surprisingly well.

### Improved accessibility

Accessibility has been a [hot topic as of late](http://www.3playmedia.com/2015/04/30/video-jss-approach-to-video-player-accessibility/), with [new regulations](http://www.w3.org/WAI/intro/wcag) helping push the industry forward and defining what accessbility means for a player. The tab-order mentioned previously was an eye sore in our accessiblity support, and we're all happy to have that fixed in 5.0\. Addtionally, after [a long debate](https://github.com/videojs/video.js/issues/841), we switched our Button elements to use the actual HTML button tag instead of divs. Using divs previously gave us a large degree of safety from external styles clobbering our own styles. This can be a big problem for html-based widgets that are dropped into other frameworks that add styles directly to native elements (_ahem_ Foundation). In the end however the accessibility experts in our community made a strong enough case, pointing out that there's still a number of devices that do not handle ARIA roles and javascript enhanced divs well enough to fully rely on them.

### Switched from LESS to SASS

The original decision to use [Less](http://lesscss.org/) over [Sass](http://sass-lang.com) in version 4 was largely driven by the fact that it could be run in the browser. We knew we wanted to start using a preprocessor, but we still wanted to provide things like the skin designer which meant we needed to be able to do all pre-processing on the client. Less was totally appropriate for the job and allowed us to start modernizing the skin in appearance and tooling.

Since then, thanks to [Emscripten](https://github.com/kripken/emscripten), Sass has joined Less in the browser. This meant that we were free to use whichever of the two we liked, so all the contributors put on Less or Sass branded boxing gloves and fought until two gloves were still standing, which turned out to be Sass. Aside from the battle royale, there were a few reasons we decided to go with Sass for the new base skin in version 5.

*   **Familiarity**. Core contributors that started working on the new base skin were a little more experienced with Sass and simply preferred it.
*   **Speed**. On top of allowing us to use Sass without requiring Ruby, [LibSass](https://github.com/sass/libsass) is _fast_.
*   **Popularity**. Your parents are right, not everything's a popularity contest... but it is when you want to pick between two basically equivalent* tools. We wanted to pick something that more devs would be familiar with, and that seems to be Sass. On that note, it was also requested fairly often on the issue tracker.
*   **Community**. This goes hand in hand with popularity, but the Sass community is large and growing, with currently popular projects using it (and [switching to it](http://blog.getbootstrap.com/2015/08/19/bootstrap-4-alpha/)) and new tooling popping up every day along side industry standards such as [Compass](http://compass-style.org/) and [Bourbon](http://bourbon.io).

Big changes that we made during the switch that are unrelated to the tooling:

*   We broke apart the source files.
*   As mentioned above, we switched to a Flexbox-based layout. "What about IE8," you say again? Tables. [Srsly](https://github.com/videojs/video.js/blob/a5dad5ade29a0ae33e0b80c600f1a06c953aff52/src/css/components/_control-bar.scss#L55-L58).
*   Improved support for responsive layouts.
*   We simplified the amount of customization we explicitly allow in the source to encourage people to build _on top_ of the base skin, not edit it directly.
*   On the simplification note, we tried to simplify the base skin as much as possible. Our goal was to allow designers to build _anything_ on top of the base without having to entirely start from scratch.
*   And more!

### Switched to Material Icons

The switch to an icon font in version 4 was a huge win for Video.js. It allowed designers to do things as basic as style component icon colors with _just CSS_. It simplified the code base by allowing us not not worry about things like image sprites or other image optimizations. The only recurring issue we ran into was the process around adding new icons to the set, which ultimately involved just uploading the font back to [IcoMoon](https://icomoon.io/) and re-exporting.

In version 5, we've switched everything about our icon set. First we went with a new set of icons: Google's [Material Icons](https://www.google.com/design/icons/). This was a big step forward in terms of appearance, but we had the same issue as far as adding new icons. To fix that process we created new [Font tooling](https://github.com/videojs/font) for the project.

The tooling is really simple, but it allows anyone to write out a JSON configuration file pointing to any SVGs they have access to. The output of that tool is the font files themselves along with a new SCSS partial that gets imported into our Sass workflow in the core project at build.

We primarily use the Material icons, but occasionally we have to pull in a social media icon from another set, which this process greatly simplifies. See icons missing in the set that you'd like to use? Give the tool a try and let us know what you think!

## Rebuilt the library in ES6/Babel/Browserify including Modules and Classes

5.0 comes with bit of enlightenment as we ditch our not-built-here syndrome and make a leap to post-modern development practices.

When we started work on the new version we originally planned to just use browserify and CommonJS modules, but when we took a closer look at the great work being done on [Babel](https://babeljs.io), it only made sense to jump again to ES6 modules and save ourselves another code transition down the road, while at the same time gaining a lot of great new JavaScript features.

Side-stepping any argument of the validity of JavaScript Classes, Video.js has always used classes for the internal UI framework. We used a custom class implementation that was of course incompatible with other custom implementations, so the move to ES6 opens the door for some potential interoperability with other frameworks as more people use ES6 classes specifically.

With the move to modules we've also opened the door to using more of the glorious ecosystem that is [npm](https://www.npmjs.com). We've started to toss out some of the non-core internal libraries that required us to be experts in just about everything, and replace them with other community-built modules. So far this includes libraries like [lodash](https://lodash.com) and [raynos/xhr](https://github.com/raynos/xhr), and we're actively looking for opportunies where we can share more code.

### What does this mean for the end user?

This should make it easier for end-users of videojs, especially if they use browserify themselves. With this change, users can just
`require('video.js')` in their `app.js` or whenever they need it to instantiate players. In addition, plugins that were written
using browserify themselves can be added very easily using browserify and `require` as well. Of course, you can use Babel and
ES6 modules as well. For example, your `app.js` could look like the following, assuming browserify and babel for ES6 syntax:

```js
// import videojs
import videojs from 'video.js';
// import several plugins.
// Each requires videojs itself and registers itself accordingly.
import 'videojs-playlist';
import 'videojs-thumbnail';
// this isn't ready yet, unfortunately
import 'videojs-contrib-hls';

// make a player
let player = videojs('my-video');
// initialize some plugins
player.playlist(myplaylist);
player.thumbnail(myThumbnailConfig);
```

## Added support for responsive layouts including auto-sizing to the video content

[Check out this video from the SF Vid Tech meetup for quick overview.](https://www.youtube.com/watch?v=0AEFvF2bHtw&amp;feature=youtu.be&amp;t=50m37s)

_Proper_ support for responsive layouts has been a long time request. We've had tips and guides for how to make it work in the old version, but now it's seamlessly built into the player. First we have two easy CSS classes you can just add to the embed code, `vjs-16-9` and `vjs-4-3`.

`<video class="video-js vjs-16-9 vjs-default-skin" ...></video>;`

Or you can set the player to a fluid layout in the options and it will **automatically update to match the aspect ratio of the content.**, whether you set a width, height, or neither.

```js
var myPlayer = videojs(id, { fluid: true, preload: 'metadata' });
```

This was very tricky to implement, but well worth the effort. We have a [jsbin example page](http://jsbin.com/qucinu/edit?html,output) where you can try out different options.

## Added support for HTTP Live Streaming in desktop browsers WITHOUT Flash

We've been chasing down the last excuses for using Flash for video for awhile now and have reached some major milestones. [videojs-contrib-hls](http://videojs.github.io/videojs-contrib-hls/) 1.0 (it's about time!) will use [Media Source Extensions](https://w3c.github.io/media-source/), an advanced video element API, to provide HLS playback in Chrome and Microsoft Edge. Besides the enhanced debuggability of doing everything in Javascript, most of the computation has been moved to a web worker and is hardware accelerated on platforms that support it. That means even 60fps or 4k streams should play back without a stutter and your battery will get less of a workout, too. As part of that work, we've split off the code we use to repackage HLS video into a separate project in the video.js org: [mux.js](https://github.com/videojs/mux.js). If you're interested in how video works down at the bit level, check it out! We're always looking for new contributors.

## Switched from Closure Compiler to Uglify (We STOPPED mangling object properties)

In version 4.0 we introduced Google's Closure Compiler into our build chain. It's **advanced optimizations mode** saved us 25% in filesize over UglifyJS at the time. The downsides were that it required us to write the code in very specific ways and mangled internal object properties, which made contributing, debugging, and writing plugins much more difficult. The reality today is that with gzip and improved bandwidth, video.js users are pushing us less and less to squeeze the last bit of filesize out of the library. So for 5.0 we've switched back to UglifyJS. Plugin authors rejoice!

## Improved audio-only support

Hey did you know? Video.js also supports the HTML5 audio tag. Try it out and let us know what you think.

`<audio id="my_audio_1" class="video-js vjs-default-skin" controls data-setup="{}"><source src="MY_AUDIO_FILE.mp3" type="audio/mp3"></source></audio>`

## Added 6 more language translations

Ever since we implemented localization in Video.js there's been a wave of contributions from all over the world. With 5.0 comes Danish, Bosnian, Serbian, Croatian, Finnish, and Turkish. This brings the number of lanugages supported to 25!

## New definition around plugins

Plugins mostly have not changed from how they worked in Videojs 4.x. You still register and use them the same way.
The only major change is that if you are instantiating a plugin inside the config of a player, this plugin will get run
before the player is ready. This is so that if plugins are doing some UI work or adding themselves as children of the player
they can do so early on. This means that plugins that require the player to be ready would need to handle that themselves or
else not support instantiation via the player config. Ex:

```js
videojs('my-video', {
  plugins: {
    playlists: {}
  }
});
```

As part of the update to videojs 5 and our switch from Google's Closure Compiler to Uglify,
we've been focusing on making the plugin experience better.
We've made sure to export some of the utility functions that we use inside our own codebase to make writing plugins easier.
This way plugins don't need to include extra code themselves if a function, merge, for example, is available from videojs.

Also, we're encouraging plugin authors to publish their plugins on npm. If the plugins are tagged with `videojs-plugin`,
they'll show up on our [spiffy new plugins listing page](http://videojs.com/plugins/).

## New player skin designer on Codepen

We're taking a slightly different approach with [the designer](http://codepen.io/heff/pen/EarCt/left/?editors=010) now. Instead of exposing all CSS properties from the default skin, we've set up a template starter that has fewer options and allows you to get at the more common customizations more easily.

It's also using [Codepen](http://codepen.io), which is a great service and much better than hosting the designer ourselves. If you use it to make a cool skin, be sure to let us know. Tweet at @videojs or comment on the designer.

[http://codepen.io/heff/pen/EarCt/left/?editors=010](http://codepen.io/heff/pen/EarCt/left/?editors=010)

## New project governance model

Jumping right on that band wagon, we're implementing a governance model for the project that's similar to [Node.js/io.js](https://github.com/nodejs/node/blob/20dae2a90610d7b6243dffc3c88a0cd2dfbcb7ce/GOVERNANCE.md) and [0MQ's C4.1](http://rfc.zeromq.org/spec:22). Along with some better structure around the project it comes with a lower bar for becoming an official maintainer of the project. We're aiming to have a more diverse set of people and companies representing the leadership of Video.js.

## Improved the definition of a playback technology or "tech"

The creator of our most popular plugin ([the YouTube plugin](https://github.com/eXon/videojs-youtube)) took on the task of improving the relationship between the player and techs. "Tech" is the term we use to describe the translation layer between the player API and the underlying video objects, which can be the HTML5 video element, plugins like [Flash](https://github.com/videojs/video-js-swf) and [Silverlight](https://github.com/Afterster/videojs-silverlight), or other players like [Youtube](https://github.com/eXon/videojs-youtube) and [Vimeo](https://github.com/eXon/videojs-vimeo). If you've built a tech or are intersted in building one, check out the changes in 5.0.

## Switched to JSDoc from a home-grown docs parser

In the last version we had a [custom built docs generator](https://github.com/videojs/doc-generator) that used [esprima](http://esprima.org) to parse the AST and create a lot of the API docs just from the code, with JSDoc tags filling in the rest. That could still be a good project, but it's become too much to maintain and too limited for plugin authors.

For 5.0 we've switched to [jsdoc](https://github.com/jsdoc3/jsdoc). Check out the [new docs site](http://docs.videojs.com). Anyone want to help us design it? :)

## Switched to BrowserStack for automated browser testing

For 5.0 we have moved from [Sauce Labs](https://saucelabs.com) to [BrowserStack](https://www.browserstack.com) as our browser provider for automated testing. BrowserStack is a cross-browser testing tool that provides desktop and mobile browsers and are currently rolling out support for real mobile devices. We moved to BrowserStack to reduce the amount of timeout errors, build time and false positives we were seeing in our builds previously. This stability has allowed us to expand our testing coverage to include iOS, Android and all supported versions of Internet Explorer.

## Switched to Fastly for our CDN

The CDN-hosted version of Video.js at vjs.zencdn.net is downloaded over 2 BILLION times every month, even with agressive browser-side caching. Video.js is relatively small in file size but that's still 34 TB per month. We were previously piggy-backing on Brightcove's Akamai account, but Fastly offered to host the library for free. We gave it a shot and fell in love. In full disclosure, our testing found that Fastly didn't perform quite as well as Akamai internationally, but Fastly's user interface, API, and real-time purge are things of beauty and make our [CDN deploy process](http://github.com/videojs/cdn) much simpler.

## New Website and Logo!

![Video.js Logo](http://67.media.tumblr.com/cb1841277ee68c248443e42c0391e286/tumblr_inline_nvf06qcRca1qzc111_540.png)

We simplified it a bit. What do you think?

If you don't feel comfortable contributing to Video.js we can always use help on [the website](http://videojs.com). It gets **over 200,000 unique views** every month. Contribute at [the website repo](https://github.com/videojs/videojs.com).

# HALP!

It's our community that keeps Video.js actually decent and **truly free** (Apache 2), so don't be shy. Jump in and submit a bug, build a plugin, or design a skin.

In conclusion, it's been a good year of a lot of good work. Thanks!

<span class="no-fancybox">
[<img width=18 src="http://66.media.tumblr.com/1e6ec3eacf62a57d0543b01cb4454527/tumblr_inline_nvesinCDHy1qzc111_500.gif">](https://news.ycombinator.com/item?id=10298267)[<img width=18 src="http://66.media.tumblr.com/2872e2376c584d2abe4293d442e367e2/tumblr_inline_nvesehMSl91qzc111_540.png">](https://twitter.com/videojs/status/648921250882977792)
</span>
![](http://feeds.feedburner.com/~r/video-js/~4/ASsV6seqM8A)
