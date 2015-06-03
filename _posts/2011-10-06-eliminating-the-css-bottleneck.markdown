---
layout: post
title: Eliminating the CSS bottleneck
date: '2011-10-06 09:44:40'
---


A big part of accelerating websites is eliminating bottlenecks. Scripts are likely the most discussed bottlenecks, but CSS files are equally bad. CSS files will block all subsequent downloads if there’s a script (internal or external) in between – which is the case on practically any real page.

Most sites are affected by this bottleneck. Even extremely fast sites like webpagetest.org could still benefit from losing this “stair” in the waterfall.  
![](http://www.guypo.com/wp-content/uploads/2011/10/csswaterfall.png)  
 So what can you do about it? You can load CSS asynchronously. Here are the key highlights of what you’ll need to do to achieve that, while maintaining the same site functionality. Alternatively, you can [just use Blaze](http://www.guypo.com), as we’ve been applying this optimization for a while now 😉

I’ll warn up-front that this isn’t trivial to do, but it’s definitely doable if your heart is set on it. Here’s the short list, details further down:

1. Replace link & style tags with a JavaScript array
2. Fetch the data for each CSS
3. Apply CSS to the page
4. Maintain the correct order
5. Avoid FOUC by setting body visibility

**Update:** CSS files consistently block other resources on IE8 and often IE9, but not on Firefox and Opera. For WebKit, they only seem to block resources if they’re in the  of the document (likely a part of the [preloader behavior](http://blog.yoav.ws/archive/2011/09/)). A cool and very simple hack found by [Yoav Weiss](http://blog.yoav.ws/2011/10/Unblocking-blocking-stylesheets) is to add a div (or similar) element above the CSS links, which will make them not block. What it does is make WebKit (or at least Chrome) start the body earlier, and thus stop blocking. It’s super simple to do, just make sure you don’t create any side effects by ending the head early. Here are examples of CSS link [in the head](http://www.guypo.com/experiments/test-css-async.php?head=1), [in the body](http://www.guypo.com/experiments/test-css-async.php) and [in the head with Yoav’s trick](http://www.guypo.com/experiments/test-css-async.php?head=1&unblock=1).

### 1. Replace link & Style Tags with a JavaScript Array

First thing to do is to replace your existing link tags with a JavaScript array holding the different links. If your links use different media rules, you should remember that and store it in the array as well.

It’s important to keep this array in the right order, since you’ll want to apply the CSS rules in the right order. In fact, sometimes your inline styles intentionally attempt to override external stylesheets. If that’s the case, those styles need to be placed in the relevant spot in the array as well.

### 2. Fetch the Data for Each CSS

Once you have the array, you need to fetch the data for each CSS. This can be done by JavaScript in various way, for instance XMLHttpRequest calls (or [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/)). Whatever technique you use, make sure you get notified when the data is available, so you can apply #3.

It’s important to make sure you submit the requests in parallel, and that you start the requests as early as possible in the page.

### 3. Apply CSS to the Page 

Once data is made available, you can apply it to the page (see caveat in #4). Applying CSS rules to the page isn’t always as easy as it sounds. The primary technique is to dynamically create a “style” tag, and populate its text. In IE, you need to change the style tag’s “styleSheet.cssText” attribute. I couldn’t find a good reference on how to do it, but bits and pieces are scattered around the web, if you know of some, please post them in the comments.

### 4. Maintain the Correct Order

Note that you may receive CSS responses in a different order than the order they were requested, so you can’t simply apply the data as you receive it. Each time you get new data, you should first store it in the relevant place in the array. Then traverse the array from the top, apply any element you got data for, and shift that item out of the array. This way you know you’re applying the data in the right order.

### 5. Avoid FOUC by setting body visibility

CSS Link tags, beyond blocking resources, also block rendering. On most sites, that’s a desirable behaviour, since rendering the site without the styling makes it look (and perhaps be) broken. Seeing the unstyled page is referred to as [FOUC – Flash of Unstyled Content](http://bluerobot.com/web/css/fouc.asp/).

To avoid FOUC, you can manipulate the body’s “visibility” style. The initial HTML can have an invisible body (e.g. ), and once all the files are loaded, set the visibility to true.

If you know the app more intimately, you can also choose to set the body as visible before ALL the CSS files are available. This is similar in notion to downloading JavaScript files but not executing them when [running JavaScript asynchronously](http://www.stevesouders.com/blog/2010/12/15/controljs-part-2/).

### Avoiding @import statements

Another scenario in which CSS blocks other resources is when using @import statements to load CSS files. There’s an old but good description of the different cases [here](http://www.stevesouders.com/blog/2009/04/09/dont-use-import/), which is mostly up-to-date. Generally speaking, you should avoid using @import statements, since they make browsers load files sequentially instead of in parallel.

When using async CSS loading, avoiding @import resources is even more important. Depending on how the dynamic CSS is applied, @import statements will keep files from being downloaded in parallel at best, or modify the CSS order at worst.

### The Inevitable Tradeoffs

Almost all performance optimizations come with a slight price. In the case of Async CSS, there are two tradeoffs to consider:

1. Making CSS async may slightly delay the CSS downloads themselves. Since more resources will be downloaded in parallel, the CSS files will need to compete with the others for bandwidth. This is true for any optimization that does more in parallel, and from our experience it’s a non-issue for CSS given their small file size (especially after minification and compression)
2. CSS files are naturally a [Single Point of Failure](http://www.stevesouders.com/blog/2010/06/01/frontend-spof/) since rendering is blocked until all CSS files are downloaded. If a CSS download fails, browsers will still display the other content. When implementing async CSS, you can apply the same behaviour, and can in fact enforce your own timeout.

### Summary

CSS loading is a significant bottleneck, and eliminating it can sometimes shave seconds of the load time. The CSS bottleneck also makes other un-blocking optimizations less useful. If the JavaScript at the top of your page is loaded asynchronously but the CSS files still block, images will still be delayed before they start getting fetched.

Making it not block isn’t trivial, but it’s a worthwhile effort.


