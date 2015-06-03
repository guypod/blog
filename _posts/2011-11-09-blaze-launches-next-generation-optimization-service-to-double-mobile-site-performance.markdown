---
layout: post
title: 'Blaze Launches Next Generation Optimization Service to Double Mobile Site
  Performance '
date: '2011-11-09 08:00:30'
---


### *Breakthrough technology the first to pre-execute JavaScript in the Cloud*

The recent launch of Amazon’s new Silk browser generated a lot of interest in the concept of a hybrid browser that pre-processed content in the cloud. While very innovative, this technology is limited to the users of Amazon’s new tablet.

![](http://www.blaze.io/wp-content/uploads/2011/11/mobilepic1.png)

Today, Blaze is pleased to announce the next generation of our Mobile Frontend Optimization (FEO) Service that will enable site owners to double the speed of their sites on most modern Tablet or Smartphone devices including iOS & Android. This new offering builds on our previous mobile optimization solution, adding new breakthrough innovations including the ability to pre-execute JavaScript in the Cloud.

See detailed blog post for more information on this exciting technology:

[http://www.blaze.io/mobile/javascript-pre-execution-for-mobile-taking-scripts-out-of-the-loop](../mobile/javascript-pre-execution-for-mobile-taking-scripts-out-of-the-loop)

### The Mobile Performance Challenge

Mobile browser usage continues to skyrocket and customers are demanding ever-faster performance. However, meeting user expectations for a two second load time is challenging for three reasons:

1. Mobile Networks are slower than broadband, have very long latencies and are unreliable.
2. Mobile Hardware is less powerful and is slow to process JavaScript, and uses different interaction models, such as touchscreens, which creates performance challenges.
3. Mobile Software, which creates unique environments such as small cache sizes and the use of HTTP pipelining, which must be accounted for when optimizing.

### The Importance of JavaScript on Mobile Load Times

Even with the steady improvements in processing power in the latest smart phones, Mobile devices, on average, take ten times longer to process JavaScript then desktop browsers.

[![](http://www.blaze.io/wp-content/uploads/2011/11/sunspider.jpg)](http://www.blaze.io/wp-content/uploads/2011/11/sunspider.jpg)

Clearly, improving JavaScript processing speed will have a large impact on Mobile performance and help to narrow the gap between the user experience on a desktop vs. Mobile device.

### Blaze’s Cloud Based Optimization Service – How it Works

Blaze is a cloud based Frontend Optimization (FEO) solution that sits between the Web server and the Browser. After the server dynamically generates the HTML, it is routed through a hosted Blaze Agent that applies a series of pre-computed optimizations. The user then receives the “Blazed” or modified page in their browser. Static files are delivered from the customer’s CDN provider.

[![](http://www.blaze.io/wp-content/uploads/2011/11/howblazeworks.jpg)](http://www.blaze.io/wp-content/uploads/2011/11/howblazeworks.jpg)

Offline, the Blaze Analysis Center is constantly monitoring the pages of a given site to pre-processes HTML, JavaScript and other pages resources to make online delivery faster. Blaze reduces load times by decreasing the number of page objects, the size of the objects and by accelerating rendering the browser. The resulting pages are typically 2x faster than they were previously.

[![](http://www.blaze.io/wp-content/uploads/2011/11/nascarvideo.jpg)](http://www.blaze.io/wp-content/uploads/2011/11/nascarvideo.jpg)

### New Mobile Optimizations

There are four new capabilities in Blaze’s latest Mobile Optimization update:

1. **JavaScript Pre-Execution.** Blaze makes real time browsing more efficient by expending CPU cycles offline. We do this by executing the JavaScript on the page offline and providing the browser with a mostly static page. Scripts that must be left dynamic are deferred till after the page load.
2. **Responsive Images.** There’s still a huge gap between broadband and cellular network speeds. Blaze detects if the user is on a slow connection and automatically serves up smaller, lower resolution images to speed downloads.
3. **Invoke click on touch.** When a user clicks on a link there is a 300ms delay as a Mobile browser waits to see if the user is pinching, zooming or actually clicking. This optimization converts hyperlink URLs to click events to eliminate this delay.
4. **Cellular connection keep-alive.** On a mobile device, each request for a page can have a 2-3 lag as the phone establishes a new connection to the cell tower. This optimization will send a dummy request to keep the connection open in between page requests to reduce start up latency.

These new optimizations add to an already rich set of Mobile performance features previously announced. These optimizations include:

1. **Adaptive consolidation.** Caching previously downloaded page elements on users’ browsers is one of the best ways to improve performance of repeat views. However, since the cache size on mobile browsers is very small, few files last in the cache until the user returns to the site. Blaze cache optimization leverage a new capability in HTML5 to bypass the shared browser cache and create a dedicated cache for each site.
2. **Asynchronous JavaScript.** Third party scripts are notorious for holding up the loading of other page objects. This optimization decouples script execution from the rest of the page load to maximize parallel processing.
3. **Asynchronous CSS.** Like the JavaScript optimization, asynchronous CSS unblocks the CSS from holding up the loading of other page objects.
4. **Adaptive image sizing.** Most images are formatted for desktop browsers on larger screens. Images are unnecessarily large for mobile devices. Blaze’s adaptive imaging sizing technology automatically delivers an optimally sized image for the phone, tablet or desktop browser that requested it resulting in a much faster page load.
5. **Just in time (JIT) image loading.** When viewing large sized pages on a small screen, the browser will load all the images on the page regardless of which ones are in view. Blaze’s JIT optimization will cause a page to only load the images that are visible within the current screen view. As the user scrolls down, new images are loaded on demand. JIT helps improve page loading speed and also reduces bandwidth for cases where a user doesn’t actually scroll down a page.

A complete list of optimizations can be found on the Blaze website at:

[http://www.blaze.io/overview/](http://www.blaze.io/overview/)

### Free Test Report

Blaze operates a free reporting service that allows site owners to measure the before/after speed impact of Blaze optimization. The reporting system will load a test URL through the Blaze optimization proxy and measure the speed improvement using a real iPhone device.

The test form can be found at: [http://www.blaze.io/](http://www.blaze.io)

### Pricing and Availability

The Blaze Mobile Optimization Service is now available as standalone service called Blaze FEO for Mobile or can be purchased as a combined desktop and mobile optimization service called Blaze FEO. Both these optimization services are available immediately and start at $399 per month.

### About Blaze

Blaze was founded in 2010 with a mission to help clients deliver better performing Web businesses by making their sites faster. User experience, conversions, search rankings and operational costs are all influenced by the speed of your site. Blaze provides a free service for measuring [Mobile Web performance](http://www.blaze.io/mobile/) and a subscription service that automates Frontend Optimization (FEO).


