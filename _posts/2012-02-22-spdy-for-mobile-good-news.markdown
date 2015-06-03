---
layout: post
title: SPDY for Mobile – Good News?
date: '2012-02-22 10:18:43'
---


![](http://www.blaze.io/wp-content/uploads/2012/02/chrome_logo.gif)A few weeks ago Google released a [beta version of Chrome for Android](https://market.android.com/details?id=com.android.chrome&hl=en), which is substantially different than the built-in Android browser. It offers some cool new features, which are outlined quite well in the [videos on the Chromium blog](http://blog.chromium.org/2012/02/deeper-look-at-chrome-for-android.html). Both [Sencha](http://www.sencha.com/blog/html5-scorecard-chrome-mobile-beta/) and [Firt](http://www.mobilexweb.com/blog/google-chrome-android-html5) reviewed and had high praise for it’s HTML5 support and more.

Chrome has always focused on speed, and this time is no exception. The new browser includes features like prerendering content while you search, GPU-accelerated rendering, [Chrome Performance Timing API](http://ecmanaut.blogspot.com/2010/06/google-bom-feature-ms-since-pageload.html) and more. More specifically, Chrome for Android seems to support SPDY (see figure #1), making it the first Mobile browser to do so.

[SPDY](http://www.chromium.org/spdy/spdy-whitepaper) is an alternate delivery protocol for HTTP traffic, and even on desktop it’s only currently supported by Chrome (with Firefox support coming soon). It tackles various problems with HTTP, and many of them seem promising for Mobile Web Performance. However, some of them raise some concerns and risks, which may lead to inferior performance instead. In this post I’ll review the primary features of SPDY, and how they relate to Mobile Performance.

A couple of disclaimers before I dig deeper:

1. I love SPDY. I think it’s a creative and powerful solution to many problems. I have a lot of criticism about it, but it’s meant to spark conversation and make it better in the long run, not to make it go away.
2. The concerns below are based on theory, not hard data. In fact, the lack of hard data is one of my rants, as you’ll see through the post.

[![](http://www.blaze.io/wp-content/uploads/2012/02/spdy-graphic-smaller.png)](http://www.blaze.io/?attachment_id=3225)


## Request Multiplexing

Possibly the most important feature of SPDY is request multiplexing. HTTP traditionally sends requests one after the other, waiting for each request’s response before continuing. This behavior introduces significant delays, especially on high-latency networks like mobile networks, as both the server and client spend a long time waiting for data to travel on the network.

HTTP Pipelining helps mitigate this problem, and is indeed [broadly used in Mobile browsers](../mobile/http-pipelining-big-in-mobile/). However, it is still limited in various ways, and introduces concerns such as [head-of-line-blocking](http://en.wikipedia.org/wiki/Head-of-line_blocking).

SPDY offers a better solution, where the client can make multiple requests, and get their responses in any order. This way the client and server waste less time waiting for data, without risking having slow responses delay fast ones. SPDY’s solution is an improved version of HTTP pipelining, and there’s little doubt in my mind it’ll be a great improvement for mobile.


## Request Prioritization

Requesting many resources at once can lead to a big traffic jam on the download pipe. The many requests and many responses will all get downloaded at once, contend with each other for bandwidth, and lead to slower completion times for each individual resource. This gets in the way of “progressive rendering”, where some resources complete quickly, and start showing at least some content to the user.

On mobile networks, bandwidth is very limited, making this concern even greater. It’s easy to see how the bandwidth contention can result in very slow start render times, and an overall worse user-experience.

To address this concern, SPDY supports request prioritization. This feature lets the server decide that one response is more important than another and return its content earlier, or hold back less important data. The client can recommend a priority to the server, helping it make the right call. Good prioritization can maintain progressive rendering, resulting in a better user experience.

Request prioritization can conceptually fix the problem, but a lot would depend on the details of the implementation. Sample questions that come to mind are:

- How would clients decide which resources matter more? Does a faster render time matter more than a fully loaded time? Do ads matter less?
- Even if they know the goal, how would the clients decide which resources are more key to achieving those milestones, before they actually loaded the page?
- How much buffering of low priority responses can/should servers do to make room for high priority responses?

The SPDY specification doesn’t attempt to standardize these decisions, and leaves them to the implementations. While that’s probably a smart move to make the protocol long lived, it means browsers and servers will need to make their own mistakes. In HTTP, similar decisions around connection handling and pipelining took a long time to mature, and it’s not clear they’re very good yet. I suspect SPDY will need to iterate as well, and that bandwidth congestion would still be an issue until the right balance is truck.

On a side note, I think clients (e.g. Chrome) should allow the web page itself to contribute to the prioritization exercise, marking resources with a priority. No one knows which parts of a page matter more than better than the website owner.


## Compress HTTP Headers

SPDY offers a simple but great optimization in compressing HTTP headers. In traditional HTTP, only the request & response body can be compressed, leading to a lot of wasted bytes.

This extra compression will lead to a bit more processing on the client, which may matter more on the weaker mobile devices. That said, I expect the value of the reduced bandwidth to far outweigh the extra CPU cycles.


## One TCP Connection

Since SPDY multiplexes any number of requests on one connection, there’s no obvious need to open an additional connection. Chrome therefore opens only one connection per host, or even [per server IP](https://groups.google.com/forum/#%21msg/spdy-dev/UW0_X2GaMSQ/6sx-_So4aikJ).

On mobile networks, the high latency and weaker CPUs increase the cost of opening another connection. I would therefore expect SPDY’s reduced connection number to help make pages load faster. [Google’s tests](http://www.chromium.org/spdy/spdy-whitepaper#TOC-The-role-of-packet-loss-and-round-t) also indicate SPDY benefits networks with slight packet loss as well, due to the smaller number of packets and SYN packets.

However, mobile networks have a **high** packet loss. Once enough packets are dropped, the entire connection stalls until the sender realizes the packets were lost. By merging all requests into one connection, such a stalled connection delays the entire page, instead of just a part. So while the average case may improve, the edge cases may become much worse.

Note that the SPDY protocol doesn’t *require* one connection, so clients may choose to open multiple connections to counter this problem. Once again, I expect some trial and error would be required.


## Always SSL

SPDY requires SSL to work. While making your site secure is a good idea for the future of the web, it does have performance implications. SSL requires additional CPU for every byte read or written. Sending all the data encrypted will come with time and battery costs, especially for underpowered mobile devices.

SSL’s biggest CPU cost is the handshake when establishing a new SSL session. In theory, SPDY’s efficient use of connection will make this a non-issue. However, in today’s world most web pages have dozens of domains, and SPDY still needs a single connection per domain (except in [rare cases](https://groups.google.com/forum/#%21msg/spdy-dev/UW0_X2GaMSQ/6sx-_So4aikJ)). If all of those sites switched to using SPDY, it could lead to many more handshakes, and be a real battery drain and slow-down factor on mobile devices. Mike Belshe suggested a [future model](http://www.belshe.com/2011/11/17/spdy-of-the-future-might-blow-your-mind-today/) where one connection would serve multiple domains, which may be the way to address this problem.

The SSL overhead means you may need to think carefully about which web applications would benefit from SPDY. If you’re using HTTPS already, SPDY won’t create more overhead, but if you’re porting a non-secure website, there’s a risk it will slow it down considerably. A smart colleague pointed out that even Google doesn’t use SPDY for YouTube, likely because of the SSL overhead. You should think whether your app is more like gmail, or more like YouTube.


## Summary

SPDY is a great protocol. It’s amazing that its authors managed to tackle so many shortcomings in HTTP, without requiring major changes to the web’s infrastructure. Specifically, I expect it to be a boon for the mobile web, and help much more than it hurts.

SPDY is also a NEW protocol, and there are many open questions out there. There aren’t many tools to measure or test it, and even less on mobile. Very little research has been published about it, and more importantly it has little real world experience under it.

I hope to see a lot more research about SPDY, addressing these questions and others. In the meantime, I would definitely advocate using SPDY, but would also suggest keeping a close eye on how it performs, and on the details of its implementation.


