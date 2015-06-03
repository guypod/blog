---
layout: post
title: Mobitest – New and Improved (and Renamed)
date: '2011-02-28 19:04:18'
---


As you know, a couple of weeks ago we launched a community service for measuring page load times on mobile devices. It looks like we weren’t the only ones feeling the pain of not having such tools – the traffic and feedback on the service has been overwhelmingly positive.

[![Mobitest Tool](http://www.guypo.com/wp-content/uploads/2011/02/mobitestupdate.png)](http://mobitest.akamai.com/)

Over the last couple of weeks thousands of URLs were submitted through the tool, uncovering quite a few bugs in the BETA tool, including accuracy issues, stability issues, and long queues. We received a lot of great feedback, helping us prioritize the urgent issues and enhance our roadmap.

We also discovered was another site called Blaze Mobile. To avoid any confusion or issues with them, we’re renaming the service to “Mobitest”. Please use this name when referencing it in future and past notes.

### New (not so) Minor Release

So, after a fair bit of scrambling and some sleepless nights, today we’re releasing a new release of [Mobitest](http://mobitest.akamai.com/). The new version includes many bug fixes, and a few small features.

Here are the highlights of the updated service:

- New Device: Google Nexus S, running Android 2.3.
This is being added to the Android 2.2 powered Samsung Galaxy S.

- Improved reliability.
Fixes to various crashes, auto-recovery mechanisms and more devices mean the service should be up and running constantly, and queue sizes should be very small.

- Major bug fixes:
- iPhone resource sizes were reported as too large
- Android often showed 1-ms resources for many pages
- iPhone/Android cache wasn’t fully cleared in some cases
- iPhone occasionally showed wrong blocking JavaScript files

- Improved percentile values (based on larger data-set)
- Various small UI improvements, such as:
- [History of tests run](http://mobitest.akamai.com/history/) (public tests only, most tests are submitted as private)
- Improved icons


Note that there is still a known issue on some Android test where resources are being displayed as 1ms resources. We’re fixing those on a case by case basis, so if you come across an example, please email it to mobitest@blaze.io.

### Keep the Feedback Coming

The good news is that we’re getting a lot of feedback. The bad news is that we can’t do it all at once… So, knowing what you want to see is critical to make sure we use our time well.

Please use the Feedback button, the comments below, or email mobitest@blaze.io to let us know what you want to see next. We’ll put together a roadmap and communicate it out once we get enough data points.


