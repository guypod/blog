---
layout: post
title: Front-End Optimization Architecture - Decisions and Implications (Part 2)
date: '2012-08-01 14:02:02'
---


Last week I posted the [first part](http://www.guypo.com/front-end-optimization-architecture-decisions-and-implications-part-1/) of reviewing the architecture aspects of Front-End Optimization, and their impact. The purpose of that post (and this one) is to give you better insight into how FEO tools work, so you can make a more informed decision when considering using one.

If you haven’t done so already, check out [last week’s post](http://www.guypo.com/front-end-optimization-architecture-decisions-and-implications-part-1/) to learn about inline vs. offline analysis, central vs. local analysis, and optimizing close to the client vs close to the server. In this post, I’ll discuss the remaining 4 items in the table below.

<table border="1" cellpadding="0" cellspacing="0"><tbody><tr><td width="40%">**Architecture Decision**

</td><td colspan="2" width="60%">**Possible Paths**

</td></tr><tr><td width="40%">**Analysis Time**

</td><td width="30%">[Inline](/front-end-optimization-architecture-decisions-and-implications-part-1/#offline)

</td><td width="30%">[Offline](/front-end-optimization-architecture-decisions-and-implications-part-1/#offline)

</td></tr><tr><td>**Analysis Location**

</td><td>[Local (on proxy)](/front-end-optimization-architecture-decisions-and-implications-part-1/#a-location)

</td><td>[Central](/front-end-optimization-architecture-decisions-and-implications-part-1/#a-location)

</td></tr><tr><td>**Optimization Location**

</td><td>[Close to Server](/front-end-optimization-architecture-decisions-and-implications-part-1/#o-location)

</td><td>[Close to Client](/front-end-optimization-architecture-decisions-and-implications-part-1/#o-location)

</td></tr><tr><td>**Resource Origin**

</td><td>[Only Local Resources](#origin)

</td><td valign="top" width="148">[All Resources](#origin)

</td></tr><tr><td>**Resource Storage**

</td><td>[Local Cache](#storage)

</td><td>[Central Persistent Storage](#storage)

</td></tr><tr><td>**Data Source**

</td><td>[Real User Traffic](#source)

</td><td>[Pulled Content](#source)

</td></tr><tr><td>**Upgrade Model**

</td><td>[All-in-one Upgrade](#upgrade)

</td><td>[Split Upgrade](#upgrade)

</td></tr></tbody></table>
## Resource Origin: Locally Served Resources vs. All Resources

In the last post we’ve discussed where and when to do the analysis, and where to modify the HTML pages. The next question is which resources to include in the analysis and optimization. You can split the resources on the page into two categories: Local Resources, which are delivered by the same system performing FEO; and Remote Resources, which are delivered by a different system, such as 3<sup>rd</sup> party resources, resources put on a separate CDN, etc.

It’s pretty clear you’ll optimize local resources, as those are readily available. It’s not clear, however, whether you’ll optimize remote resources. In order to optimize remote resources you must be able to “see” them during analysis. This requires actively requesting them, unlike local resources, which can be passively “seen” as they get delivered. Such active requests are hard to do if you’re analyzing locally, and are even harder if you’re doing inline analysis.

In our ongoing mission to achieve a perfect understanding of the page, we chose to optimize remote resources as well. Remote resources are a big and growing part of web pages today, and without including them in your analysis, your understanding of the page and its performance is very limited. Optimizing these resources means we need to be more careful not to pull in personalized or highly dynamic 3<sup>rd</sup> party content, but the results are well worth the effort.


## Optimized Resources Storage: Central Storage vs. Local Cache

The decision between central & local analysis also impacts where the optimized resources are stored. If the analysis is done locally, resources are most likely stored in the local server cache. If the analysis is done centrally, resources must be stored in a central location as well, such as Amazon S3 or Akamai NetStorage.

We chose to avoid local cache-based storage because of one word – **Volatility**.

A cache resource, by definition, is not guaranteed to be there when a request arrives. In addition, having the resource stored locally means the page and all its resources must be served by the same server. In a load-balanced environment, such stickiness is harder to achieve and has significant performance implications.

This volatility has a lot of repercussions.

First of all, **every request must include all the data necessary to recreate the resource**. This includes the full URLs of the resources, all the conclusions the analysis reached, and much more. This means requests can get very big, especially consolidation requests, adding significant weight to the downloaded page and the uploaded bytes.

Second, **versioning is no longer guaranteed**. Versioning is the notion of adding a signature (or version number) to a resource, and changing this signature each time the file is changed. This guarantees the same file name will always lead to the same content. If you generate resources on-the-fly, this guarantee is broken. This creates a lot of complications around cache instructions, and can easily lead to the wrong version being cached.

Lastly, **some resources may be broken, as not everything can be created on-demand**. One example is outlining, where a part of the HTML is moved to an external resource. If that resource disappeared, re-generating it requires fetching the original page again, and even then it isn’t guaranteed. The page would be the same. Another example is the case where two resources must be in sync, like a low-res and high-res version of the same image. Each image can be generated on-demand, but they’re no longer guaranteed to be the same. Each such case has a low likelihood of happening, but they’re far from impossible

There are other implications to using local cache, like the fact that delivering a page and resources requires a lot of I/O activity, the fact certain resources may take a long time to return, and the fact the CPU of creating the resources is duplicated. However, the items above are the primary ones.


## Security Model/Data Source: Real Traffic vs. Pulled Content

The discussion above assumes the analysis has access to the HTML page and its resources. This is clearly a requirement, but there are in fact different ways to get this data.

One option is to use real traffic. This means looking at the HTML and resources served to the user, and learning from those. This is probably the easiest way to get access to the website content, and is easier to use when performing inline (or at least local) analysis.

At Akamai we took the other option, which is to explicitly pull the content from the site at analysis time. This is the only way to fetch remote resources, but it can also be used to fetch the HTML and its same-domain resources. This approach is a bit more complex, and may require configuration to access pages behind a login page or in the midst of a step-by-step walkthrough.

This seems like a technical question, but it in fact carries a huge security implication. Every FEO tool creates new resources, which are served to all users. These resources hold content from the original resources, and sometimes even the actual page. **If you use real user traffic when creating these resources, you may leak private information**. This concern is why we didn’t take this path.

Leaking information is not a likely scenario. It’ll require very specific conditions, like an external JS file with the user’s personal information in it. It’s also likely that properly configuring any FEO tool would prevent this problem.

However, if there’s one thing I learned from working on Web Application Security for a decade, is that **if a security problem could happen, it most likely will**. If the security concern is not enough, note that auto-generating resources from real user traffic could cause compliance problems with practically any regulation, ranging from PCI to HIPAA.


## Upgrade Model: All-in-one vs. Analysis-only upgrades

The last item I want to mention is the upgrade model of an FEO solution. The fact we chose the Central Analysis route offered another side benefit – we can upgrade each part of the system independently. We use a backward compatible protocol between our inline agents and our analysis center, and can therefore upgrade each as needed.

From a delivery perspective, this allows us to upgrade the software **as rarely as possible**. The inline code mostly applies HTML-aware search and replace instructions, which is fairly simple code, and doesn’t change often. This means less risk is introduced to the component in charge of delivering your website.

From an analysis perspective, this allows us to upgrade the software **as frequently as possible**. The analysis center is not inline, and if it goes offline briefly, customer websites are not affected. This means we can afford to be more aggressive and deploy new innovative software frequently.

Website owners still need to explicitly enable new optimizations, but this agility makes it easier for us to constantly offer and improve new ways to make their website faster. Between the fast pace of changes in the browser and mobile worlds, and the never-ending innovation cycle, this upgrade model has proven to be invaluable.

Note that if you’re using hosted FEO solution, the upgrade model isn’t really your problem, as it’s a part of the service. However, it’s still worth understanding what happens behind the scenes, and knowing what’s easier or harder for your provider to do.


## Summary

As is always the case, building an FEO solution involves more than meets the eye. The architecture decisions outlined above mattered a lot to us, and some of them are the result of trial and error – this isn’t the architecture we originally created…

In fact, FEO architecture is so complex, I had to split this post into two parts, to make it a bit more digestible… And these are just the summaries of the core decisions, not touching on specific optimizations or implementation details!

I hope these points will be useful when you consider using an FEO solution, helping you understand a bit more what are the trade-offs and decisions to make.

 


