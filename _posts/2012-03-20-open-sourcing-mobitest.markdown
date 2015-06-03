---
layout: post
title: Open-Sourcing Mobitest
date: '2012-03-20 06:15:01'
---


![](http://www.blaze.io/wp-content/uploads/2012/03/osi_standard_logo.png)I’m excited to announce that today we’re open-sourcing Mobitest!

We always saw [Mobitest](http://www.blaze.io/mobile/) as a community tool. Following the acquisition, we raised the idea of open-sourcing it to the relevant people in Akamai, and it was immediately accepted. And so, roughly a month after the acquisition, we’re ready to share the code with the world!


## Mobitest Huh?

If you’re not familiar with it, here’s a short recap on Mobitest.

Mobitest is a unique technology able to measure page load times on real mobile devices. It offers detailed performance information, ranging from total page load times to individual request headers and timings. It can also capture screenshots during page load, and show a video visualizing the page load as it happened.

Mobitest runs on iOS, Android and Blackberry, regardless of the hardware – smartphone, tablet or simulator. It uses the default (embedded) browsers the OS offers, and can measure over any network connection the device is connected to, for instance WiFi or a 3G connection.

The Mobitest agents are installed on the device itself. Once installed, they run in an infinite loop on the device, turning it into a (very) small server. The devices poll a webpagetest server, and you can submit test requests and view results through the webpagetest UI.

We also offer a hosted instance of Mobitest on [http://blaze.io/mobile/](http://blaze.io/mobile/). This hosted version is a free service that lets you use our own devices to measure, which is easier than setting up your own. What we’re open-sourcing today is the mobile agent code (the real technology), but the hosted version is still up and running, and we encourage you to use it.


## Measuring The Right Thing

Performance measurement tools are a critical component of making the web faster.  You can’t know how much to invest in performance if you can’t understand your current state? This is true across all dimensions of performance, and is just as important in Mobile, if not more.

That said, measuring accurately is also very hard. The most common way to measure performance is Synthetic Testing, where you make an agent you control browse your site and take measurements in the process. While that may *sound *simple, the devil is in the details. Using different networks, running on different hardware and using different browsers (simulated or real) can all dramatically change the results.

On Mobile specifically, synthetic testing still has a long way to go. Shortcomings in the existing tools are what drove us to build Mobitest in the first place. We needed a tool that can measure on real mobile devices, using real mobile browsers, over a real mobile network.

In the process we found solutions to all sorts of problems, some logical and some hacky. By open sourcing this code we hope others can learn from what we’ve seen, both to use it as is and to build upon it.


## What about RUM?

It’s worth mentioning that a different performance measurement technique is called Real User Monitoring (RUM). RUM includes capturing load times from real users browsing your site, and aims to give you a more accurate view of your website’s performance as experienced by your actual users.

I’m a huge advocate of RUM, but I see it as complementary to synthetic testing, not a replacement. RUM measures the “right thing”, but it provides a lot less information to truly understand what led to those load times. It’s also hard to impossible to use RUM for measuring changes to your site, or for monitoring your performance to verify it doesn’t fluctuate.

RUM is even harder to implement on Mobile. RUM on desktop was greatly helped by the [navigation timing](https://dvcs.w3.org/hg/webperf/raw-file/tip/specs/NavigationTiming/Overview.html) standard, but on Mobile less than 1% of clients support it. In the future, I expect RUM to grow in importance, partly at the expense of synthetic testing, but there will always be a need for both techniques.


## So Where is the Code?

Now to the technical details.

The Mobitest code is hosted here:  [http://code.google.com/p/mobitest-agent/](http://code.google.com/p/mobitest-agent/)

The code includes agents for the three different platforms: iOS, Android and Blackberry. The iOS and Android versions are quite mature, the Blackberry version is still very much an Alpha version. Each project includes more detail and specific instructions. All of these agents require a webpagetest server (version 2.5 or higher).

The iOS and Blackberry agent also have a secondary app that acts as a crash recovery app. Mobile phones are not designed to be servers, so we had to get creative to overcome various crashes and memory leaks. On Android we could do that using OS hooks, but on iOS and Blackberry we needed a second app to keep the app running smoothly.

[![](http://www.blaze.io/wp-content/uploads/2012/03/mobitest-opensourced.png)](http://code.google.com/p/mobitest-agent/)

If you just want to browse the code and understand what it does, just click the link above and browse away. If you want to actually install it on your own device, you will need the appropriate developer license from [Apple](https://developer.apple.com/devcenter/ios/index.action), [Google](http://developer.android.com/index.html) or [RIM](https://bdsc.webapps.blackberry.com/java/). As always, you can use the freely use the [hosted version of Mobitest](../mobile/) if you just want to run some tests.


## How Can I Contribute?

Now it’s your turn to try it out and send us any feedback.

Keep in mind that this is first open sourced version, and I’m sure the documentation isn’t as good as it should be. The preferred way to send such feedback is to log an issue [in the project](http://code.google.com/p/mobitest-agent/issues/list). This is true for lacking documentation, bugs, enhancements or any other wishes.

Better yet, if you’re interested in contributing code fixes or becoming an active developer in the project, send us a note at [contact@blaze.io](mailto:contact@blaze.io) or through the project.


