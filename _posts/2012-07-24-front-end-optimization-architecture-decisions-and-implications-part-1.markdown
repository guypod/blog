---
layout: post
title: Front-End Optimization Architecture - Decisions and Implications (Part 1)
date: '2012-07-24 16:07:50'
---


**Update:***The follow-up post after reading this one ([Part 2](http://www.guypo.com/front-end-optimization-architecture-decisions-and-implications-part-2/)) is now posted [here](http://www.guypo.com/front-end-optimization-architecture-decisions-and-implications-part-2/).*

In the last Velocity, Pat Meenan gave a presentation reviewing [Front-End Optimization (FEO)](http://velocityconf.com/velocity2012/public/schedule/detail/22973). Pat did a great job discussing when you should use an automated solution, pointing out some of the strengths and weaknesses, and offering his thoughts about cloud-based vs on-premise solutions. One thing Pat didn’t cover is the core architecture of the FEO solution – namely where and how the analysis is performed. Since FEO has been at the center of my life for the past few years, I wanted to share my thoughts on the architecture aspects that matter.


## The Cheat Sheet (a.k.a. TL;DR;)

Throughout this blog post I’ll describe various cases where FEO solutions can take different paths. I’ll describe the path that our team (Blaze before, and Akamai now) chose, and explain why. While I’m clearly biased in favor of the decisions we’ve made, **I hope this post will give you food for thought when considering FEO. Even if you disagree with my views, it will arm you with some questions to ask.**Here’s a concise list of the possible paths. The terms may not make sense to you if you haven’t read the details below yet, but it can be handy in the future.

<table border="1" cellpadding="0" cellspacing="0" width="90%"><tbody><tr bgcolor="#28ACD3"><td width="30%">**Architecture Decision**

</td><td colspan="2" width="70%">**Possible Paths**

</td></tr><tr><td bgcolor="#28ACD3" width="30%">**Analysis Time**

</td><td width="35%">[Inline](#offline)

</td><td width="35%">[Offline](#offline)

</td></tr><tr><td bgcolor="#28ACD3">**Analysis Location**

</td><td>[Local (on proxy)](#a-location)

</td><td>[Central](#a-location)

</td></tr><tr><td bgcolor="#28ACD3">**Optimization Location**

</td><td>[Close to Server](#o-location)

</td><td>[Close to Client](#o-location)

</td></tr><tr><td bgcolor="#28ACD3">**Resource Origin**

</td><td>[Only Local Resources](/front-end-optimization-architecture-decisions-and-implications-part-2/#origin)

</td><td>[All Resources](/front-end-optimization-architecture-decisions-and-implications-part-2/#origin)

</td></tr><tr><td bgcolor="#28ACD3">**Resource Storage**

</td><td>[Local Cache](/front-end-optimization-architecture-decisions-and-implications-part-2/#storage)

</td><td>[Central Persistent Storage](/front-end-optimization-architecture-decisions-and-implications-part-2/#storage)

</td></tr><tr><td bgcolor="#28ACD3">**Data Source**

</td><td>[Real User Traffic](/front-end-optimization-architecture-decisions-and-implications-part-2/#source)

</td><td>[Pulled Content](/front-end-optimization-architecture-decisions-and-implications-part-2/#source)

</td></tr><tr><td bgcolor="#28ACD3">**Upgrade Model**

</td><td>[All-in-one Upgrade](/front-end-optimization-architecture-decisions-and-implications-part-2/#upgrade)

</td><td>[Split Upgrade](/front-end-optimization-architecture-decisions-and-implications-part-2/#upgrade)

</td></tr></tbody></table>This is a long list, and explaining each item in detail will make this post way too long. I therefore decided to split it in two. This post – Part 1 – will deal with the first three items, and the next one (to be posted next week) will cover the rest.


## Offline Analysis vs. Inline Analysis

In order to optimize the page, any FEO solution must first understand it by analyzing it. This analysis includes HTML parsing, JS/CSS analysis, image manipulations and much more. Such analysis can be performed either inline or offline. Inline analysis works on the actual response to a user’s request, delaying the first response(s) to a page. Offline Analysis works in parallel with sending the response back to the user, meaning no response is delayed but there’s a small time window where responses are not optimized. In both cases, the results of the analysis are cached and reused on subsequent requests. And in both cases, the changes to the page itself are done inline, to support dynamic and personalized content. When building our solution, we chose the offline analysis route, because **we believe offline analysis lets us to better understand the page**. To get a complete understanding of a page, the analysis needs to dig deep into the page, and that takes time. At a minimum, fully processing a page could take as long as loading it in a browser. When introducing more logic and manipulations, it can easily take much longer. Offline analysis can accommodate such long analysis, but **Inline analysis must complete very quickly**, since a user is sitting there waiting. This restriction naturally conflicts with truly understanding the page, and without such an understanding, the chances of missing an optimization or breaking the page are much higher.


## Central Analysis vs. Local Analysis (Per-Server)

If you’re using inline analysis, it’s clear you’re performing this analysis locally, on the server delivering the responses. If you’re using offline analysis, you have a choice – analyze locally, or analyze in a central location, and communicate the results to the servers that intercept and modify the pages in realtime. We chose the central analysis option, for two primary reasons. **The first reason is efficiency**. Even at Blaze, we applied our optimizations on multiple servers. At Akamai, we’re extending it to over 100,000 servers. It would be very inefficient to re-analyze each page on every server, instead of analyzing it once and sharing the analysis results. **The second reason is that it lets us make our analysis even deeper**. Using offline analysis gives us more time than inline analysis does, but if it’s done locally it still competes for resources with the actual delivery of the website. Doing things like executing JavaScript on a page or compressing images is resource intensive, and can slow down live traffic. As a result, local analysis – even if done offline – requires a contained and limited analysis, which conflicts with creating the best and safest optimization of it. Performing the analysis on separate, dedicated hardware, lets us lift this restriction, and dig even deeper into the page.


## Optimization Location: Close to Server vs. Close to Client

In addition to where you perform the analysis, there’s a question of where to perform the optimization. All FEO solutions must sit in the critical path between the client and the server, in order to manipulate the pages. However, different solutions sit in different spots along the path, some of which are closer to the server, and some closer to the client. **Simply put, proximity to one end provides a better understanding of it and control over it**. If you’re closer to the server, you can fetch resources from it more quickly, notice changes more quickly, and affect more (or all) of the interactions and requests made to this server. If you’re performing local analysis, optimizing close to the server can also be more efficient, since traffic is likely routed through less servers. Similarly, if you’re closer to the client, you can better understand and interact with the client. You know the latency to the client (and can modify your optimizations accordingly), leverage the protocols being used (e.g. SPDY), and can push data to the client before the page is even generated. Even at Blaze, we preferred to be closer to the client than to the server. While it requires more effort when interacting with the server, client proximity is required for several powerful optimizations. Understanding the client is also the way of the future. The huge differences between various mobile clients and networks means optimization is no longer one-size-fits-all, and must be attuned to the client and network conditions. The acquisition by Akamai made this decision a bit moot, since the vast deployment of Akamai puts us close to both server and client. Still, I would advocate proximity to the client is the more important one, and within Akamai we’re still using that as a guiding principle.


## Interim Summary

As is always the case, building an FEO solution involves more than meets the eye. The architecture decisions outlined above mattered a lot to us, and some of them are the result of trial and error – this isn’t the architecture we originally created… I hope these points will be useful when you consider using an FEO solution, helping you understand a bit more what are the trade-offs and decisions to make. In part 2 of this post, I’ll dig deeper into the remaining four items in the top table, explaining the pros and cons of each, which one we chose, and why.


