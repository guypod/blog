---
layout: post
title: Real World RWD Performance - Take 2
date: '2013-03-07 23:28:11'
---


About a year ago, I [ran a test](http://www.guypo.com/mobile/performance-implications-of-responsive-design-book-contribution/) comparing the performance of websites using Responsive Web Design (RWD) across different screen resolutions. The test consisted of loading the same RWD sites in Chrome using 4 different screen resolutions, triggering different “views” of the site, and comparing bandwidth and load times in each.

The test results [didn’t bode well](http://www.guypo.com/technical/responsive-web-design-is-bad-for-performance-there-i-said-it/). They showed that practically all RWD websites downloaded the full payload of the desktop website regardless of what was displayed. These websites would download hundreds of KB of unnecessary data to a small screen, only to hide or shrink them on the client, resulting in abysmal performance on mobile devices.

Over the past year, RWD performance has been discussed at length, with great posts from [Jason Grigsby](http://blog.cloudfour.com/first-thing-you-should-do-to-optimize-your-desktop-site-for-mobile/), [Tim Kadlec](http://timkadlec.com/2013/01/setting-a-performance-budget/) and many others. New tools such as [PictureFill](https://github.com/scottjehl/picturefill) and [Adobe Shadow](http://html.adobe.com/edge/inspect/) make optimization and testing easier, and some of the [conversation](http://blog.cloudfour.com/the-real-conflict-behind-picture-and-srcset/) even made it into [the HTML5 draft](http://www.w3.org/html/wg/drafts/srcset/w3c-srcset/).

So, with all this progress, I figured it’s time for a retest – are we faster yet?


## Quick Test Details

Similar to last year, I used [http://mediaqueri.es/](http://mediaqueri.es/) as a repository of responsive websites. using [WebPageTest](http://www.webpagetest.org/), I loaded each of the 471 websites currently inventoried (an increase from 347 sites last year) in a Chrome browser, resized to one of 4 different screen resolutions each time. I then collected the results, and looked at the number of bytes needed to download each page in each resolution.

You can see more details on the test methodology in [last year’s presentation](http://www.slideshare.net/guypod/performance-implications-of-mobile-design/), and can rerun the test on a given site with the instructions on [this slide](http://www.slideshare.net/guypod/performance-implications-of-mobile-design/50/).


## Problem Solved? Not really.

The results showed that, despite the better tools and conversation, most RWD websites still downloaded the full content of the page on every screen resolution. As you can see from the chart below, RWD pages loaded on the smallest screen were only 9% smaller than those on the largest screen (on average), despite huge differences in the amount of content shown and the physical size of images.

[![2013-page-size-per-resolution](http://res.cloudinary.com/guypo-blog/image/upload/v1431082700/2013-page-size-per-resolution_ebyq8r.png)](http://res.cloudinary.com/guypo-blog/image/upload/v1431082700/2013-page-size-per-resolution_ebyq8r.png)

Looking across websites, less than 7% of websites were at least 2x smaller when loaded on the smallest screen compared to the biggest screen. Another 22% weighed between 50-90% of the large-screen page size when loaded on the smallest one, and the majority (72%) were roughly the same size on the smallest and biggest screens.

[![2013-page-size-small-vs-big](http://res.cloudinary.com/guypo-blog/image/upload/v1431082699/2013-page-size-small-vs-big1_rtg24i.png)](http://res.cloudinary.com/guypo-blog/image/upload/v1431082699/2013-page-size-small-vs-big1_rtg24i.png)


## Not good, but better

While these results are definitely not *good*, they’re still better than what we saw last year. As you can see in the chart below, the percentage of websites that are much smaller or slightly smaller in 2013 was double what it was in 2012. In addition, the average page in 2012 weighed only 6% less on a small screen than on a big screen, compared to 9% today – again a notable improvement.

This means the enhanced evangelism and improved tooling for RWD performance did have an effect, which is a significant achievement. That said, we probably need this growth to be exponential, not linear..

[![trend-page-size-small-vs-big](http://res.cloudinary.com/guypo-blog/image/upload/v1431082700/trend-page-size-small-vs-big_zosiqb.png)](http://res.cloudinary.com/guypo-blog/image/upload/v1431082700/trend-page-size-small-vs-big_zosiqb.png)

Unfortunately, while the cross-resolution page size stats improved, the overall performance did not. The [HTTP Archive clearly shows](http://httparchive.org/trends.php#bytesTotal&reqTotal) how pages are constantly getting bigger, and this trend holds true for RWD sites as well.

The chart below (last one, I promise!) shows the average page size by window size. As you can see, the average page size has increased significantly across all the resolutions. Note that the sites included in each year’s averages are not exactly the same, but this trend holds even if we only look at the URLs shared across the two tests.

[![trend-page-size-per-resolution](http://res.cloudinary.com/guypo-blog/image/upload/v1431082701/trend-page-size-per-resolution_bourku.png)](http://res.cloudinary.com/guypo-blog/image/upload/v1431082701/trend-page-size-per-resolution_bourku.png)


## Summary

The bottom line is – our work is not done. We’re seeing some fruit of our performance preaching labor, but the vast majority of RWD sites still don’t devote enough attention (or time) to performance. As a result, if you happen to browse a responsive website on your smartphone, chances are it’ll be considerably slower than if you were browsing a dedicated mobile site.

We need to keep pushing and promoting RWD performance, keep building tools and making it easier to be fast, and find a way to improve RWD performance faster than the pace we make pages slower in general…

So for all my fellow performance enthusiasts out there, keep going, we’re not there yet!

Side Note: If you’re interested in the raw test data, you can download the detailed excel sheet with test results, pivot tables and links to the individual tests [here](http://www.guypo.com/wp-content/uploads/2013/03/RWD-perf-2013-results.xlsx).

 


