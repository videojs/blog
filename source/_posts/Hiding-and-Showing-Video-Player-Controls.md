---
title: Hiding and Showing Video Player Controls
tags: []
author:
  name: Steve Heffernan
  github: heff
alias:
  - post/57828375480/hiding-and-showing-video-player-controls/index.html
  - post/57828375480/index.html
date: 2013-08-09 18:51:47
---

Last week I decided to tackle a number of outstanding issues around the control bar, and then proceeded to fall down a rabbit hole of related player updates. I&rsquo;ve thankfully resurfaced now, and figured I&rsquo;d write about a few of the updates that came from it.

One of the expected behaviors of the player&rsquo;s control bar is that it will fade out after a couple of seconds when the user is inactive while watching a video. Previously, the way we achieved this with video.js was through a bit of a CSS trick. When the user&rsquo;s mouse would move out of the video player area, the control bar would be given the classname `vjs-fade-out`. This class had a visibility transition with an added 2 second delay.

<pre class="prettyprint">
.vjs-fade-out {
  display: block;
  visibility: hidden;
  opacity: 0;

  -webkit-transition: visibility 1.5s, opacity 1.5s;
     -moz-transition: visibility 1.5s, opacity 1.5s;
      -ms-transition: visibility 1.5s, opacity 1.5s;
       -o-transition: visibility 1.5s, opacity 1.5s;
          transition: visibility 1.5s, opacity 1.5s;

  /* Wait a moment before fading out the control bar */
  -webkit-transition-delay: 2s;
     -moz-transition-delay: 2s;
      -ms-transition-delay: 2s;
       -o-transition-delay: 2s;
          transition-delay: 2s;
}
</pre>

When the user&rsquo;s mouse moved back over the player, the class would be removed, canceling any delayed fade-out. This provided a similar experience to how you might expect the controls fading to work, and only took a few lines of javascript to add/remove the class.

<pre class="prettyprint">
player.on('mouseout', function(){ 
  controlBar.addClass('vjs-fade-out'); 
});

player.on('mouseover', function(){ 
  controlBar.removeClass('vjs-fade-out'); 
});
</pre>

There&rsquo;s a few drawbacks though that have made it necessary to move away from this approach.

1.  Controls don&rsquo;t fade out in fullscreen mode because the mouse can never move out of the player area.
2.  There is no mouse on mobile devices so different events and interactions are needed to show/hide the controls.

In addition to these issues, we want it to be possible for any player component or plugin to hook into the same trigger that hides the controls. Components like social sharing icons should fade out in the same way that the controls do.

## User State

One of the first things that is being added is a `userActive` property on the player, that can be either `true` or `false`. What this does is abstract the controls hiding out to what it is we&rsquo;re actually concerned with, that is, whether the user is currently interacting with the player or just passively watching the video. This also decouples the control bar from tracking the user activity itself, and allows other components to more easily behave the same way as the control bar, through a player-level state.

That actual property is `player.userActive()` and returns either `true` or `false`. When this value is changed, it triggers an event on the player.

<pre class="prettyprint">
player.userActive(true)
    // -&gt; 'useractive' event triggered
player.userActive(false)
    // -&gt; 'userinactive' event triggered
</pre>

A CSS classname of either `vjs-user-active` or `vjs-user-inactive` is also added to the player element. The classname is what&rsquo;s actually used now to hide and show the control bar.

<pre class="prettyprint">
.vjs-default-skin.vjs-user-inactive .vjs-control-bar {
  display: block;
  visibility: hidden;
  opacity: 0;

  -webkit-transition: visibility 1.5s, opacity 1.5s;
     -moz-transition: visibility 1.5s, opacity 1.5s;
      -ms-transition: visibility 1.5s, opacity 1.5s;
       -o-transition: visibility 1.5s, opacity 1.5s;
          transition: visibility 1.5s, opacity 1.5s;
}
</pre>

