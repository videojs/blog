---
title: 'Feature Spotlight: Accessibility'
author:
  name: Brandon Casey
  github: brandonocasey
tags:
  - videojs-6
  - accessibility
---

Accessibility! The most important feature you never knew about.

In the Video.js organization we try hard to have good accessiblity. Like most other software any change can effect the system in unintended ways. For example `MuteToggle` and `VolumeControl` were recently married into `VolumeMenuButton`. While this change did allow these controls to work in tandem. It also did something unintended. It broke accessibility. In this post we will go over what broke, what the fix was, what accessibility is, and how to test and make sure it works.

> Feel free to skip to the [last section](#The-problem-and-the-solution) if you already know what accessibility is.

### Accessibility? What's that?
Accessible software has support for users with vision, hearing, or other impairments. Out of the box web applications have some accessibility due to the nature of HTML, but this is only the case if you are using native elements in intended ways. If you cannot use native DOM elements like button, and instead must use a div for buttons. Then you need worry about accessibility in your page.

Supporting hearing impairment is not something that we can do for the users of Video.js. Instead we must indirectly support these users by supporting captions in their videos. In Video.js we have had support for captions/subtitles for some time, internally they are called `TextTracks`.

Supporting vision impairment is harder, but fully in our control. To support this group of users our player must be accessible to "screen readers". A "Screen reader" is an application that reads elements off of the screen to the user (as the name implies). HTML has certain rules that must be followed so that a page can be accessible. We will go over the basics of these rules in the next section.

Here are some popular screen readers that are actually used in the wild:

* [VoiceOver for macOS](https://en.wikipedia.org/wiki/VoiceOver)
* [JAWS for Windows](https://en.wikipedia.org/wiki/JAWS_(screen_reader))

### How do you make a web application screen reader accessible?
The most basic way to make a website accessible is to add the `role=""` attribute to an HTML element.

> A list of roles can be found on [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques#Composite_roles).

Some Examples (assume all of these are plain `div`s):
* A `MuteToggle` button would have a `role` attribute with a value of `button` like this: `role="button"`
A `VolumeBar` slider would have a `role` attribute with a value of `slider` like this: `role="slider"`.

If you use the native `<button>` element, you will not have to use the button role. This is the recommended way to make an element a button to the screen reader. In some cases (such as in Video.js) it will not be possible to use the native button element. So you will have to role instead.

Another important point is that all ARIA controls must be usable with the keyboard alone. This is somewhat outside of the scope of this blog post but, usually you will have to add `tabindex`s to elements.

After adding roles to the controls and content in your webpage. The next thing to look over are `aria` attributes. For instance we use `aria-live` for our `ProgressBar` slider. This is because the `ProgressBar` is always updating when a video is playing.

> A list of attributes can be found in the [ARIA specification](https://www.w3.org/TR/wai-aria-1.1/)

Finally you need to add an accessible name an element so that it can be referred to correctly. For instance a "Mute Button" with only a `role` of `button` would be read to the user as "button". If you add an accessible name to it, it will then be read as "Mute Button". In Video.js we refer to the accessible name as "Control Text". When we change the "Control Text" for a `Component`. A span with the specified text is added as a child element to the `Component` in question. This will let the screen reader know how to refer to the control.

### The problem and the solution

Now let's talk about how screen reader accessibility broke in Video.js. First `VolumeMenuButton` replaced `MuteToggle` and `VolumeControl` on the `ControlBar`. `VolumeMenuButton` was set to mimic `MuteToggle` when clicked. It was would also show the `VolumeControl` on mouseover or focus. This was a problem because a `VolumeControl` was a now a child of a button, and buttons should not contain other controls. So to the screen reader and to the DOM there are two `MuteToggle button` controls. When visually there is a `VolumeControl` and a `MuteToggle`. Below you can see a gif of this behavior in action:

![Before The Fix](before-the-fix.gif)

The solution to this problem was to use a regular `div` to house the `MuteToggle` and `VolumeControl`. This regular `div` would have no role or "Control Text" so that it would be invisible to a screen reader. From that point forward we just needed to mimic the old UI. For those who are wondering this new `Component` is called the `VolumePanel`. See the new screen reader behavior in a gif below:

![After The Fix](after-the-fix.gif)

### Wrap up

Hopefully this post has given you some insight into making a web application accessible. If you find any issue or have any suggestions for our accessibility or in general feel free to [contribute to Video.js](https://github.com/videojs/video.js/blob/master/CONTRIBUTING.md).
