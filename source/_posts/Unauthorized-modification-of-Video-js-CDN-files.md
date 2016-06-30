---
title: Unauthorized modification of Video.js CDN files
tags: []
author:
  name: Steve Heffernan
  github: heff
alias:
  - post/61309840958/unauthorized-modification-of-videojs-cdn-files/index.html
  - post/61309840958/index.html
date: 2013-09-15 11:23:00
---

**UPDATE 2013-09-19:**

The CDN continues to be secure and we have taken significant steps to ensure it never falls under a similar attack again.

*   Access to the CDN has been restricted to a few key individuals
*   A third-party service is now monitoring changes made to the CDN
*   Processes have been defined for responding to any such future issues

The original source of this event was the [Sendori Auto-update Hack](https://isc.sans.edu/forums/diary/Suspect+Sendori+software/16466), which possibly affected millions of people including, unfortunately, an admin of the CDN.

* * *

On the morning of September 14, 2013 at 6:25am PDST, we discovered that certain versions of video.js being served from our content delivery network (CDN) had been modified by an unknown attacker. The file was changed to contain malicious code that would attempt to install malware on any Windows or Macintosh computer that loaded the video.js file.  The malware has been identified to be a variant of [Trojan.PWS.Stealer.1932 or Trojan.Ransom.ED](https://www.virustotal.com/en/file/ea3be0fb4367e038c602a3de5811821d2367f3326ab2a12f469db4cda06fafa7/analysis/).  We quickly reverted to safe versions of the video.js file, and took steps to ensure that the issue could not reoccur.

The specific files affected were:

vjs.zencdn.net/c/video.js

vjs.zencdn.net/4.0/video.js

vjs.zencdn.net/4.1/video.js

No patch-level versions (e.g. vjs.zencdn.net/4.1.0/video.js) were affected, and neither was the latest version (4.2). Users who host their own copy of Video.js were also not affected.

Potential Impact: Any browsers that loaded the affected files during the compromised period may have prompted users to install malicious software on their computers.

It has been determined that the files were originally modified at 4:30am PDST. The files were repaired at 7:15am PDST and completed propagation to CDN edge caches around the world at 7:51am PDST.

Rest assured that video.js is once again safe to load.  We are currently investigating the root cause. Once we fully understand the nature of the incident, we will provide an update with additional information.

Keeping our users safe is one of our top priorities, and we sincerely apologize to anyone who was negatively impacted by this event.
![](http://feeds.feedburner.com/~r/video-js/~4/wMwHep00UvY)
