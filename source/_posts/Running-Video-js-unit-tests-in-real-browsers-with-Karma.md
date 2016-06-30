---
title: Running Video.js unit tests in real browsers with Karma
tags: []
author:
  name: Jim Whisenant
  github: BCjwhisenant
alias:
  - post/61644484835/running-videojs-unit-tests-in-real-browsers-with/index.html
  - post/61644484835/index.html
date: 2013-09-18 23:21:00
---

If you’ve ever cloned the video.js repository, either to contribute or to build your own version, you’ve no doubt run the video.js unit tests. Until just recently, though, we only had support for running unit tests with grunt, using the PhantomJS browser. Well, that’s changed, with the first phase of our integration with Karma. Now, you’ll be able to run your tests in real browsers.

Setting things up is a snap. After you pull down the latest from video.js and run `npm install`, simply copy the test/karma.conf.js.example file to test/karma.conf.js, add the browsers you wish to test to the browsers array, and run `grunt karma:dev`. That’s it. Of course, there are more options that you can configure, but if you want to get the ball rolling quickly, just add browsers, and run the tests. See the test/karma.conf.js.example file for more  instructions.

For our next phases of integration, we’re planning to include support for running tests on mobile devices, as well as running these  tests in a publicly-available location, so that anyone can tell at a glance how things are going.

You can learn more about Karma [here](https://npmjs.org/package/karma).

Cheers!

-Jim

![](http://feeds.feedburner.com/~r/video-js/~4/Iv1mc-5p_Og)
