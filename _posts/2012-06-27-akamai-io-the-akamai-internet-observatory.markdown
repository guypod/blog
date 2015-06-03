---
layout: post
title: Akamai IO – The Akamai Internet Observatory
date: '2012-06-27 16:47:21'
---


Possibly the best part of working on Web performance is the community. It always amazes me how users, vendors, and even competitors work together to make the Web faster. Akamai does its best to support this community, through actions like sponsoring [Web](http://www.meetup.com/Web-Performance-Boston/ "Web") [performance](http://www.meetup.com/SF-Web-Performance-Group/ "performance") [meetups](http://www.nywebperformance.org/ "meetups") and open-sourcing tools like [Mobitest](http://www.akamai.com/mobitest "Mobitest").

However, the most valuable resource Akamai has to offer this community arguably isn’t tools – it’s data. That’s the reason I’m really excited to share with you the launch of [Akamai IO – The Akamai Internet Observatory](http://www.akamai.com/io).


## What is Akamai IO

[Akamai IO](http://www.akamai.com/io "Akamai IO") is a portal for sharing continuous data from the traffic Akamai sees with the community. Akamai delivers roughly 2 trillion content requests a day, spanning Web sites from practically every industry and geography. This volume and diversity of data is pretty unique, and is a fairly accurate representation of the entire Web.

It’s important to emphasize that Akamai IO is a source of data, not conclusions. We hope you will dig into that data and reach your own conclusions, helping everybody’s understanding of the Web. We will summarize some of the data in the *[State of the Internet](http://www.akamai.com/stateoftheinternet/)* report, and highlight trends in blog posts, but not within Akamai IO itself.

Lastly, the data in Akamai IO will be continuously updated. The exact frequency will change based on the data, but we hope to show data no more than a couple of days after it occurred. The timing granularity of the data will also vary, but our goal is to aim for daily data or better.

The initial data set used for Akamai IO is based on very small sample of traffic from several hundred Akamai customer Web sites, amounting to roughly 600 million requests per day. While small in Akamai scale, this is definitely a big enough sample to draw conclusions from. Over time we expect to grow that sample to include requests from most Akamai customer Web sites.


## First Step: Browser Market Share

The first type of data we’re sharing in Akamai IO is browser market share. More specifically, the reports include browser family market share, desktop browser version market share, and mobile browser share.

While you can find other statistics about browser market share, there’s a huge difference between the different information sources. The main reason for this variance is most likely differences in the data being sampled. In some cases, such as  a specific website sharing their statistics, it’s easy to understand the difference. In other cases, like other 3rd-party data, it’s harder to know which websites and clients were included in the sampel set. As mentioned before, we feel that Akamai’s perspective is a pretty accurate one, and a useful one to include in Akamai IO.

In addition, you can filter the data to see only cellular or non-cellular network traffic. Akamai maintains a fairly accurate mapping of cellular networks, which enables this rather unique filter. I highly recommend you try it out, as it offers some interesting insights, especially for mobile browser market share.

The actual browser identification is done based on WURFL (big thanks to [ScientiaMobile](http://scientiamobile.com/) for supporting the project), with some Akamai enhancements. Check out the [FAQ](http://www.akamai.com/html/io/io_faq.html) for more information.


## All good things start as Beta

[Akamai IO](http://www.akamai.com/io) is initially marked as Beta, for a variety of reasons.

The first reason is a slight bias in our data set. While the sampled data includes traffic from all over the world, most of the sampled Web sites are oriented towards a US audience. This creates a slight bias in the data, and is also the reason we decided not to split the data by geo. We plan to address this issue in the coming months, and then add geographic filters on the data as well.

The second reason is the processing of the data. We’re very confident we’re identifying the top clients well, but we want to gather more data to ensure full accuracy for the less popular clients too. If you spot any anomaly you think is worth highlighting, please let us know.

The last reason is the presentation of the data – and this is where we really need your feedback. Gathering and processing the data is one thing, but presenting it in a way that is simple and yet flexible is not an easy challenge. We’re starting with basic graphs, and have more controls in the plan, but we’d love to hear how you want to see the data.

To help the conversation, we’ve created a [Google Group for Akamai IO](https://groups.google.com/forum/#!forum/akamai-io), and hope to see some good discussion. If you want to send more private feedback, please email us at [io@akamai.com](mailto:io@akamai.com).


## Early insights

You can’t really create a portal for sharing data without digging into the data yourself… We are, after all, data geeks too!

So here are a few cool screenshots of interesting trends seen in Akamai IO. As a side note, we do plan to support embedding Akamai IO data and linking directly to filtered views – just haven’t implemented that yet…

### Insight: Firefox 13 adoption in action

[![](http://res.cloudinary.com/guypo-blog/image/upload/v1431082706/firefox-insights_ctgsk4.png "firefox-insights")](http://res.cloudinary.com/guypo-blog/image/upload/v1431082706/firefox-insights_ctgsk4.png)

### Insight: IE 8 usage “dips” on weekends, likely implying it’s used most commonly at work places.

[![](http://res.cloudinary.com/guypo-blog/image/upload/v1431082705/ie-insights_wfet6m.png "ie-insights")](http://res.cloudinary.com/guypo-blog/image/upload/v1431082705/ie-insights_wfet6m.png)

 

### Insight: Android leads market share on Cellular networks

[![](http://res.cloudinary.com/guypo-blog/image/upload/v1431082706/mobile-cellular-insights_bswbmy.png "mobile-cellular-insights")](http://res.cloudinary.com/guypo-blog/image/upload/v1431082706/mobile-cellular-insights_bswbmy.png)

 

### Insight: Mobile Safari has a huge lead in non-cellular networks, helped by the iPad

[![](http://res.cloudinary.com/guypo-blog/image/upload/v1431082707/non-cellular-insights_ecpdck.png "non-cellular-insights")](http://res.cloudinary.com/guypo-blog/image/upload/v1431082707/non-cellular-insights_ecpdck.png)

 

 


## Summary

Akamai IO is a truly exciting project for us, and we intend to keep sharing more data and offer better visualizations and interfaces to it. The insights from the Browser Market Share reports are pretty cool, but it’s definitely just our first step.

While we have a lot of ideas on how to enhance Akamai IO, we need your help to make it truly great. Please tell us what YOU want to see in Akamai IO, and how you’d want it presented. The more feedback we get, the better the portal will be. So don’t be shy, and either [join our Google Group](https://groups.google.com/forum/#!forum/akamai-io) or send your thoughts to [io@akamai.com](mailto:io@akamai.com).

 


