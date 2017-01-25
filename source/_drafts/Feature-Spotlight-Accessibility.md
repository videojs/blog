---
title: 'Feature Spotlight: Accessibility'
author:
  name: Brandon Casey
  github: brandonocasey
tags:
  - video.js 6.0
  - accessibility
  - a11y
---

Accessibility! The most important feature you never knew about.

In the Video.js organization we try hard to have good accessiblity. Like most other software any change can effect the system in unintended ways. For example `MuteToggle` and `VolumeControl` were recently (in video.js 5) married into `VolumeMenuButton`. While this change did allow these controls to work in tandem. It also did something unintended. It broke accessibility. In this post we will go over what broke, what the fix was, what accessibility is, and how to test and make sure it works.

> Feel free to skip to the [last section](#The-problem-and-the-solution) if you already know what accessibility is.

### Accessibility? What's that?
Accessible software has support for users with vision, hearing, or other impairments. It also helps users that just want to use the keyboard to navigate. Out of the box web applications have some accessibility due to the nature of HTML, but this is only the case if you are using native elements in intended ways. If you cannot use native DOM elements like button, and instead must use a div for buttons. Then you need worry about accessibility in your page.

Supporting hearing impairment is not something that we can do for the users of Video.js. Instead we must indirectly support these users by supporting captions in their videos. In Video.js we have had support for captions and subtitles for some time, internally they are called `TextTracks`.

Supporting vision impairment is harder, but fully in our control. To support this group of users our player must be accessible to "screen readers". A "Screen reader" is an application that reads elements off of the screen to the user (as the name implies). HTML has certain rules that must be followed so that a page can be accessible. We will go over the basics of these rules in the next section. Screen readers are further supported by having description tracks that can be read out by screen readers during video playback (that is another type of `TextTrack`).

See the [resources section at the end of this post](#resources) for a list of screen readers.


### How do you make a web application screen reader accessible?
If you use the native elements for the purposes that they were intended you will be most of the way towards beings accessible. Using the native element is the recommendede way to make anything accessible for a screen reader. For instance if you use a `<button>` element you will get the following accessiblity attributes (without them actually being on the button):
* `tabIndex` which allows users to tab to the button
* `role="button"` which tells the screen reader that this is a button
* The space and the enter key will both press the button

In some cases, such as in Video.js, it will not be possible to use the native button element. So you will have to mimic the accessible functionality from the list above and use a `div`. For a `div` that wants to be classified as a button:
* You have to add the `role="button"` attribute to classify it as a button.
* You have to add a `tabIndex` which will allow the `div` to be navigated to using only the `tab` key
* You have to add handling for the space and enter key that press the button

{% pullquote %}
A list of role attribute values can be found on [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques#Composite_roles).
{% endpullquote %}

After adding roles to the controls and content in your webpage. The next thing to look over are `aria` attributes. For instance we use `aria-live` for our `ProgressBar` slider. This is because the `ProgressBar` is always updating when a video is playing.

{% pullquote %}
For a more complete list of [ARIA attributes see the specification](https://www.w3.org/TR/wai-aria-1.1/).
{% endpullquote %}

Finally you need to add an accessible "name" and "status" to an element so that it can be referred to correctly. For instance to indicate that the `MuteToggle` is not just a "button" we have to set the `title` and the `innerHTML`/`innerText` to "Mute Toggle". In Video.js we refer to accessible "name"/"status" updates as "ControlText". This will allow the screen reader to refer to the `MuteToggle` as "Mute Toggle" rather than button. We also have to update the button whenever it is "pressed" to indicate that it will now "Mute" or "Unmute" depending on its state.

Here are some examples of accessiblity straight from Video.js:
* The `MuteToggle` `<button>`:
  * Has `aria-live` set to polite, which means that the screen reader should wait until the user is idle to send valuenow updates to the user
  * Has "ControlText" of "Mute" or "Unmute" which indicates the current status of the button to the user. "ControlText" is a Video.js-ism that indicates a change in the `innerHTML` and `title` attribute that is localized along the way.
* The `VolumeBar` slider `<div>`:
  * Has a `role` attribute with a value of `slider` like this: `role="slider"`
  * Has a `tabIndex` attribute
  * Has `EventHandlers` that listen for:
    * The up and right arrow keys to increase the volume and the slider percentage
    * The down and left arrow keys to decrease the volume and the slider percentage
  * Has `aria-label` of "volume level" which is an accessible name that the screen reader schould refer to this with
  * Has `aria-valuenow` and `aria-valuetext` properties that update to indicate the current volume level (so the screen reader can read it)
  * Has `aria-live` set to polite, which means that the screen reader should wait until the user is idle to send valuenow updates to the user

### The problem and the solution

Now let's talk about how screen reader accessibility broke in Video.js. First `VolumeMenuButton` replaced `MuteToggle` and `VolumeControl` on the `ControlBar`. `VolumeMenuButton` was set to mimic `MuteToggle` when clicked. It was would also show the `VolumeControl` on mouseover or focus. This was a problem because a `VolumeControl` was a now a child of a button, and buttons should not contain other controls. So to the screen reader and to the DOM there are two `MuteToggle button` controls. When visually there is a `VolumeControl` and a `MuteToggle`. Below you can see a gif of this behavior in action:

![Before The Fix](before-the-fix.gif)

The solution to this problem was to use a regular `div` to house the `MuteToggle` and `VolumeControl`. This regular `div` would have no role or "Control Text" so that it would be invisible to a screen reader. From that point forward we just needed to mimic the old UI. For those who are wondering this new `Component` is called the `VolumePanel`. See the new screen reader behavior in a gif below:

![After The Fix](after-the-fix.gif)

### Outlines

On top of fixing a accessibility issue with our controls, we decided to remove the following css from video.js:

```css
{
  outline: none;
}
```

Why did we do it? With feedback from the community and [external resources](http://www.outlinenone.com/) we learned that outlines should always be on. Without outlines there is no focus on navigation elements and without that you page is much less accessible for the visually impaired and keyboard users.

### Wrap up

Hopefully this post has given you some insight into making a web application accessible. If you find any issue or have any suggestions for our accessibility or in general feel free to [contribute to Video.js](https://github.com/videojs/video.js/blob/master/CONTRIBUTING.md).

### Resources

Here are some popular screen readers that are actually used in the wild:

* [VoiceOver for macOS](http://www.apple.com/accessibility/mac/vision/)
* [JAWS for Windows](https://www.freedomscientific.com/Downloads/JAWS)
* [NVDA for Windows](http://www.nvaccess.org/)

Resources for learning more about web accessibility:

* [WebAIM](http://webaim.org/)
* [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/Accessibility)
* [Outline None](http://www.outlinenone.com/)
