---
layout: post
title: The Mobile HTTP Archive Is On Its Way
date: '2011-05-13 09:42:02'
---


Just over a month ago, Steve Souders [launched](http://www.stevesouders.com/blog/2011/03/30/announcing-the-http-archive/) a great service called the [HTTP Archive](http://httparchive.org/). The service collects HTTP and performance data from thousands of websites, offering great insights into the state of the web and how it changes over time.

However, the HTTP Archive is limited to desktop browsing, not capturing the mobile web – likely the most dynamic and least understood part of the web. Gaining similar visibility into what the top websites are doing for mobile could help identify problems and let website owners know where they stand. So the same day Steve launched the service, he and I started discussing a mobile version of it.  
  
 Since both Blaze’s [Mobitest](http://mobitest.akamai.com/) and the HTTP Archive are based on [WebPageTest](http://www.webpagetest.org/), we hoped we could build it quickly. About 10 days ago we started to work on it, and within a couple of days we had our initial results. The integration clearly showed the value of standards, primarily the common use of WebPageTest and some heavy use of HAR files.

[![](http://chart.apis.google.com/chart?chs=400x225&cht=p&chco=007099&chd=t:51,145,59,11,0,5&chds=0,145&chdlp=b&chl=HTML+-+51+kB%7CImg-145+kB%7CScripts+-+59+kB%7CStylesheets+-+11+kB%7CFlash+-+0+kB%7COther+-+5+kB&chma=|5)](http://mobile.httparchive.org/)  
 Starting today, the [Mobile HTTP Archive](http://mobile.httparchive.org/) is open to the public. The mobile HA is still in Alpha phase, as we iron out some quirks and expand the data set, but overtime we’ll add more URLs, accumulate more data, and probably add other mobile related data points. Please [let us know](mailto:research@blaze.io) what you’d like added.

### Two First Insights

We’ve only just started mining the data, but here are a couple of initial insights.

**Mobile has many more redirects**  
 When browsing a website’s homepage (e.g. www.cnn.com) on desktop, 81% of websites served an HTML right away. On Mobile, only 53% of websites served content directly! The remaining 47% had at least one redirect, adding to the already slow mobile browsing experience. In total, 37% of the websites landed on a different website when browsed from iPhone compared to being browsed from IE8.

**Mobile pages are smaller, use less domains**  
 No surprise here, but now there’s some stats to prove it. The average page size on mobile was 271K for Mobile vs. 401K for Desktop, and the average number of domains went down from 9.8 on desktop to 6.1 in mobile. As [Steve points out](http://www.stevesouders.com/blog/2011/05/13/http-archive-mobile/), the HTML size actually increased from 24K on desktop to 51K on mobile, so this may be the result of embedding more data.

### Time Will Tell

The Mobile Web is moving fast. Right now we only have a week’s worth of data – enough to make some nice looking charts, but nothing to draw conclusions from. Over time, we’ll be able to see the mobile web in the making, as it becomes a bigger and bigger piece of our lives. I can’t wait to see what we’ll find out.


