---
layout: post
title: HTTP Pipelining – Request Distribution Algorithms
date: '2011-08-03 11:01:30'
---


This is a follow-up post to the review of [HTTP Pipelining in Mobile](http://www.blaze.io/mobile/http-pipelining-big-in-mobile/).

HTTP pipelining introduces an interesting dilemma to the browsers which support it: How should they distribute the requests across the different connections and pipes?

When a new request needs to be made, the browser has three options:

1. Send the request on a new connection (or an idle connection, not currently in use)
2. Pipe the request on an active connection (and if so, which connection to use?)
3. Delay the request until one or more previous requests complete

Each decision has pros and cons:

- Opening a new connection takes more time and resources, but allows the request to be sent immediately, without delays due to previous requests.
- Piping the request after an existing one is less costly, but if a previous request on the pipe takes long to respond, it will delay the current request as well.
- Delaying the request has the least resource costs, but clearly delays receiving a response.

Our analysis shows browsers were sometimes consistent and sometimes inconsistent in their handling of such cases. The following table summarizes the browser’s behavior (extending the table from the previous post), and each column is explained in the following sections.

<table><tr><th class="nobg">Windows Desktop Browsers</th><th>Supports Pipelining?</th><th>Request Distribution Model</th><th>Max pipelined requests</th><th>Max Connections Per Host</th></tr><tr><td class="spec">IE 9</td><td>No</td><td>–</td><td> –</td><td>6</td></tr><tr class="specalt"><td class="specalt">Chrome 12</td><td>No*</td><td>–</td><td> –</td><td>6</td></tr><tr><td class="spec">Safari 5.1</td><td>No</td><td>–</td><td>–</td><td>6</td></tr><tr class="specalt"><td class="specalt">Opera 11.5</td><td>Yes</td><td>Distribute First</td><td>5</td><td>6</td></tr><tr><td class="spec">Firefox 5 </td><td>Yes (Off by default)</td><td>Fill First</td><td>4</td><td>6</td></tr></table>* Logged as an [enhancement request](http://code.google.com/p/chromium/issues/detail?id=8991) since March, 2009

<table><tr><th class="nobg">Mobile Browsers</th><th>Supports Pipelining?</th><th>Pipeline vs. Connection Distribution</th><th>Max pipelined requests</th><th>Max Connections Per Host</th></tr><tr><td class="spec">MobileSafari</td><td>No</td><td>–</td><td> –</td><td>6</td></tr><tr class="specalt"><td class="specalt">Blackberry</td><td>No</td><td>–</td><td> –</td><td>5</td></tr><tr><td class="spec">Android</td><td>Yes*</td><td>Fill First</td><td>3*</td><td>4</td></tr><tr class="specalt"><td class="specalt">Opera Mobile</td><td>Yes</td><td>Distribute First**</td><td>11**</td><td>2</td></tr><tr><td class="spec">Opera Mini</td><td>Yes***</td><td>Distribute First</td><td>4</td><td>10</td></tr></table>* Details are for “stock” Android. Specific devices vary greatly.

** Opera Mobile behaved unexpectedly, see below for more info.

*** The Opera Mini browser itself doesn’t request most resources, but the Opera proxy uses pipelining when requesting the resources from our server.

### Consistent Behavior: Max Connections and Max Requests Per Pipe

All browsers set a cap on the number of connections per host, and the number of piped requests on a given connection. While the values themselves vary, the algorithm is consistent:

- Once the max connections are opened, future requests will be piped or delayed.
- Once all connections reached the max piped requests, requests will be delayed.

### Consistent Behavior: Prefer a new connection to pipe

All browsers preferred opening a new connection to pipelining. As long as the number of connections per host wasn’t met, a new request will be sent on its own connection. This was true whether the connection has to be opened or was already open and idle.

### Inconsistent Behavior: Piped requests distribution

Once deciding to pipe the request, we’ve observed two approaches, which we dubbed “Fill First” and “Distribute First”. We’ll explain those with an example of a browser that has 2 max connections per host and 3 max piped requests.

“Fill First” means the browser puts as many requests into a pipe as it can on one connection, before starting to use the second pipe. In our example, if the browser piped 4 requests it would send 3 on one connection, and one on the second connection. If it had to pipe 6 requests, it would put requests 1-3 on the first connection, and requests 4-6 on the second.

Figure 1: Fill First

[![](http://www.blaze.io/wp-content/uploads/2011/08/figure1-small.png)](http://www.blaze.io/wp-content/uploads/2011/08/figure1.png)  
 Android and Firefox (with pipelining enabled) use the “Fill First” approach. Figure 1 shows Firefox loading our test page, where each resource takes 1 second to be generated. Firefox first opens a connection per host to verify server support, and then pipes the next consecutive requests on the same connection.

Figure 1 shows Firefox “Fill First” approach

“Distribute First” means the browser distributes the resources evenly across the different connections. In our example, if the browser piped 4 requests it would send two on each connection. If it had to pipe 6 requests, it would put requests 1, 3 and 5 on the first connection, and requests 2, 4 and 6 on the second.

Figure 2: Distribute First

[![](http://www.blaze.io/wp-content/uploads/2011/08/figure2-small.png)](http://www.blaze.io/wp-content/uploads/2011/08/figure2.png)

All the Opera browsers (Mini, Mobile and Desktop) use the “Distribute First” approach. Figure 2 shows Opera loading the same test page. Note that all the requests start at the same time, and return roughly in the order they’re listed on the page.

Figure 2 shows Opera “Distribute First” approach.

While it’s hard to tell which approach will yield the fastest page load, the “Fill First” approach seems less intuitive, since the resources won’t be received by the web server in the order they appear on the page. This statement assumes the server will receive the first request on each connection before receiving piped requests, which is usually the case. If your site is designed to achieve progressive loading, this may impact your results.

### Surprising Behavior: Opera Mobile Pipes on one connection?

Figure 3: Opera Mobile

[![](http://www.blaze.io/wp-content/uploads/2011/08/figure3-small.png)](http://www.blaze.io/wp-content/uploads/2011/08/figure3.png)

We’ve observed a surprising behavior on Opera Mobile. On a page with 40 same-domain resources (and nothing else), Opera Mobile piped 11 (!!!) requests on the first connection, but did no pipelining on the latter. Figure 3 shows the resulting waterfall chart.

Figure 3 shows Opera Mobile Waterfall Chart, browsing a page with 40 same-domain resources.

We couldn’t reproduce this on real world websites, and in fact could barely get Opera Mobile to pipeline more than two requests on such sites. Our assumption is that there are more elaborate heuristics in play, perhaps not tuned for our test scenario where resources took long to create.

### Firefox takes a cautious approach

Firefox took a cautious approach, by supporting HTTP pipelining but turning it off by default. According to [their own support](http://kb.mozillazine.org/Network.http.pipelining) knowledge base: “Users who want better page loading speed can try setting this preference to true, keeping in mind this may break some websites”.

Having it off by default likely means an extremely small number of users actually use it. Given Opera and Android’s active use of it, now would be a good time to turn it on by default.

Figure 4:   
Firefox “about:config” pipelining options

[![](http://www.blaze.io/wp-content/uploads/2011/08/figure4.png)](http://www.blaze.io/wp-content/uploads/2011/08/figure4.png)

### Summary

Despite the fact that HTTP pipelining has been around for a while, there is still significant variability in how browsers implement it (if at all). The variability is both in algorithms (such as request distribution) and in the limits they impose. We tried to summarize the key decisions, but suspect there are more variables in the heuristics the browsers use.

Standardizing these heuristics and decisions can help developers plan better and achieve better results. A draft standard [already exists](http://tools.ietf.org/html/draft-nottingham-http-pipeline-01) to help address known issues in HTTP pipelining, and may be a good home for standardizing distribution and max numbers as well.

Given the broad use of HTTP pipelining in Mobile, we believe it’s time more browsers start supporting it (enabled by default!). [While SPDY is a great protocol](http://www.chromium.org/spdy/spdy-whitepaper), it’ll take years till the majority of sites support it. HTTP pipelining is already supported by most sites today, and we’re not taking full advantage of it.


