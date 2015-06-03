---
layout: post
title: SPDY Benchmark - Feedback Highlights
date: '2012-06-20 14:16:06'
---


The [SPDY benchmark I posted last week](http://www.guypo.com/technical/not-as-spdy-as-you-thought/) got some pretty heavy traffic, and I was happy to see it also sparked a lot of conversation and comments. Some of the comments were just trolls looking for attention ([don’t feed them!](http://www.slideshare.net/stubbornella/dont-feed-the-trolls)). Others, however, held good ideas for follow up tests or suggestions for how to address the lack of acceleration.

Below is a collection of the top ideas that came up, and my thoughts on them.


## Theory: SPDY wasn’t faster because websites optimized for HTTP

Many pointed out that websites – and primarily high-traffic websites – are tuned to work around the HTTP limitations, using techniques like domain sharding. Domain sharding is actually harmful for SPDY, resulting in slower performance.

I was aware of this problem and tried to mitigate it by moving resources statically referenced in the page to the page’s domain. However, that mitigation is not perfect, and some sites were probably still using multiple 1<sup>st</sup> party domains.

I think this is a valid concern, but my gut feeling is that it would not change the results. An average page in this test used 18 different domains, and some spot checking (plus past experience) shows only 2-3 of those domains are a 1st party domains.


## Idea: Websites should optimize for SPDY

The idea here is that we can create best practices for making your site fast in a SPDY environment. This will be the modern equivalent to the best practices for HTTP, and will let websites leverage SPDY more.

I believe we indeed can (and should) create such best practices, though I’d expect most of them will be the same ones we use in HTTP. Domain Sharding is probably the biggest exception to that.

However, the main problem with this path is that the same website will be accessed both via SPDY and via plain HTTPS. So even if we manage to develop great best practices, only infrastructures that can serve different HTML based on whether SPDY is in use or not can take advantage of them. The majority of websites don’t have that flexibility.


## Theory: The CDN to Origin communication used HTTP, nullifying SPDY’s value

In my test I used the Cotendo CDN to act as a SPDY-supporting reverse proxy for the test sites. The communication between the Cotendo edge (in NYC) and the origin (wherever it is) was over plain HTTP, without SSL or SPDY. That communication could therefore become the bottleneck, thus leaving little opportunity for acceleration between CDN and client.

I think this is an interesting theory, but I don’t think it holds, for three primary reasons.

First, CDNs like Cotendo and Akamai are designed to accelerate websites. They use many connections to multiplex results to origin, and don’t tend to delay any incoming request. This is the core value proposition of Dynamic Site Acceleration (DSA), and has been proven over and over again.

Second, the average latency and bandwidth between CDN and origin are far better than those between CDN and client. This gap is especially big in the tests run over a high latency mobile network, and even in those conditions SPDY didn’t accelerate much.

Third, there were significant differences in average load time when simulating different connection speeds between client and edge. The average load time for Cable speed was less than half the average load time for the low-latency mobile connection. The CDN to origin path didn’t change between those two cases.

These reasons and more make me think it’s extremely unlikely the CDN’s use of HTTP was the bottleneck in these tests, and that using SPDY to origin would have changed the results.


## Idea: Test the sites while caching all 1<sup>st</sup> party content

One way to test the previous theory is to repeat the test, but this time cache all 1<sup>st</sup> party content on the CDN edge. This means the CDN will have practically no communication to the origin during the test, eliminating the above concern.

Despite disagreeing with the theory above, I think this is an interesting test to run, and hope to get around to doing so. It creates a different type of bias, since it makes resources faster than they would be in the real world. This bias could very well hurt SPDY results, since multiplexing matters less when resources are equally fast. However, it’s probably a worthy thing to test.


## Theory: 3<sup>rd</sup> party domains will fade away as SPDY-first designs rise

I would **LOVE** to see this happen, but I think there’s no chance it will. The use of 3<sup>rd</sup> party domains is already hurting performance, and minimizing use of 3<sup>rd</sup> party domains is already a top performance best practice. And yet, websites keep adding more and more such domains to their pages.

Websites use 3<sup>rd</sup> party domains because they fulfill business needs, and will likely continue to do so for the foreseeable future. In fact, trending shows websites will use **MORE** 3<sup>rd</sup> party domains, not less… So my view is that SPDY (and HTTP/2.0) should be designed to support an eco-system where many domains are used, and not expect the use of 3<sup>rd</sup> party domains to go away.


## Summary

This study isn’t perfect, and there are definitely follow up studies that can be done. Each additional study improves our understanding of SPDY, and in turn helps us make SPDY better. I don’t doubt Google sees SPDY load times being 20%+ faster than HTTP, and the biggest question would be how to replicate such results for other websites, which aren’t as optimized.

Finally, a small clarification: **I am a big fan of SPDY**. I think it challenged conventional thinking, and made us challenge fundamental parts of how HTTP works. I think we should push SPDY along with full force (more accurately, we should standardize its concepts in HTTP/2.0), and alongside that further improve it where it falls short.

 


