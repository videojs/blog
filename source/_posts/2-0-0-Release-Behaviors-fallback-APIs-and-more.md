---
title: "2.0.0 Release - Behaviors, fallback APIs, and more."
tags:
  - version
author:
  name: Steve Heffernan
  github: heff
date: 2010-11-22 00:00:00
---

Big update. The biggest change for current users is a move back to using DIVs for control bar elements instead of unordered lists. There were a lot of conflicting styles issues when using lists, which shouldn&rsquo;t be an issue with divs. So if you upgrade, don&rsquo;t forget to upgrade your stylesheets as well.

Beyond that, a lot of code was reorganized and modularized to create a platform for further expansion, like custom plugins and controls. The concept of &ldquo;behaviors&rdquo; was added, so you can activate any element on the page to act like a video controls. For instance, the following code snippet will make the specified element act like a play button, and play the video when clicked.

<pre>myplayer.activateElement(myElement, "playButton");
</pre>

The next code snippet will make the element act like a play progress bar, meaning it will grow horizontally as the video plays.

<pre>myplayer.activateElement(myElement, "playProgressBar");
</pre>

More documentation on this is coming.

The code is now prepped for APIs to the fallback flash players. So if you call myPlayer.play(), it will trigger a play in both the HTML5 and the Flash version, whichever is currently being used. A flowplayer API is just about done, and other popular Flash players will follow.

Finally, you can change the fallback order by modifying the playerFallbackOrder option, which is an array of player platforms. So if you want Flash to be dominant, you would pass the following option.

<pre>VideoJS.setupAllWhenReady({
  playerFallbackOrder: ["flash", "html5", "links"]
});
</pre>

This also leaves room for other platforms to be added, like Quicktime.

So coming up is Flash player APIs, and another cool feature I don&rsquo;t want to mention just yet.

Full list:

*   Feature: Created &ldquo;behaviors&rdquo; concept for adding behaviors to elements - Feature: Switched back to divs for controls, for more portable styles
*   Feature: Created playerFallbackOrder array option. [&ldquo;html5&rdquo;, &ldquo;flash&rdquo;, &ldquo;links&rdquo;]
*   Feature: Created playerType concept, for initializing different platforms
*   Feature: Added play button for Android
*   Feature: Added spinner for iPad (non-fullscreen)
*   Feature: Split into multiple files for easier development
*   Feature: Combined VideoJS &amp; _V_ into the same variable to reduce confusion
*   Fix: Checking for m3u8 files (Apple HTTP Streaming)
*   Fix: Catching error on localStorage full that safari seems to randomly throw
*   Fix: Scrubbing to end doesn&rsquo;t trigger onEnded

[Download version 2.0.0](http://videojs.com/#download)
