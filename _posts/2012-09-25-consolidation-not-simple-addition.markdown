---
layout: post
title: Consolidation - Not Simple Addition
date: '2012-09-25 17:08:52'
---


Consolidation of JavaScript and CSS files is one of the simplest Front-End Optimization techniques. Its goal is to reduce the number of roundtrips to the server, and thus save time and resources. Implementation is simple – Take all the (textual) JS files linked on a page, paste them into one large file, and make your page point to that resource. Repeat for CSS, and e-voila, you have a faster page.

Unfortunately, nothing is really as simple as it sounds… While implementing consolidation is fairly easy, applying consolidation creates two new performance problems: **Duplicate Downloads** and **Breaking Progressive Processing**. This blog post explains these issues, and describes the techniques we use – and you *can *use – to address it in Akamai FEO.  
  
 A short disclaimer – the solutions are not trivial to implement. That said, our experience is that they are definitely worth the effort. Besides, if you like the ideas but don’t want to implement them yourself, you can always use an automated FEO solution…


## Duplicate Downloads

The most well known problem with consolidation is caching. Consider a website with two pages- a home page, and a product page. The home page uses A.JS and B.JS, and the product page uses A.JS and C.JS. A user browsing the two pages on a site, would download each JS file once, requiring three round-trips to fetch them (one for each of A.JS, B.JS and C.JS).

After applying consolidation for each page, the home page uses AB.JS and the product page uses AC.JS. A user that browses both pages will indeed make only two requests for JS files instead of three, but at the price of downloading the content of a.js twice – once with each consolidated “package”. Since a real web page uses many more JS/CSS resources, a real site has many pages, and pages change quite frequently, redundant downloads happen all the time.

The standard technique to mitigate this problem is to only consolidate “common” resources, shared across pages along a common workflow. This is a trade off – consolidate less (i.e. make more requests), in exchange for downloading less duplicate content. However, striking a perfect balance is very hard, and even if achieved, both problems still remain – you still have some duplicate downloads, and still require multiple requests to fetch resources.


## Solution: Adaptive Consolidation

The logical solution to this problem is to split a file into fragments – fetch data into the browser as a single file, but store it in the cache as fragments. Each fragment represents one of the original JS or CSS files (before consolidation). In fact, a fragment would ideally represent a unit of functionality, like a JS module or the styles of a page area.

Unfortunately, browser cache doesn’t work that way, and offers no way to cache a part of a file ([MHTML](http://en.wikipedia.org/wiki/MHTML) was getting close, but [didn’t tackle caching](https://tools.ietf.org/html/draft-ietf-mhtml-info-00#page-6)) or to have a script write to cache. Until that’s resolved, we use localStorage instead.

At a high level, Adaptive Consolidation works as follows:

–       At the top of an HTML, “register” all the JS/CSS fragments this page requires

–       Following that, a script checks which fragments are already in localStorage

–       Those that are not in localStorage are fetched with one request

–       Each fragment is evaluated (e.g. execute script or apply CSS) in order

–       Any fragment that wasn’t in localStorage before is now stored (can be deferred till post onload)

I discussed Adaptive Consolidation in more detail in my “DIY Scriptable Cache” Webinar, you can find the slides [here](http://www.slideshare.net/blazeio/diy-scriptablecachev2) and the recording [here](http://oreillynet.com/pub/e/2200).

This flow is intentionally simplified, and doesn’t fully match our optimization. In addition, there are some complicated parts in the flow, such as managing the stored data as a [scriptable cache](http://www.guypo.com/browser-cache-2-0-scriptable-cache/); evaluating scripts and CSS with JavaScript without breaking functionality; and handling errors when localStorage is full or unavailable.

Despite these caveats, and as mentioned above, this optimization proved to be extremely valuable to us, completely avoiding redundant data while achieving maximum consolidation.


## Breaking Progressive Processing

While it’s well known that Caching and Consolidation don’t get along, few know that consolidation also breaks progressive processing. Let’s go back to the Home Page of the imaginary website from above. This page has two JS files – A.js and B.js – showing up in this order in the HTML. In addition, let’s assume A.js is 1 KB in size, and B.js is 100KBs.

In the unoptimized page, the browser will request both files at once. A.js is small, will likely return faster, and will be executed as soon as it arrives. B.js will be slower, and will execute when its download is complete.

If the resources are consolidated, however, neither will get executed until the combined AB.js file is fully downloaded. To be extra clear here – **browsers do not process partial JS or CSS files**.

Once again, when you consider the large number of JS/CSS files on an average site, this can be a real problem. Beyond the delay in displaying content, scripts and CSS files often lead to more requests (e.g. CSS images, written tags). These requests will start much later, and in turn the user experience will suffer.


## Solution: Streaming Consolidation

As it turns out, there IS one resource that browsers process progressively – HTML. HTML content gets processed as soon as it arrives, which is the foundation for the “[Flush The Buffer Early](http://www.stevesouders.com/blog/2009/05/18/flushing-the-document-early/)” optimization. So in order to fix the problem above, all we need to do is turn our JS/CSS files into HTML!

For instance, “Regular” Adaptive Consolidation can use a script tag like this:

***<script src=”consolidated.js”>***

And consolidated.js will hold this content:

***Notify(“a.js”,”alert(1)”);  
 Notify(“b.js”,”alert(2)”);***

Streaming Consolidation, on the other hand, will use an IFrame like this:

***<iframe src=”consolidated.htm”>***

And consolidated.htm will hold this content:

***  
**<script>parent.Notify(“a.js”,”alert(1)”)</script>  
**<script>parent.Notify(“b.js”,”alert(2)”)</script>  
*****

All in all, it’s pretty easy to convert Adaptive Consolidation into Streaming Consolidation. There is only one catch… it requires Async JS Execution.

The call from the IFrame to the parent is asynchronous, and will therefore not delay other scripts, cannot use document.write(), etc. Implementing Async JS is not a simple feat, so while Streaming Consolidation isn’t hard, it comes with significant pre-requisites.


## Summary

Nothing is ever as simple as it seems… Last year [I wrote about the problems with taking Inlining too far](http://calendar.perfplanet.com/2011/why-inlining-everything-is-not-the-answer/), and Consolidation is not perfect either. This doesn’t mean you shouldn’t consolidate anything, it just means you should proceed with caution.

If you can afford it, implement or buy one of the solutions above. If not, weigh the issues above when deciding what should be merged with what. And most importantly – **measure**. The fact you followed a best practice doesn’t mean your page is faster until the numbers tell you so.

 