The 2 second delay has been removed from the CSS, and instead will be built into the process of setting the userActive state to false through a javascript timeout. Anytime a mouse event occurs on the player, this timeout will reset. e.g.

<pre class="prettyprint">
var resetDelay, inactivityTimeout;

resetDelay = function(){
    clearTimeout(inactivityTimeout);
    inactivityTimeout = setTimeout(function(){
        player.userActive(false);
    }, 2000);
};

player.on('mousemove', function(){
    resetDelay();
})
</pre>

The mousemove event is called very rapidly while the mouse is moving, and we want to bog down the player process as little as possible during this action, so we&rsquo;re using a technique [written about by John Resig](http://ejohn.org/blog/learning-from-twitter/).

Instead of resetting the timeout for every `mousemove`, the `mousemove` event will instead set a variable that can be picked up by a javascript interval that&rsquo;s running at a controlled pace.

<pre class="prettyprint">
var userActivity, activityCheck;

player.on('mousemove', function(){
    userActivity = true;
});

activityCheck = setInterval(function() {

  // Check to see if the mouse has been moved
  if (userActivity) {

    // Reset the activity tracker
    userActivity = false;

    // If the user state was inactive, set the state to active
    if (player.userActive() === false) {
      player.userActive(true);
    }

    // Clear any existing inactivity timeout to start the timer over
    clearTimeout(inactivityTimeout);

    // In X seconds, if no more activity has occurred 
    // the user will be considered inactive
    inactivityTimeout = setTimeout(function() {
      // Protect against the case where the inactivity timeout can trigger
      // before the next user activity is picked up  by the 
      // activityCheck loop.
      if (!userActivity) {
        this.userActive(false);
      }
    }, 2000);
  }
}, 250);
</pre>

That may be a lot to follow, and it&rsquo;s a bit simplified from what&rsquo;s actually in the player now, but essentially it allows us to take some of the processing weight off of the browser while the mouse is moving.

## Hiding controls in fullscreen

Thanks to the new userActive state and the javascript timeout for the delay, the controls no longer require the mouse to move outside of the player area in order to hide, and can now hide in fullscreen mode the same way they do when the player is in the page. This also means we can now hide the mouse cursor in the same way we do the controls, so that it doesn&rsquo;t sit over the player while watching in fullscreen.

<pre class="prettyprint">
.vjs-fullscreen.vjs-user-inactive {
  cursor: none;
}
</pre>

## Hiding controls on touch devices

The expected behavior on touch devices is a little different than in desktop browsers. There is no `mousemove` event to help determine if the user is active or inactive, so typically a longer delay is added before the controls are faded out. Also, while a click on the video itself in desktop browsers will typically toggle between play and pause, a tap on the video on mobile devices will toggle the controls visibility.

Luckily the framework we&rsquo;ve set up around userActive has made this last part easy enough to set up.

<pre class="prettyprint">
video.on('tap', function(){
  if (player.userActive() === true) {
    player.userActive(false);
  } else {
    player.userActive(true);
  }
});
</pre>

Manually toggling userActive between true and false will apply the appropriate classnames and trigger the events needed to show and hide the controls as you&rsquo;d expect on a mobile device.

The `tap` event is actually a custom made event, similar to the tap event you&rsquo;ll find in [jQuery mobile](http://jquerymobile.com), [Hammer.js](http://eightmedia.github.io/hammer.js/), and other mobile touch libraries. A tap event occurs whenever a `touchstart` event is fired with the associated `touchend` event firing within 250 milliseconds. If the `touchend` event takes longer to fire, or if a `touchmove` event happens between the two, it is not considered a `tap`.

## Conclusion

I hope this has given some insight into how that piece of the controls operate in Video.js, and how you can mimic the same interaction if you&rsquo;re building your own plugins for Video.js. Feedback is always appreciated.

Cheers,

-heff
![](http://feeds.feedburner.com/~r/video-js/~4/70tJSFYIq9Y)
