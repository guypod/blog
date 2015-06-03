---
layout: post
title: Mobile Performance Testing Stats From the First 100 Days
date: '2011-05-06 15:48:11'
---


[![](http://www.guypo.com/wp-content/uploads/2011/05/bigmscore60.png)](http://mobitest.akamai.com/)It’s been almost 3 months since we launched [Mobitest](http://mobitest.akamai.com/), and we’ve learned a lot in that time. The service is getting used more and more, and it’s always nice to see the phones get hyper-active when we get a mention in some presentation 😉

By now the service is more robust, and we’ve officially removed the Beta mark – coinciding with the Blaze product leaving Beta. At the same time, we gathered up some statistics to share from the tests submitted.

### Pages Load Slower In “Real Life”

The first thing we noticed was that the average load time was 6 seconds (without video, more about that later). This is almost 3 times longer than the findings we had in our Fortune 1000 benchmarks!  
 Sifting through the data, the best theories we have are the set of sites and time of testing. Most tests were run during business hours, when the web is much slower, while our benchmark was run at night, to reduce variability. In addition, many of the sites loaded were lower volume, less sophisticated sites, often not using CDNs and not highly optimized.

### Video Adds ~30%

Many of the tests were run with Video, to get a visualization of the load time. We reduced the video frequency to 1 Frame-Per-Second (FPS) to reduce its impact on performance, but we could tell it still takes its toll.

Measuring across many tests, we can now say video adds about 30% to the load time. This stat remains consistent across the different devices. While we still recommend measuring without video if you want more accurate numbers, this helps us know how much adjustment is needed.

### iPhone The Popular Client, Google The Popular Site

About two thirds (66%) of tests were submitted on iPhone. This could be impacted by the fact iPhone is first on the list, but it holds even now when we have one iPhone entry and two Android entries… Looks like most users care about iPhone timing more.

At the same time, the most measured URL was google.com. 3.5% of all tests we received were for google.com, most directly to http://www.google.com/, and some to other paths. The runner up was cnn.com, accounting for ~1.5% of tests.

### More Granular Percentiles

Now that we have more data from a diverse set of sites, we’ve updated our percentiles and divided them into 10% groups. The following table shows how the percentiles will be divided from now on:

<table><tr><th>Percentile</th><th>Number of milliseconds</th></tr><tr><td>Faster than 90% of Sites</td><td> 1,539</td></tr><tr><td>Faster than 80% of Sites</td><td> 2,533</td></tr><tr><td>Faster than 70% of Sites</td><td> 3,400</td></tr><tr><td>Faster than 60% of Sites</td><td> 4,323</td></tr><tr><td>Faster than 50% of Sites</td><td> 5,391</td></tr><tr><td>Faster than 40% of Sites</td><td> 6,525</td></tr><tr><td>Faster than 30% of Sites</td><td> 8,208</td></tr><tr><td>Faster than 20% of Sites</td><td> 11,407</td></tr><tr><td>Slower than 10% of Sites</td><td> 18,662 </td></tr></table>Note that these measurements are still taken over wifi, and represents runs without video. Now that we know the impact video has, we’ll account for it in our percentile calculation.  
 We’ll share more info over time, and we have some great features and capabilities in the pipe. To stay up to date follow us on twitter – twitter.com/blazeio


