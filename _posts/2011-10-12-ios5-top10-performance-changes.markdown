---
layout: post
title: iOS 5 Top 10 Browser Performance Changes
date: '2011-10-12 08:00:41'
---


![](http://www.guypo.com/wp-content/uploads/2011/10/iso5.png)The much-hyped new version of iOS is finally here, and with it comes a new version of its browser. Here at Blaze we’ve been studying this new browser for a while, to learn what performance related changes it introduces.

Now that the release is official, we can finally share our insights (previously prevented by the Apple Developer NDA).  
  
 Before we dig into details, note that iOS offers three flavour’s of its browser.

1. MobileSafari – The standalone browser included with iOS.
2. Home-Screen Pages – Pages added to the home-screen by the user after browsing them within Safari. These pages are launched as easily as regular apps, and show up as separate applications in the task manager.
3. UIWebView – The embedded browser component used within different apps. Many popular apps such as Twitter, Facebook & Yelp use UIWebView, and many popular app development platforms, such as PhoneGap are built on top of them.

Each of these is launched in a different context, and has different features enabled. We therefore repeated our tests, where relevant, for each.

### Highlights

If you only have a minute to read this blog, here’s a quick comparison table:

[![](http://www.guypo.com/wp-content/uploads/2011/10/blazechart.png)](http://www.guypo.com/wp-content/uploads/2011/10/blazechart.png)

Share This: http://www.guypo.com/wp-content/uploads/2011/10/blazechart.png

### JavaScript Performance

One of the most common tests for any new browser version is its JavaScript performance. For iOS, there has been much controversy around whether the modern Nitro engine is used in Home Screen Pages and UIWebView, making it an even more important test than usual.

The table below shows the average results of 3 runs of the SunSpider JavaScript Benchmark (v0.9.1) in each environment. The test measures duration, so lower numbers are better. All tests were done on iPhone 4 hardware.

[![](http://www.guypo.com/wp-content/uploads/2011/10/sunspiderbenchmark.png)](http://www.guypo.com/wp-content/uploads/2011/10/sunspiderbenchmark.png)

<table><tr><th>OS/Browser</th><th>MobileSafari</th><th>Home Screen Pages</th><th>UIWebView</th></tr><tr><td>iOS 4</td><td>4052</td><td>10528</td><td>10044</td></tr><tr><td>iOS 5</td><td>3574</td><td>4551</td><td>12101</td></tr></table> 

As you can see, the JavaScript performance of MobileSafari is a little bit better, improving by roughly 10%. This is to be expected, since new browser versions usually improve performance, but the dramatic improvement was already delivered in iOS 4.3 not too long ago.

For Home Screen Pages, it’s clear that the Nitro engine is now being used. JavaScript performance improved dramatically in those apps, though it still lags a bit behind MobileSafari itself. Our guess is that the Nitro engine is indeed used, but some of its optimizations are disabled, presumably for security reasons.

Lastly, in UIWebView JavaScript performance actually declined! We were extremely surprised by this finding, but repeating the test after power cycles and using the Google V8 Benchmark led to the same results (V8 v3 score of 80 on iOS 4.3 vs 65 on iOS 5). We don’t have a good theory as to why this happened. Perhaps the iOS 5 JavaScript Engine was designed to assume newer hardware? Perhaps it was just an oversight? Any theories (or better yet, data) are welcome.

### Rendering Performance

A key performance enhancement in iOS 5 is GPU accelerated rendering. The GPU (Graphics Processing Unit) is much faster than the CPU at processing graphics related actions, such as 3D rendering. Many modern browsers allow websites to leverage the GPU through specific CSS and JavaScript actions, such as translate3d.

iOS 4 performs very poorly when running those predefined graphics actions, and Microsoft created a test called “HTML5 Speed Reading” demonstrating how IE 9 Mobile is way faster. In iOS 5, Apple responds with a dramatic improvement, performing 20x better in Microsoft’s test rendering 40 FPS compared to 2 FPS on iOS 4. [Windows Phone 7 clocks at about 24 FPS, and Nexus S at 10 FPS](http://www.youtube.com/watch?v=Or3wvF9ts0I&feature=player_embedded).

Here’s a short video showing iOS 4 vs. iOS 5, clearly demonstrating the improvement:

While most websites won’t become 20x faster in iOS 5, some rich HTML5 functionality will go from unusable to working smoothly. For instance, Joe Hewitt’s [iScrollability](http://joehewitt.com/2011/10/05/fast-animation-with-ios-webkit) works great on iOS 5, but is far from ideal on iOS 4.

### HTTP Pipelining

 Another very interesting change in iOS 5 is the introduction of HTTP Pipelining. As we outlined in [previous](http://mobitest.akamai.com/http-pipelining-big-in-mobile/) [posts](http://www.guypo.com/http-pipelining-request-distribution-algorithms/), HTTP Pipelining is a great technique to help address the long latency mobile networks suffer from. Android and Opera have been using Pipelining for a while, and now iOS is joining the party.

HTTP Pipelining is enabled for all browser modes (MobileSafari, UIWebView, Home-Screen). Some private API method names indicate determined developers can probably find ways to disable pipelining in their apps if they have reason to do so.

Digging deeper into its behaviour, here are the specifics on how Pipelining is used in iOS 5 (the terms are explained [here](http://mobitest.akamai.com/http-pipelining-big-in-mobile/) and [here](http://www.guypo.com/http-pipelining-request-distribution-algorithms/)), compared to the other browsers that support it:

<table><tr><th>Mobile Browsers</th><th>Server Support Detection</th><th>Max pipelined requests</th><th>Max connections per host</th><th>Pipeline vs. Connection Distribution</th></tr><tr><td>iOS 5.0</td><td>Per Host</td><td>–</td><td>6</td><td>Fill First*</td></tr><tr><td>Android</td><td>Per Connection</td><td>3</td><td>4</td><td>Fill First</td></tr><tr><td>Opera Mobile</td><td>Per Host</td><td>11</td><td>4</td><td>Distribute First</td></tr><tr><td>Opera Mini</td><td>Per Host</td><td>4</td><td>10</td><td>Distribute First</td></tr><tr><td>Blackberry</td><td colspan="4" style="text-align: center">– Not Supported –</td></tr></table>* Except for the first set of requests per connection. See below.

iOS 5 mixes and matches other browser behaviours, using Opera’s more aggressive detection model and Android’s request distribution model. Blackberry remains the only top browser not to support HTTP Pipelining, at least for now.

One twist iOS 5 introduces is that the first request on every connection is distributed first, and only then the pipe is filled. For example, if a page makes 24 requests to a host (after verifying host support), split over the 6 connections MobileSafari opens, then:

- Connection 1 will send requests 1, 7, 8 and 9
- Connection 2 will send requests 2, 10, 11 and 12
- Connection 3 will send requests 3, 13, 14 and 15
- Connection 4 will send requests 4, 16, 17 and 18
- Connection 5 will send requests 5, 19, 20 and 21
- Connection 6 will send requests 6, 22, 23 and 24

Personally I think the “Distribute-First” model is better, so I’m happy to see that at least the first requests are distributed, even if the latter ones are “Filled-first”.

### Parallel Downloads

A large component of browser performance has to do with downloading files in parallel. Put simply, the more files are downloaded in parallel, the better the performance. On this field, iOS 5 brings with it good and bad news.

The good news is the new support for the[ “async” script attribute](http://www.w3schools.com/html5/tag_script.asp). This attribute allows website owners to explicitly mark a script as “async”, and thus keep it from blocking the page loading event or other scripts on the page. Support for async scripts is especially important on mobile networks, as they make it easier to keep 3rd party scripts from delaying the page load.

The bad news is the removed support for downloading CSS files alongside other resources. Just last week we posted a [blog about the CSS bottleneck](http://www.guypo.com/eliminating-the-css-bottleneck/), and discussed how CSS files in the body are downloaded alongside other files in WebKit browsers. iOS 4.3 worked this way, meaning it didn’t wait for the CSS files to complete before downloading the images on a page. In iOS5, the images won’t start getting downloaded until all CSS files are fully downloaded.

You can test both of these properties and more by running [BrowserScope’s Network Tests](http://www.browserscope.org/network/test).

### Caching

For the most part, caching hasn’t changed in iOS5. Individual files of at least 4 MBs are still cacheable (as tested using [Steve Souders’ test](http://www.stevesouders.com/blog/2010/07/12/mobile-cache-file-sizes/)), persistent cache is still zero, and memory cache can still reach 100MB at times (using the same tests [we used before](http://mobitest.akamai.com/understanding-mobile-cache-sizes/)).![](http://www.guypo.com/wp-content/uploads/2011/10/apple_folder.png)

The main changes happened in how Home Screen Pages handle caching. As Souders [observed](http://www.stevesouders.com/blog/2011/06/28/lack-of-caching-for-iphone-home-screen-apps/), Home Screen Apps didn’t enjoy any persistent cache in iOS 4. In iOS 5 that is fixed, and these apps do enjoy proper caching, and don’t need to reload resources on load.

In fact, these Home Screen Pages now enjoy a **better** cache than MobileSafari does. Running the same tests Souders did on the untapped app showed that even after killing the process or restarting the device, the cached data persisted. Repeating the test for other pages showed the same results.

Adding Nitro to Home Screen Pages, as well as giving them a better cache than MobileSafari enjoys, seriously challenges any conspiracy theories claiming Apple is looking to cripple such apps…

UIWebView seems to remain unchanged, offering some memory cache if used with the right settings, but no persistent cache.

### HTML5 Web Workers Support

![](http://www.guypo.com/wp-content/uploads/2011/09/html5.jpg)  
 HTML5 introduces a new entity called Web Workers. These workers are side threads used for running time consuming JavaScript actions. Running such actions on a separate thread leaves the main thread free for user interaction and UI, and makes the application more responsive. You can read more about Web Workers [here](http://www.codediesel.com/javascript/introducing-html5-web-workers/).

The Android Browser supported Web Workers since Android 2.0, but iOS did not support them until now. iOS 5 now added complete support for Web Workers, which is great news for rich applications.

**UPDATE:** Android removed support for Web Workers in Android 2.2, due to [security concerns](http://code.google.com/p/android/issues/detail?id=10004#c7). You can see the full list of browser support for Web Workers [here](http://caniuse.com/webworkers).

### Better localStorage Management

As the use of localStorage grows, the need to clear this data increases. iOS 5 acknowledged that by replacing “Clear Cookies” and “Clear Cache” buttons with a “Clear Cookies and Data” button, which clears cookies, cache and localStorage information.

In addition, under Advanced Settings users can see the amount of data stored for each site, and clear it. However, our testing indicates the size information is still a bit buggy, and often only shows cookie size data, or an inflated localStorage size.

### So? Is It Faster?

This post is meant primarily to explain the differences, but obviously we were curious about the bottom line impact it has on performance too. Our initial testing shows iOS 5 loaded pages are 10% faster than iOS 4.3.

These tests were performed using the iOS Simulator (4.3 and 5.0) over simulated 3G network speeds. The measurement was done using the [Mobitest](http://mobitest.akamai.com/) technology, and the test set was Top 100 US Websites, as defined by Alexa. Each page was measured 3 times, and both tests were performed overnight.

The average page load time on iOS 5 was 12.6 seconds, compared to 14 seconds on iOS 4.3. The median page load showed a similar gap, going from 10.4 seconds on iOS 4.3 to 8.7 on iOS 5.

These are only preliminary results, and we hope to run more comprehensive tests later on, including tests of the new hardware as well. We will also make iOS 5 available for testing on Mobitest shortly.

### Non-Performance Goodies

Beyond these performance related improvements, iOS 5 offers various browser enhancements, such as:

- Porting Reader from desktop Safari to Mobile, making it easier to read content on cluttered sites
- Improved Security controls include private browsing or controlling set-cookie permissions.
- Enhanced HTML5 support, in areas such as Canvas, form inputs and CSS 3.0 functionality

### Summary

iOS 5 brings with it an evolved browser, and improves many aspects of the browser. The most impressive enhancement is the GPU enabled rendering acceleration, beating Microsoft in its own test. The most unexpected new feature is the use of HTTP Pipelining – a great move, but clearly an informed decision and not simple evolution.

That said, it’s also slightly disappointing. The regression in UIWebView JavaScript performance and making CSS files block other resources is unexpected, and degraded performance is something we never expect. In addition, all the enhancements feel like simple evolution, but not revolution, and performance enhancements like Navigation Timings or JavaScript detection of the connection types are notable absences.

Still, iOS 5 is a free upgrade, and customers can expect slightly faster browsing and better caching. Website owners have a few more tools to use to accelerate their websites on the iOS platforms. All in all, iOS 5 is good news for mobile web performance.


