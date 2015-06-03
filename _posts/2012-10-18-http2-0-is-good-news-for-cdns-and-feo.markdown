---
layout: post
title: HTTP/2.0 is good news for CDNs and FEO
date: '2012-10-18 17:07:58'
---


In case you didn’t hear, a new HTTP version is coming to town. There’s a lot of great information about it, including a [recent post](https://blogs.akamai.com/2012/10/http20-what-is-it-and-why-should-you-care.html) by Stephen Ludin and a [recent presentation](http://www.slideshare.net/mnot/what-http20-will-do-for-you) by Mark Nottingham.

HTTP 2.0 is in its infancy, but much of its charter is to implicitly get rid of various performance problems HTTP/1.1 presents. Techniques like header compression and request multiplexing try to make websites inherently faster, with no extra effort required of the website owners.

As a result, I often hear statements like “HTTP/2.0 would get rid of the need for FEO”, or even “HTTP/2.0 would make CDNs unnecessary”. I strongly disagree with these statements, and figured it’s worth writing a post about why.


## True: HTTP/2.0 would make websites implicitly faster

Making websites faster is a part of the goal with HTTP 2. The IETF is intentionally looking for changes that would tackle real world problems, and make websites implicitly faster. If websites are not faster, the IETF would have failed.

Various optimizations have very little downside. Compressing headers, for instance, always reduces the number of bytes sent and received by the client, and is therefore (practically) guaranteed to offer some acceleration. Multiplexing, as another example, would accelerate the communication with every domain that serves multiple resources on the page. This should accelerate at least one domain in practically every web page.

One caveat to this statement is the requirement for TLS. The use of SPDY currently requires TLS (i.e. requires an https site), which comes with some performance implications. For at least some websites, the performance penalty of switching from HTTP to HTTPS outweighs the value if SPDY, making them more secure – but slower. The current HTTP/2.0 plans don’t include mandatory TLS, but if that changes this will be a concern to keep in mind.


## False: HTTP/2.0 would require no changes to websites

Smart websites are quick to adapt to the limitations of the browsers. We change the web page’s HTML to trick the browser into working like we want it to, which often makes pages faster. And then we’re stuck with those tricks long after the browsers are fixed…

There are plenty of examples for this, Here’s one of them: When we saw IE 7 [downloads scripts sequentially](http://www.stevesouders.com/blog/2010/02/07/browser-script-loading-roundup/), we started writing scripts using “document.write()” to make the browser download them in parallel. Many websites still do so today, which in turn makes their sites **slower** on most modern browsers.

Another example is the use of [domain sharding](http://www.stevesouders.com/blog/2009/05/12/sharding-dominant-domains/) – splitting your content across multiple domains, tricking the browser into opening more connections in parallel. Domain sharding was invented to overcome IE 7’s painful limit of 2 connections per host. On modern browsers, which open 6 parallel connections, its usefulness is less clear, and it often ends up slowing down webpages.

Domain sharding and similar techniques cripple the optimizations of HTTP/2.0. Many of the optimizations in SPDY & HTTP/2.0 are domain-specific, and are most efficient if the content is served from a single domain. To take full advantage of HTTP/2.0 websites would need to – at the very least – undo these optimizations. Other optimizations, like server push, would require new techniques and code to make your website as fast as it can be.


## True: HTTP/2.0 client-side adoption will take a while

Sadly, while browsers get updated quickly – many of them fade slowly… Backward compatibility is a very real problem in the web today, and will keep plaguing us long after HTTP/2.0 ships. It took us years to get websites to start dropping support for IE 6, and even today few do so.

On one hand, the fast pace of Chrome and Firefox updates promises browser support for HTTP/2.0 as soon as the draft is finalized, and likely even before. On the other, the slow upgrade process for IE and older Firefox versions guarantees we’ll see browsers that don’t support HTTP/2.0 long after 2014.


## True: Automated FEO can make HTTP/2.0 work best

The transition period to HTTP/2.0 will take a while. It’ll be years before the vast majority (say, 80%) of our clients will support HTTP/2.0. In that interim period, website owners will be in a tough spot. Each protocol version would require different optimization techniques, requiring websites to choose who do they love more – HTTP/1.1 users or HTTP/2.0 users.

Luckily, Automated FEO can help with this problem. Since every Automated FEO tool out there (AFAIK) optimizes per browser, and since that tool is the one applying the dirty tricks to tune for older browsers, it’s easy to **not** apply these techniques for new, HTTP/2.0 clients (assuming the server supports 2.0 as well).

Sticking to the domain sharding example, an FEO tool can easily apply sharding only to 1.1 clients, and skip it – or even undo sharding (consolidating resources into one domain) – for clients using 2.0. In the Akamai FEO tool this is already the default for SSL/TLS connections, and only requires a simple configuration tweak.

We already leverage the fact shared web server software (Apache, Nginx, CDNs) can help the adoption of the HTTP/2.0 protocol itself. Having automated FEO tools can push the 2.0 awareness to the application level, and adjust as necessary.


## False: HTTP/2.0 would remove the need for a CDN

Frankly, I don’t know what makes people think HTTP/2.0 would remove the need for a CDN, but I’ve heard this statement from quite a few people. The short statement here is – no, HTTP/2.0 would NOT take away the need to have a CDN.

It’s true that request multiplexing would mitigate the cost of having high latency to your server, but a 20ms roundtrip time (RTT) would still make your page faster than a 200ms RTT. In addition, CDNs bring to fore a huge set of other values, like offloading, reliability, security and more, which are hardly affected by HTTP/2.0.


## True: HTTP/2.0 opens up new opportunities for FEO & CDNs

Beyond the fact HTTP/2.0 is not harmful to FEO & CDNs, this new protocol actually opens up new opportunities for these products. When HTTP/1.1 was created, CDNs were in their infancy, focusing purely on caching. As websites changed, both CDNs and browsers evolved, but the channel of communication between them hasn’t, leading to many inefficiencies.

HTTP/2.0 is trying to acknowledge and address common problems and opportunities related to CDNs. Techniques such as [browser hints](http://tools.ietf.org/html/draft-nottingham-http-browser-hints-00) and [HTTP-based DNS updates](http://lists.w3.org/Archives/Public/ietf-http-wg/2012JulSep/0285.html) will help websites implicitly get more out of their CDN. Other enhancements, such as server push, open up opportunities for new product offerings CDNs can offer their customers.


## Summary

CDNs ♥ HTTP/2.0.

I’m sure many CDNs feel that way, and I can personally attest to the excitement within Akamai. The new protocol rev is an opportunity to get rid of old problems and offer new products, and we’re eager to help shape it and start using it ASAP.

Some early indications of this excitement are the drafts we submitted to the IETF and having Mark Nottingham – the chair of the IETF group handling HTTP/2.0 – rejoin Akamai, and I’m sure there’ll be many more to follow.

As for automated FEO, HTTP/2.0 is all goodness. It presents no threat to its existence, boosts its current value proposition, and opens up a new world of opportunities. This is partly due to the fact HTTP/2.0 is mostly about fixing HTTP, while FEO probably aligns more with HTML. Maybe I’ll be singing a different tune when HTML 6 comes out 😉


