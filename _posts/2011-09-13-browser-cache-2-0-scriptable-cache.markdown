---
layout: post
title: Browser Cache 2.0 - Scriptable Cache
date: '2011-09-13 13:31:31'
---


![](http://www.guypo.com/wp-content/uploads/2011/09/html5.jpg)Browser cache is one of the anchor tenants of web optimization, and has been around since the early days of the web. Unfortunately, such veteran optimizations often don’t keep up with the times. The last real improvement to browser caching happened in 1999, when the cache-control header was added in HTTP 1.1. One of the many changes in the web since then is the use of JavaScript, moving from rare and minor usage in 1999 to being a dominant technology in web pages today.  
  
 During our constant research of optimization techniques at Blaze, we repeatedly uncovered great optimization techniques that could be implemented if only scripts had access to the cache. Eventually, since browsers don’t provide this capability, we decided to write it ourselves. Leveraging [HTML5 localStorage](http://dev.w3.org/html5/webstorage/), we wrote a script-based caching system, and started building optimizations on top of it.  
**  
 Here are some of the optimizations our scriptable caching platform enables.**

### Persistent Cache

The first optimization is to use HTML5 localStorage for caching. This was a prerequisite to have a scriptable cache, but it also offers a much more persistent cache. While localStorage is limited in size, that data is dedicated to your site, and is not cleared as easily and lightly as the regular shared browser cache. A user can browse a thousand sites, return to your site 3 months later, and chances are the data will still be stored. With the regular shared browser cache, unused files usually get deleted within a few days at most as new content from other domains gets cached. This is especially useful in mobile, where [cache sizes are much smaller](http://mobitest.akamai.com/understanding-mobile-cache-sizes/).

### Caching-Friendly Consolidation

Consolidation and Caching don’t get along. When you consolidate multiple files and cache that file, a single byte changing in any of the original files requires downloading the entire package again.

Using scriptable caching, we created a caching-friendly form of consolidation. When a user browses a page, we’ll check if the files in question are in the cache. If not, we’ll fetch all of them in one request, but then store them individually in the cache. If one of the files changes and the user browses the page again, only the resources that changed will be downloaded – saving bytes and accelerating page loads. The same will happen when the user browses another page on the site, which had similar but not identical resources.

This technique eliminates the conflict between caching and consolidation. Scriptable caching lets you get the best of both worlds, adapting to the situation at hand.

### Prioritized Caching

Most browsers employ a rather simple LRU cache eviction model, clearing the least recently used files when more space is needed. While simple, this is a bad model for most sites. In most pages, for example, images account for the majority of requests and bytes, but byte-for-byte, CSS and JS files impact load time much more. For those sites, it would be better to keep CSS and JS files in the cache at the expense of images. Similarly, caching resources used during page load impacts user-experience more than resources loaded on-load or on-demand. Lastly, on some pages, custom logic can indicate which resources are most impactful to keep in the cache.

The HTML5-based scriptable cache allows us to manage these priorities, and cache certain files instead of others, leaving some resources to be managed by the regular cache.

### And there’s more…

These are but a few of the optimizations a scriptable cache enables. Others include robust [prefetching](http://www.guypo.com/whiteboard-video-pre-fetching-anticipating-the-users-next-click/), consolidation on-demand, fragment caching and many more. A scriptable cache allows modern applications to interact intelligently with the browser, and thus make better decisions.

Surprisingly, there’s been very little talk about a scriptable cache in the standards or communities. Steve Souders [reviewed other cache shortcomings](http://www.stevesouders.com/blog/2010/04/26/call-to-improve-browser-caching/) and recently Mark Nottingham posted a [good article](http://www.mnot.net/blog/2011/08/28/better_browser_caching) about it, but that’s basically it. In my opinion a scriptable cache fits nicely with the changes HTML5 promotes, and should be a part of HTML5, alongside localStorage (and with similar security controls).

Until that happens, building a scriptable cache on top of HTML5 like we did at Blaze is a harder but good alternative. Here at Blaze we already know it was a worthwhile investment, and are excited about all the optimizations it unlocked.


