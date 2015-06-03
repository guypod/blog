---
layout: post
title: HTTP Pipelining - Big in Mobile
date: '2011-07-26 12:36:08'
---


HTTP pipelining is a performance optimization that is often overlooked due to its low adoption rate – only the Opera browser uses it by default on desktop .

However, on Mobile HTTP pipelining has a much higher adoption rate – at least the Android and Opera browsers employ it, accounting for [about 40% of mobile browsing](http://gs.statcounter.com/#mobile_browser-ww-monthly-201006-201106)! In this post we’ll explain what is pipelining, how is it used by the browsers, and what you as a web developer should do about it.  
[![](http://upload.wikimedia.org/wikipedia/commons/thumb/1/19/HTTP_pipelining2.svg/640px-HTTP_pipelining2.svg.png)](http://upload.wikimedia.org/wikipedia/commons/thumb/1/19/HTTP_pipelining2.svg/640px-HTTP_pipelining2.svg.png)  
 HTTP pipelining has been around for quite a while – since HTTP/1.1 was introduced. When using pipelining, an HTTP client may send multiple requests on the same connection, without waiting for the server to respond. Doing so practically eliminates the round-trip time (RTT) overhead of all but the first request, especially if the server responds quickly.

HTTP pipelining can certainly accelerate many websites, as [recently demonstrated](http://www.brianp.net/2011/07/19/will-http-pipelining-help-a-study-based-on-the-httparchive-org-data-set/) by Brian Pane. Unfortunately, due to various [issues and challenges](http://www.subbu.org/blog/2011/02/can-pipelining-help) most major desktop browsers chose not to support HTTP pipelining (or not to enable it by default). Only Firefox and Opera support it, and only Opera enables it by default – [less than 2%](http://gs.statcounter.com/#browser-ww-monthly-201006-201106) of total desktop browsing.  
 In Mobile, HTTP pipelining is an especially interesting opportunity for two reasons:

1. **Mobile networks have a much higher latency on average. **  
 This makes the value of avoiding extra RTTs greater
2. **Mobile browsers and websites are newer**  
 As such they tend to get more attention and be more innovative.

And indeed, HTTP pipelining has a much higher adoption amongst mobile browsers. Opera Mini*, Opera Mobile and the Android browser all use HTTP pipelining by default. Together they account for about [40% of mobile browsing](http://gs.statcounter.com/#mobile_browser-ww-monthly-201006-201106). If you’re developing a mobile site, your site is experiencing HTTP pipelining daily, and you should understand how it works.

* Opera Mini uses a proxy to make most requests to the server, and the proxy uses pipelining to make those requests.

### The bottom line

Before we dive into details, here’s the final list of HTTP pipelining support. The different columns are explained in the following sections.

<table style="margin-bottom: 6px;"><tbody><tr><th class="nobg">Windows Desktop Browsers</th><th>Supports Pipelining?</th><th>Server Support Detection</th><th>Max pipelined requests</th><th>Max connections per host</th></tr><tr><td class="spec">IE 9</td><td>No</td><td>–</td><td>–</td><td>6</td></tr><tr class="specalt"><td class="specalt">Chrome 12</td><td>No*</td><td>–</td><td>–</td><td>6</td></tr><tr><td class="spec">Safari 5.1</td><td>No</td><td>–</td><td>–</td><td>6</td></tr><tr class="specalt"><td class="specalt">Opera 11.5</td><td>Yes</td><td>Per Host</td><td>5</td><td>6</td></tr><tr><td class="spec">Firefox 5</td><td>Yes (Off by default)</td><td>Per Connection</td><td>4</td><td>6</td></tr></tbody></table>* Logged as an [enhancement request](http://code.google.com/p/chromium/issues/detail?id=8991) since March, 2009

<table style="margin-bottom: 6px;"><tbody><tr><th class="nobg">Mobile Browsers</th><th>Supports Pipelining?</th><th>Server Support Detection</th><th>Max pipelined requests</th><th>Max connections per host</th></tr><tr><td class="spec">MobileSafari</td><td>No</td><td>–</td><td>–</td><td>6</td></tr><tr class="specalt"><td class="specalt">Blackberry</td><td>No</td><td>–</td><td>–</td><td>5</td></tr><tr><td class="spec">Android</td><td>Yes*</td><td>Per Connection</td><td>3*</td><td>4*</td></tr><tr class="specalt"><td class="specalt">Opera Mobile</td><td>Yes</td><td>Per Host</td><td>11</td><td>4</td></tr><tr><td class="spec">Opera Mini</td><td>Yes**</td><td>Per Host</td><td>4**</td><td>10**</td></tr></tbody></table>* Details are for “stock” Android. Specific devices varied greatly.

** The stats are for the Opera Mini proxy, as the browser makes very few requests itself

### Server Support Detection

The browsers who chose to support HTTP pipelining use heuristics to determine whether the server they’re communicating with supports it as well. The first request to every server is sent by itself (only one request on the connection), and the browser looks for two properties in the response:

1. Use of HTTP/1.1
2. An explicit “Connection: Keep-Alive” header (required by Android)

If those are not present, requests will be sent without HTTP pipelining. This validation is done per host for Opera Mini, Opera Mobile and Opera Desktop, and per connection for Android and Firefox.  
 Support per connection means the first request on a connection is always sent by itself (no pipelining). If the response showed the server supports pipelining, multiple requests may be pipelined on that connection.

<table border="0"><tr><td> Figure 1 shows a network capture of Android Pipelining Connection on www.amazon.com. Note the first request “validates” the connection, followed by piped requests.  
 Support per host means that once the first request showed the server supports pipelining, all future connections will use pipelining immediately. Opera Mini, Opera Mobile and Opera Desktop all use this approach. </td><td style="text-align:center" width="30%">**Figure 1**  
[![](/wp-content/uploads/2013/09/13.HTTP-Pipelining.Part1_.Figure1-thumb.png)](/wp-content/uploads/2013/09/13.HTTP-Pipelining.Part1_.Figure1.png)</td></tr><tr><td> Figure 2 shows a waterfall chart from Opera. Note that all the resources on the page start at once, once support has been established by the fetching of the page itself. </td><td width="30%">**Figure 2**

[![](http://www.guypo.com/wp-content/uploads/2013/09/13.HTTP-Pipelining.Part1_.Figure2-thumb.png)](/wp-content/uploads/2013/09/13.HTTP-Pipelining.Part1_.Figure2.png)

</td></tr></table>### Recovery On “Connection: Close”

When using pipelining, the server may always close the connection before all requests are fulfilled. For example, if requests 1 and 2 are sent on the same connection, the response to request 1 may include a “Connection: close” header. In this case, the browser will honor the close instruction and close the connection, without a response to request 2, and resent request two on a different connection.  
 It’s worth noting that getting such a “Connection: Close” will not make Opera stop using pipelining on that host, even if all future requests come back with such a “close” header.

### Max Pipeline Requests

Since pipelining creates a queue of requests, browsers set a limit on how big the queue can get. This helps mitigate the risk of many requests getting delayed by one slow response (dubbed “[head-of-line blocking](http://en.wikipedia.org/wiki/Head-of-line_blocking)”). All the browsers we tested preferred opening a new connection to pipelining (until reaching their max connections limit). Additional requests will then be piped on existing connections, until all connections reach their max. Remaining requests will wait in the queue.

The algorithms browsers use for distributing pipelined requests will be covered in a subsequent post.

### Android Varies Greatly by Device

Android’s versatility made it very hard to predict whether and how pipelining is used. All the devices that supported pipelining maintained the same logic for determining Server Support, but the number of connections and max pipelined requests were very different. Even pipelining support in general wasn’t constant.

<table><tbody><tr style="background-color: #fe6b00;"><th>Android Device</th><th>OS Version</th><th>Supports Pipelining?</th><th>Max Connections Per Host</th><th>Max Connections</th><th>Max pipelined requests</th></tr><tr><td>Nexus S</td><td>2.3</td><td>Yes</td><td>4</td><td>4</td><td>3</td></tr><tr><td>Galaxy S</td><td>2.2</td><td>Yes</td><td>12</td><td>12</td><td>6</td></tr><tr><td>XOOM</td><td>3.0</td><td>No</td><td>6</td><td>35</td><td>–</td></tr><tr><td>Simulator</td><td>3.1</td><td>Yes</td><td>4</td><td>4</td><td>3</td></tr></tbody></table>The Nexus S and the simulator likely indicate the way Android was meant to behave. However, Motorola opted to modify the XOOM, adding many connections and disabling HTTP pipelining. Samsung took the opposite direction, and while they also increased the number of connections, they doubled the number of pipelined requests. Figure 3 shows Samsung Galaxy S piping 6 requests at once.

**Figure 3**

[![](http://www.guypo.com/wp-content/uploads/2013/09/13.HTTP-Pipelining.Part1_.Figure3-thumb.png)](http://www.guypo.com/wp-content/uploads/2013/09/13.HTTP-Pipelining.Part1_.Figure3.png)

It’s hard to say which values would be ideal, but the variability in Android definitely makes it hard for website owners to optimize for the entire platform. This may be the result of the different hardware specs being tuned for different settings, or just a symptom of the experimentation phase Android seems to be in. We hope that eventually a more consistent behavior will surface.

Seeing as Google is likely the biggest promoter of making the web faster, it’ll be great if they shared the research that made them choose the initial numbers. Doing so may help convince the manufacturers to stick to it, and will help website owners to optimize accordingly. After all, it’ll probably take some time before most of the web switches to [SPDY](http://www.chromium.org/spdy).

### I’m A Mobile Website Developer – What does this mean for me?

If you have a mobile website, HTTP pipelining is a part of your reality – whether you like it or not. You should make sure you support it, and try to tap into the performance opportunity it presents.  
 Here are some specific recommendations:

1. **Make sure your web infrastructure supports HTTP pipelining. **  
 If it’s not extremely old, it probably does.
2. **Use “Connection: Keep-Alive” to leverage pipelining. **  
 Make sure to include an explicit “Connection: Keep-Alive” header for Android.
3. **Serve slow (dynamic) resources from a different domain than fast (static) ones.**  
 With pipelining, multiple requests may get delayed by one slow response.
4. **Use Domain Sharding carefully (or not at all) for browsers that support pipelining**  
 Opening more connections is more costly than pipelining (especially on Mobile), and Android relies on pipelining so much it opens very few connections.
5. **Test your site.**  
 For desktop, download and run Opera and enable Firefox pipelining.  
 For mobile, tools like [http://mobitest.akamai.com/](http://mobitest.akamai.com/), simulators and more can help.


