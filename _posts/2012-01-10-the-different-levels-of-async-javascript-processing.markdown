---
layout: post
title: The Different Levels of Async JavaScript Processing
date: '2012-01-10 12:45:07'
---


Async JavaScript Execution is a hot topic. Naïve use of JavaScript causes various problems ranging from blocking other resources to creating availability dependencies on 3<sup>rd</sup> parties. Therefore, advanced websites and sophisticated 3<sup>rd</sup> parties try to load JavaScript asynchronously as much as possible.

However, processing scripts asynchronously isn’t a yes-no decision. When we implemented automated async for the Blaze [Web Performance Optimization](http://www.blaze.io) service, we learned that there are many variations of async to consider. The right approach can vary for different sites, pages, scripts and even devices. In this post, I’ll go over the different levels of Async JS, demonstrate their impact on different pages and discuss how to decide what approach works best for your situation.


## Level 1: No Async JS

The first level is the simplest one – let the browser do its thing. Scripts run inline within the page, block rendering, and block downloading of other resources, namely images.

It’s hard to call this level “Sync JS”, since browsers do perform various look ahead actions, and 3<sup>rd</sup> party components on the page may still set the async flag themselves. However, it’s clearly not “Async JS”, since most scripts on the page block resources in a very non-async manner.

If we look at the [NY Times website](http://www.nytimes.com/), we can see this blocking in action. The next image is a piece of a [visual filmstrip of the page loading](http://www.webpagetest.org/video/compare.php?tests=111216_08_2ad88eb6c729669bfe8ba3fd80a633ab-r:1-c:0). You can see the page is being built step-by-step, each second adding some more content. This is the effect of scripts – in this case 3<sup>rd</sup> party ads – blocking the rendering (and download) of everything that follows.

[![](http://www.blaze.io/wp-content/uploads/2012/01/nytimes-blocking.png)](http://www.blaze.io/technical/the-different-levels-of-async-javascript-processing/attachment/nytimes-blocking/)

 


## Level 2: Async JS

The next level is taking scripts off their pedestal, and running them in parallel to the rest of the resources on the page. This means scripts are loaded in a way similar to images, without blocking other downloads or rendering. We simply call this “async JS”, since scripts are running asynchronously to the rest of the page load.

Since we’re taking away the “high priority” usually given to scripts, scripts have to contend with other resources on the page for bandwidth and CPU. Therefore, when using Async JS scripts may complete a bit later than they would have in Level 1. That said, we’ve observed that on most pages, scripts still complete at around the same time, while the rest of the page renders and completes loading faster.

It’s important to know that performing Async JS also requires some form of script preloading (a.k.a. [in-page prefetching](http://www.blaze.io/technical/whiteboard-video-pre-fetching-anticipating-the-users-next-click/)). Without preloading, any implementation of Async JS will have to download scripts sequentially, just before they’re getting executed. Doing so will not harm the non-scripted content, but it will greatly delay the processing of the scripts themselves. At Blaze we prefer preloading into HTML5 localStorage where possible, but tools like [LABjs](http://labjs.com/), [controljs](http://stevesouders.com/controljs/) and others use other techniques. Preloading usually happens at the very top of the page, to get the data ASAP and start executing it.

To demonstrate the value of Async JS, let’s look at [Macy’s website homepage](http://www.macys.com/). The top bar represents the original site, and the bottom bar shows the site after applying Async JS. Applying async JS to the page made the page start rendering much faster (1.2s vs 2.0s), and the full visual at 2.8 seconds was probably better, but the promotional gray bar was loaded earlier in the original page. This bar is likely generated using JavaScript, and thus async JS may actually delay its loading. In this case the total effect is still positive (which is usually the case, according to my experience), but in some sites it may not be.

[![](http://www.blaze.io/wp-content/uploads/2012/01/nytimes-async.png)](http://www.blaze.io/technical/the-different-levels-of-async-javascript-processing/attachment/nytimes-async/)

 


## Level 3: Async Download, Defer Execution

Now that we’ve separated the loading of scripts from the web page, we can go a step further and separate the preloading of scripts from the execution of them. The next level of Async JS is to preload the scripts as early as possible (i.e. at the top of the page), but to defer the actual execution of the page till after page load.

Doing so pushes the balance between scripts and static content further in favour of the latter. Static content still contends with scripts for bandwidth, but not for CPU. The browser first fully processes the DOM, images and CSS, and only then starts executing the scripts. If your page is usable without JavaScript (as it should be), this technique will improve its perceived load time.

Another advantage of deferring execution is consistent load times. As observed by [Aaron Peters](http://www.aaronpeters.nl/blog/why-loading-third-party-scripts-async-is-not-good-enough) and others, running scripts asynchronously doesn’t always prevent Google Analytics and other RUM tools from counting them in the load times. The same is true for the browsers’ progress indicators (progress bar in chrome, spinning circle in IE, etc). Those may continue until the async scripts are complete.


## Level 4: Defer Download & Execution

The last level of async JS is complete script deferral. This means pushing both the script download and the script execution till after the page load.

This last step means static content on the page doesn’t need to contend with scripts for bandwidth, and thus runs that much faster. The scripts, on the other hand, get delayed further, since they don’t even get downloaded before the page load completes. Therefore, this last step biases the page fully in favour of static content.

Using this technique is most useful in low-bandwidth networks. In these networks download bandwidth is precious, so not contending for it makes a real impact. In addition, full deferral especially helps sites where scripts make up a large portion of the total page size. If scripts make up 5% of the total page size, they wouldn’t take up much of the download bandwidth anyway. If they make up 30% of the total size, it’s a different story.

Deferring scripts is therefore especially useful for Mobile. Mobile networks are slow, and so using the download bandwidth wisely can really help. In addition, Mobile websites are often much lighter, and scripts make up a bigger portion of them. Therefore, we recommend (and default to) using this technique for mobile websites.


## Summary

Scripts may download and run at various levels of priority within a page.

If your page is not usable before scripts run, you probably shouldn’t go beyond Async JS (level 2), if that. You should also consider re-architecting your site, or – if you’re using Blaze – turn on [JavaScript Pre-Execution](../mobile/javascript-pre-execution-for-mobile-taking-scripts-out-of-the-loop/).

If your page is quite usable without any scripts, pat yourself on the shoulder, and proceed to implementing script deferral (level 3 or 4). This will help you capitalize on the better page architecture, and will make your page load time that much faster.


