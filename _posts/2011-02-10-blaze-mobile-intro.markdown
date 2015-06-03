---
layout: post
title: 'Mobitest: Measure Mobile Web Performance'
date: '2011-02-10 17:07:22'
---


[![](http://www.guypo.com/wp-content/uploads/blazemobileiphone.png)](http://mobitest.akamai.com/)  
 Today we’re excited to announce a new community service – [Mobitest](http://mobitest.akamai.com/).  
 Mobitest is a free service that lets website owners get detailed web page load performance on various mobile devices. Let’s take this sentence apart…  
 Detailed web page load performance means all the great info we got used to seeing from [YSlow](http://developer.yahoo.com/yslow/), [WebPageTest ](http://www.webpagetest.org/)and other tools – but on a phone. This includes load times, waterfall charts, detailed HAR files, video playback and more. This type of information is critical when trying to understand a web page’s performance, and was hard to impossible to obtain for mobile devices.

Various mobile devices means real physical smartphones browsing the web page. We avoided emulators which run on very different hardware and don’t’ reflect a real user’s experience. The devices include iPhone 4 and Samsung Galaxy S (Android 2.2) to begin with, and we expect to constantly add more devices and platforms. More details about that later.

Free service means just that – no charges, no registration, no hidden fees. Just browse to [http://mobitest.akamai.com/](http://mobitest.akamai.com/), enter a URL and select a device. We created it to be a community service, and we hope it’ll give the mobile web a big performance boost.

**BETA DISCLAIMER:**Mobitest is still in Beta, and we expect a large volume of initial traffic. Please be patient until we iron out the quirks and let us know of anything that doesn’t work as it should.

Here’s a look at our “Mobile Data Center”: [![](http://www.guypo.com/wp-content/uploads/2011/02/Mobile-Lab1-240x300.jpg)](http://www.guypo.com/wp-content/uploads/2011/02/Mobile-Lab1.jpg)

Before diving into the details, this is a good time to thank [Pat Meenan](http://blog.patrickmeenan.com/), first and foremost for the great WebPageTest platform he created, but also for his help and advice whenever needed. Another thanks goes out to [Steve Souders](http://stevesouders.com/), [Stoyan Stefanov](http://www.phpied.com/) and [Sergey Chernyshev](http://www.showslow.com/) for their great feedback, which delayed the release by a bit but made it that much better…

### Behind the Scenes

Mobitest is made up of two main parts – WebPageTest and Mobile Agents.  
 WebPageTest is well known and widely used. We use a modified private instance of it, enhanced to meet our needs. Experienced WPT users would notice its use in the waterfall charts, videos and other indicators.

The Mobile Agents are the key new technology. These are custom applications created individually for each device. Each app acts as a WPT agent, polling for URLs to measure, loading up the page, and reporting the results back. Their exact flow can be seen in the [“Methodology” page](http://mobitest.akamai.com/methodology).

Beyond the obvious dev effort, the real difficulty in building these agents was in getting the data out. Mobile devices are extremely locked down and make it very hard to get detailed performance measurements. What we initially thought would take a few weeks of work turned into many months, even with help from [top mobile development experts](http://selectstartstudios.com/). Eventually we got to the data, and while there are still details we’ll want to add over time, the most useful information is available.

Note that our UI tries to keep things simple for most users, but the “View HAR File” link allows the hardcore users to dig into the details. These files don’t currently hold the raw content of the files, but are otherwise pretty complete.

### Load Time Percentile

Since the Mobile Measurement world is practically non-existent, it’s hard to assess whether your site is fast or slow relative to the average site? To help benchmark your performance, we measured the mobile load times for the Fortune 500 websites. Every run is compared to this benchmark and states how fast your site is in comparison.  
 Over time as we get more sites through Mobitest, we expect to improve this index and perhaps break it down by industry.

### Futures

This tool isn’t perfect, and we have a lot of things we’re still working on. To name a few:  
 – Include Cached View  
 – Add Start Render & Fully Loaded times  
 – Calculate a PageSpeed Score  
 – Adding 3G measurements (currently tests are done over Wi-Fi)  
 – More platforms  
 – More phones  
 – More networks & locations  
 – Additional performance metrics, such as CPU Usage, Memory, etc.  
 – …  
 So clearly we have big plans for this tool, but most of all we want to hear from you what YOU want to see being added, changed or fixed. Please let us know via the comments, the Feedback tab on the left, or privately at [mobile@blaze.io](mailto:mobile@blaze.io).


