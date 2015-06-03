---
layout: post
title: 'Mobitest Update: iOS 5, Blackberry and Simulated 3G'
date: '2011-11-08 10:09:38'
---


It’s been a while since we updated about Mobitest, and we’re way overdue for an update. Despite the lack of updates, Mobitest use has been growing quickly, and we have some exciting new additions we’re releasing today too.

First some words about growth. Mobitest has grown dramatically since we launched back in February of last year. Just in the last couple of months, Mobitest traffic has tripled, and with it the number of tests submitted. Testers clearly care about iOS the most – 55% of the tests are sent on iOS, and 69% of the tests on iOS. Granted, the fact iPhone is the default device probably helps those stats…  
  
[![](http://www.blaze.io/wp-content/uploads/2011/11/mobiteststats.png)](http://www.blaze.io/wp-content/uploads/2011/11/mobiteststats.png)

The backbone technology is also playing a greater role outside of the public site. By now we’ve run 13 batch tests for the [Mobile HTTP Archive](http://mobile.httparchive.org/), including the latest one being 2,000 URLs. We used Mobitest in various studies such as the iOS 5 Review and various presentations. We also have several private instances of Mobitest deployed by now, used by 3rd party organizations.

### New: iOS 5

Moving on to the new announcements. The first update is iOS 5 – we’ve upgraded most of our iOS devices to iOS 5. Seeing as this is a free upgrade, we feel like this is the more representative OS to use. We kept one iPhone on 4.3, and will keep it there for a couple of weeks, to support comparisons between the versions.

We’ve already done a [thorough review of iOS 5 Browser Performance changes](http://www.blaze.io/mobile/ios5-top10-performance-changes/), now you can run these tests yourself!

### New: Blackberry – Alpha

For a while now, we’ve had a Mobitest agent running Blackberry in the works. The platform and device proved very problematic, which delayed us making it available to the public. We’ve now put in the extra effort, and think it’s ready to be used by all of you. While declining in market share, Blackberry still accounts for over 10% of mobile browsing in [Europe](http://gs.statcounter.com/%23mobile_browser-eu-monthly-201108-201110) and [North America](http://gs.statcounter.com/%23mobile_browser-na-monthly-201108-201110).

Adding Blackberry was especially important to us since Blackberry doesn’t allow measuring performance on an ad-hoc wireless network. For iOS and Android, and many other phones,if you have a device you can capture its traffic by connecting it to your phone as a [wireless access point](http://code.google.com/p/pcaphar/wiki/CaptureMobileTraffics). Unfortunately, Blackberry doesn’t allow users to connect to such “ad-hoc wireless networks”, leaving you blind to how resources were requested.

The device we use is Blackberry Torch, running OS 6. Our testing showed it to be much slower than the iPhone and Nexus S. Running the SunSpider JavaScript Benchmark took about 10.5 seconds to complete, compared to 3.5-4 seconds on the Nexus S and iPhone 4. When running the Top 100 US Websites (defined by Alexa) on a fast network, iOS and Android took less than 5 seconds on average to load a page, while the Blackberry Torch took almost 14 seconds on average!

Try it out and let us know what you think. Note that the agent is still marked as Alpha since we still know of some problems when running large numbers of URLs on it, and it has lower capacity than some of the other phones. Please be patient with it, and if you encounter problems let us know at [contact@blaze.io](mailto:contact@blaze.io).

### New: Simulated 3G Network

While the Mobitest technology supports measuring over 3G, we’ve yet to add that to our public service. We’re working through some options, and hope to add this capability soon.

In the meantime, we’ve added a consistent and robust alternative: the iPhone Simulator running over a simulated 3G connection. The connection is throttled to 1Mbps download, 0.5Mbps upload, based on a variety of sources, including [PCWorld’s latest measurement](http://www.pcworld.com/article/189592/atandt_roars_back_in_pcworlds_second_3g_wireless_performance_test.html). The connection also simulates a 300ms latency, based on Novarum’s study and our own measurements. If you believe these numbers should be different, [please let us know](mailto:contact@blaze.io).

While the iOS Simulator doesn’t represent the phone’s hardware, it does represent the software well. The simulated network speed also allows for a consistent measurement, which is impossible to achieve on a real 3G network (which is inherently inconsistent).

### What’s next?

As I mentioned above, we have real 3G coming soon. We’re also still interested in additional test locations, so if you’re interested in hosting a phone and sponsoring a test location, do [contact us](mailto:contact@blaze.io).

What more do you want to see? Post your thoughts in the comments below, and we’ll try to make them a reality.


