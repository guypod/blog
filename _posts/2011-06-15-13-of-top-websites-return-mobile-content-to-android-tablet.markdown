---
layout: post
title: 1/3 Of Top Websites Return Mobile Content to Android Tablet
date: '2011-06-15 08:00:11'
---


### Creating a poor browsing experience when comparing to iPad

A strong ecosystem is a critical factor in the success of mobile platforms today. Acknowledging this fact, Google, Apple and other vendors woo app developers and boast rich SDKs. However, [browsing is by far the top activity](http://www.businessinsider.com/how-people-really-use-the-ipad-our-exclusive-survey-results-2011-5#the-ipad-is-a-web-surfing-machine-first-and-foremost-6) on mobile devices. Is the browsing experience the same on all of them?  
  
 To answer that question, we created a study, announced from the showroom floor at the [Velocity 2011 conference](http://velocityconf.com/velocity2011). In the study we measured the top 500 sites in the US, according to Alexa. We used Nexus S and Motorola XOOM to represent Android. Our analysis showed that a similar number of sites are optimized for Android and iOS smartphones, returning a mobile version of the site (42% iPhone vs. 38% Android). However, Android tablets didn’t fare as well as the OS’s phones.

84% of the sites with a mobile Android version returned the same mobile page to the tablet. While this helped pages load very quickly on the XOOM, it otherwise created a poor browsing experience. In contrast, for the iPad, virtually all these sites (92%) recognized the iPad and redirected to either a desktop site or an iPad-specific site. Since many sites didn’t have a mobile version even for the phone, the end result was a third of the top 500 sites returning a mobile version to the full screen Android tablet.

Note that these results are based on our methodology, which is outlined below.

### What’s wrong with a Mobile Site on a Tablet?

[![](http://www.blaze.io/wp-content/uploads/2011/06/cnetonzoom.jpg)](http://www.blaze.io/wp-content/uploads/2011/06/cnetonzoom.jpg)  
 A mobile site is a loosely defined term used to describe a site customized for a smartphone. Mobile sites are thus designed for small screens, are often more textual and provide a less rich design.

Viewing mobile websites on a tablet creates two problems. First, the website is designed for a small screen, and often renders poorly on the larger screen. Second, and more importantly, the websites do not take advantage of the bigger screen, creating an inferior browsing experience for the user.

Some see Tablets as a replacement for netbooks and laptops, at least for functions such as browsing. Clearly in such cases users would prefer to browse a full-feature rich site, and not the limited mobile version.

To get a feel for what this feels like, take a look at some of the examples below, showing screenshots

<div style="float: left;clear both">[![](http://www.blaze.io/wp-content/uploads/2011/06/accuweather.jpg)](http://www.blaze.io/wp-content/themes/Blaze/images/accuweather_big.jpg) [![](http://www.blaze.io/wp-content/uploads/2011/06/addictinggames.jpg)](http://www.blaze.io/wp-content/themes/Blaze/images/addictinggames_big.jpg) [![](http://www.blaze.io/wp-content/uploads/2011/06/americanairline.jpg)](http://www.blaze.io/wp-content/themes/Blaze/images/americanairline_big.jpg) [![](http://www.blaze.io/wp-content/uploads/2011/06/americanexpress.jpg)](http://www.blaze.io/wp-content/themes/Blaze/images/americanexpress_big.jpg)</div><div style="width: 100%;float: left;clear: both"> </div>### Why Is This Happening?

The obvious reason for this finding is that Android tablets are newer and site owners have not gotten around to supporting them. Their sites detect an Android device requesting the page and assume it’s a small screen phone when really it was a 10 inch tablet.

Another factor is that supporting Android devices is not as simple as iOS. For iOS there are only a handful of phones and one tablet (two versions of it) with known resolutions and screen sizes. For Android, there are [over 310 different phone and tablet devices](http://googleblog.blogspot.com/2011/05/android-momentum-mobile-and-more-at.html) in many shapes and sizes, with new ones coming out monthly. Variety provides more choice to consumers but creates a burden for sites owners to keep pace.

Our testing was done using the Motorola XOOM, the first Honeycomb (Android 3.0) tablet. This version of Android is used exclusively by tablets, making it easy for website owners looking for it to identify such clients. However, previous Android tablets, such as the Galaxy Tab, use the same OS version as the phones. The only way (as far as we know) for website owners to identify them is to keep track of all the different models, an extremely hard task, or use javascript-based detection (“feature detection”), which has its own limitations. We therefore expect such tablets to be confused for phones even more frequently.

### The Impact of New Hardware

As a part of the same study, we also measured the load time on these sites on the different devices. We compared five different devices:

- iPhone 4 (iOS 4.3.3)
- iPad 1 (iOS 4.3.3)
- iPad 2 (iOS 4.3.3)
- Nexus S (Android 2.3.3)
- XOOM (Android 3.0)

Since we use the embedded browser, we couldn’t safely compare performance between Android and iOS, so we kept our comparisons to be within the same OS (see more about our methodology below). Our goal was to assess the impact of new hardware on browsing speed. We only compared sites that returned the same content to all devices, to be sure we’re comparing apples to apples (and Androids to Androids).

The newest devices seemed to make an impact. iPad 2 was on average 24% faster than iPhone 4, and 19% faster than iPad 1. This trend matches the CPU power on these devices, as while iPhone 4 and iPad 1 use the same CPU, the iPhone CPU is presumed to be underclocked. iPad 2 sports a dual-core CPU (doubling processing power) and a better GPU, adding up to a significant impact.  
[![](http://www.blaze.io/wp-content/uploads/2011/06/applespeed.jpg)](http://www.blaze.io/wp-content/uploads/2011/06/applespeed.jpg)  
 Comparison of the Android devices showed a similar 25% improvement between the XOOM and the Nexus S. In this case the devices are also running different versions of the OS, as Android 3.0 is a tablet-only OS. Between the new hardware and the newer version, the already fast browser was significantly improved.  
[![](http://www.blaze.io/wp-content/uploads/2011/06/androidspeed.jpg)](http://www.blaze.io/wp-content/uploads/2011/06/androidspeed.jpg)

### Comparing User Wait Time

When comparing the performance of the different devices, we intentionally limited ourselves to comparing only the sites that returned the same page. However, from a user perspective what we care about is overall wait time across all sites. This reflects how long do users wait until the page they browsed is fully loaded.

For Android, this comparison yielded roughly the same results. This was expected, as have we’ve noted practically all sites returned the same content to the Android phone and tablet. Therefore, users browsing on the XOOM would enjoy pages loading 25% faster.

For iOS, we saw that iPhone loaded pages 12% faster than the iPad 2, despite the fact the iPad 2 hardware was much faster. The average page size on the iPhone was also more than 40% smaller – about 370K for iPhone compared to 620K for iPad 2. This shows that while the improved hardware helped, the richer websites served to the iPad are considerably slower, and users will have to wait longer for pages to load.  
[![](http://www.blaze.io/wp-content/uploads/2011/06/applespeed2.jpg)](http://www.blaze.io/wp-content/uploads/2011/06/applespeed2.jpg)

### Methodology

To perform our study, we used the [Mobitest](http://www.blaze.io/mobile/) technology for measuring and collecting data. We recently added tablets to Mobitest, allowing for these new insights. Mobitest uses the embedded browser of each platform for measurements. Our measurements were all taken during night time, over a WiFi connection from our Ottawa, Canada lab. For each device, we measured each site 3 times. Each time we loaded the page 3 times, and took the median load time. We then averaged those results to achieve the final numbers. Measuring at night and many times helps reduce the variability caused by the network, and achieve more accurate results.

To identify a mobile site, we measured the same pages on IE8, and compared the number of resources on each page in both cases. If the page served to the phone or tablet had at least 30 resources less than the same URL on IE8, it was marked as mobile. We then manually reviewed the results, and saw that this heuristic is accurate +/- 5%. Sites that were simple on the desktop too were missed, but otherwise the heuristic was deemed good enough to use.

Note that Apple has stated that their embedded browser (UIWebView) is more limited than Mobile Safari, which may have some impact on the load times. We therefore avoided comparing load times between Android and iOS, and only compared devices within the same OS family. We still hope Apple addresses the limitations of the mobile browser – maybe in iOS 5?  
 Mobitest is a free and open web service, allowing everyone to run their own tests. For any questions and for access to the raw data, [please contact us](http://www.blaze.io/contact/).


