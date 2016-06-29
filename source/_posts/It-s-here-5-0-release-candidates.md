---
title: 'It''s here: 5.0 release candidates!'
tags:
  - release
  - videojs
  - version
author:
  name: Matthew McClure
  github: mmcc
date: 2015-06-08 19:59:42
---

Today we&rsquo;re releasing our first official 5.0 release candidate. We&rsquo;re really excited to get this into the wild, but a lot has changed under hood. It should be stable for normal usage, but we&rsquo;ve got a limited number of test devices (and testers for that matter), so what we need now is for people to bang on 5.0 and let us know what breaks!

You can either grab the master branch from Github and run `grunt` to create your own build, or we&rsquo;ll push each new release candidate to the CDN. We&rsquo;re going to be moving quickly and pushing new candidates when we uncover / fix bugs, so be sure to check for new versions! As of this post, the newest release is [5.0-rc.2](http://vjs.zencdn.net/5.0.0-rc.2/video.js). If you&rsquo;d like to see a demo, we&rsquo;ve got one up on [JSBin](http://jsbin.com/halokodoxo/1/edit?html,output).

## What&rsquo;s different?

### New Base Theme

The most obvious difference you&rsquo;ll see when you load up the release candidate is most likely going to be the new base theme. We&rsquo;ve worked hard to simplify the theme and make it possible to do as much as possible with purely CSS.

![screenshot](https://cloudup.com/c0dLiQlBE8F+)

There are elements in the control bar that aren&rsquo;t shown by default, but can be by simply overriding a little CSS. For example, if you&rsquo;d rather use an inline volume control instead of the popup menu, you could make these changes:

`css
.video-js .vjs-volume-menu-button { display: none; }
.video-js .vjs-mute-control, .video-js .vjs-volume-control { display: flex; }`

This just hides the `.vjs-volume-menu-button` control, and unhides the mute control / inline volume control. Other things that are available but hidden by default are a time divider (`.vjs-time-devider`), total duration (`.vjs-duration`), and a spacer (`.vjs-custom-control-spacer`).

### ES6

Thanks to the incredible [Babel project](http://babeljs.io), we&rsquo;re able to write ES2015 and have it transpiled to ES5\. It&rsquo;s fantastic, and means we get glorious things such as string interpolation, so reading our codebase ``just got a whole lot ${_.sample(['better', 'awesomer', 'more fantastic', 'less plus-y'])}``.

On our Wiki we have a page devoted to [ES6 features we use extensively](https://github.com/videojs/video.js/wiki/ES6-Features-used-in-the-source). There are probably a few little things missing, but if you familiarize yourself with those features you should feel comfortable reading the Video.js source.

We&rsquo;re also using [Browserify](http://browserify.org) (with a Babelify transform), so now we&rsquo;re able to take advantage of all the browser packages NPM has to offer. One big area this has allowed us to improve is utility functions, which are now provided by [Lodash](http://lodash.com).

### Plugins

Plugins are initialized earlier than before, giving plugin developers even more control over the player experience. They&rsquo;re now initialized before other components are added (like the control bar), but after the player div is created. Keep in mind you&rsquo;ll now need to wait for the `ready` event to do things such as append items to the control bar.

The plugin-development world should have also gotten more predictable since we&rsquo;ve gotten a little less aggressive with our minification. Keep in mind that when you want to subclass a component externally (not using ES2015), you&rsquo;ll want to use [`videojs.extends`](https://github.com/videojs/video.js/wiki/5.0-Change-Details#switched-to-es6-classes-updated-how-you-subclass-components).

You can find more information on our [5.0 change details](https://github.com/videojs/video.js/wiki/5.0-Change-Details) wiki page.

# Want to be helpful?

Use it! We&rsquo;ve spent months using this player in very specific ways while development, but there&rsquo;s always the issue of &ldquo;not being able to [see the forest for the trees](http://en.wiktionary.org/wiki/see_the_forest_for_the_trees)&rdquo;. If you find issues while testing the player, please let us know on the [issue tracker](https://github.com/videojs/video.js/issues).
![](http://feeds.feedburner.com/~r/video-js/~4/lLH3vkDw9AY)
