---
layout: post
title: Why Repeat Visit Measurements Are Deceiving
date: '2010-12-08 15:00:35'
---


Website owners and performance tools both give special attention to a returning visitor’s user experience. Making your site faster for your loyal, repeat customers is obviously a good idea. However, there’s a fundamental disconnect between what we think we’re measuring and what we’re actually measuring. **The result is that we optimize for scenarios that don’t exist, and do nothing for the ones we actually care about**.

### Repeat Visits are NOT Return Visits

As a website owner, you probably see a return visitor as someone who visited your site yesterday, and then came back today (or later). In contrast, tools such as [YSlow](http://developer.yahoo.com/yslow/) and [Webpagetest](http://www.webpagetest.org/) simulate a user returning to the site instantly, effectively measuring the time it takes for the same page to refresh. That’s clearly not what most would consider a returning visitor.

To distinguish between the two, we’ll call an immediate return visit a Repeat Visit, and a visit at a later point in time a Return Visit. For the sake of our measurements, we defined the return visit as browsing the page 24 hours later. Optimization 101 is that you can only optimize what you measure. The result is that website owners work hard to speed up return visits, when in fact they’re only improving repeat visits. How much slower are return visits compared to repeat visits? As it turns out, they’re MUCH slower.

### How this affects the Real World

To get an estimate on the real world impact of these confusing metrics, we did an analysis on the fortune 100 list and the 10 top retail sites. The data showed that on average,** repeat visits were up to 10 times faster than return visits**. The average improvement was 40%. The return visits were only marginally better than a first visit, missing out on an opportunity to give your existing customers a better experience than the competition.

For example, consider a user going through gift ideas on the BestBuy website. The first time the user browses the site, the page will take about 4.4 seconds to load. When the user returns the next day to check some details about his favourite items, the site will still take 4.4 seconds to load. Had BestBuy optimized the return visit, the site would have loaded in an almost instantaneous 0.4 seconds, more than 10 times faster!

### Why is this happening to me?

Why are repeat visits so much faster? Logically, it’s because we optimized for repeat visits. If repeat visits were slow, we’d know about it and would do something about it. Technically, the main reason is the use of short term cache.

Websites often cache the page resources (images, CSS files, JavaScript files) for a few hours at most, since they fear users having a stale cache (an out-dated copy of a file in their cache). This short lived cache speeds up the immediate repeat visits, but by the time a return visit occurs, the cached items need to be downloaded again. Thus the total page size in the repeat visit is much smaller than that of the return visit, leading to a faster page load.

So how can you make your return visits as fast as your repeat visits?

- Use long-term caching. [Steve Souders](http://stevesouders.com/) and [other experts](http://www.phpied.com/caching-vs-inlining/) have long been touting the value of far-future expires headers, and techniques like [Versioning](http://www.blaze.io/optimizations/#versioning) can help avoid stale cache.
- Start measuring return visit times. To do our study we used a modified version of the terrific [Webpagetest](http://www.webpagetest.org/) tool, and have started working with [Patrick Meenan](http://blog.patrickmeenan.com/) on integrating these changes into the public version. We hope to see commercial tools such as like [Keynote](http://www.keynote.com/), [Gomez](http://www.gomez.com/) & [BrowserMob](http://browsermob.com/performance-testing) incorporate return visit measurements as well.

### For the data hungry reader…

Here are the stats on the top retail sites we analyzed, showing the load times for first visit, return visit and repeat visit. The fortune 100 data got to practically the same average with a very similar distribution.

<table border="1" cellpadding="0" cellspacing="0" style="padding: 10px" width="98%"><tbody><tr><td valign="top" width="20%">Domain</td><td valign="top" width="20%">First Visit (Sec)</td><td valign="top" width="20%">Return Visit (Sec)</td><td valign="top" width="21%">Repeat Visit (Sec)</td><td valign="top" width="17%">Optimization Opportunity</td></tr><tr><td valign="top" width="20%">Amazon.com</td><td valign="top" width="20%">3.3</td><td valign="top" width="20%">2.6</td><td valign="top" width="21%">0.9</td><td valign="top" width="17%">65%</td></tr><tr><td valign="top" width="20%">Overstock.com</td><td valign="top" width="20%">4</td><td valign="top" width="20%">3.4</td><td valign="top" width="21%">3.3</td><td valign="top" width="17%">3%</td></tr><tr><td valign="top" width="20%">Walmart.com</td><td valign="top" width="20%">9.7</td><td valign="top" width="20%">9.9</td><td valign="top" width="21%">6</td><td valign="top" width="17%">39%</td></tr><tr><td valign="top" width="20%">JCPenny.com</td><td valign="top" width="20%">3.2</td><td valign="top" width="20%">2.8</td><td valign="top" width="21%">2.3</td><td valign="top" width="17%">18%</td></tr><tr><td valign="top" width="20%">BestBuy.com</td><td valign="top" width="20%">4.4</td><td valign="top" width="20%">4.4</td><td valign="top" width="21%">0.4</td><td valign="top" width="17%">91%</td></tr><tr><td valign="top" width="20%">Sears.com</td><td valign="top" width="20%">6.6</td><td valign="top" width="20%">3.8</td><td valign="top" width="21%">2.7</td><td valign="top" width="17%">29%</td></tr><tr><td valign="top" width="20%">Lowes.com</td><td valign="top" width="20%">13.5</td><td valign="top" width="20%">12.4</td><td valign="top" width="21%">7</td><td valign="top" width="17%">44%</td></tr><tr><td valign="top" width="20%">HomeDepot.com</td><td valign="top" width="20%">9</td><td valign="top" width="20%">7.2</td><td valign="top" width="21%">7</td><td valign="top" width="17%">3%</td></tr><tr><td valign="top" width="20%">Kohls.com</td><td valign="top" width="20%">6.5</td><td valign="top" width="20%">4.8</td><td valign="top" width="21%">1.9</td><td valign="top" width="17%">60%</td></tr><tr><td valign="top" width="20%">Barnes & Noble</td><td valign="top" width="20%">7.8</td><td valign="top" width="20%">5</td><td valign="top" width="21%">3.8</td><td valign="top" width="17%">24%</td></tr></tbody></table>* Measured from the US east coast, running IE8 on a 6.5Mbps download / 384Kbps upload connection.


