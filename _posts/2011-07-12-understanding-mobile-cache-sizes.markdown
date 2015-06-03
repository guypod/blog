---
layout: post
title: Understanding Mobile Cache Sizes
date: '2011-07-12 11:09:52'
---


There’s a common understanding across the performance industry that the cache sizes on mobile devices are small. While everybody seems to agree on that fact, we couldn’t find the actual cache size indicated anywhere. So we measured it – and the conclusion is that they’re indeed much too small.

The [detailed methodology](#methodology) is outlined further down, but here’s the summary table on the cache sizes on different devices, including desktop for comparison. Persistent cache means cache that survives a browser process restart, while memory cache is caching within a browsing session:

#### Cache size for desktop browsers (in MB)

<table style="margin-bottom: 6px"><tbody><tr style="background-color: #fe6b00;"><th>Browser</th><th>Chrome</th><th>Firefox</th><th>Opera</th><th>Safari</th><th>IE</th></tr><tr><td>Version</td><td>12</td><td>5.0</td><td>10</td><td>5</td><td>9</td></tr><tr><td>Persistent</td><td>85</td><td>75</td><td>20</td><td>0/100*</td><td>150**</td></tr><tr><td>Memory</td><td>85</td><td>75</td><td>80</td><td>60</td><td>150**</td></tr></tbody></table>* Safari on windows demonstrated no persistent cache, while on Mac it stored 100MB of data

**UPDATE:** We couldn’t reproduce this with the beta version of Safari 5.1, will update the table once its released

** We stopped measuring at 150MB, IE9 can [reportedly go up to 250MB](http://blogs.msdn.com/b/ie/archive/2011/03/17/internet-explorer-9-network-performance-improvements.aspx)

#### Cache size for Mobile Browsers/Devices (in MB)

<table><tbody><tr style="background-color: #fe6b00;"><th>Smartphones</th><th>iPhone 4</th><th>Galaxy S</th><th>Nexus S</th><th>Blackberry Torch</th></tr><tr><td>OS / Ver</td><td>iOS/4.3</td><td>Android 2.2</td><td>Android 2.3</td><td>Blackberry 6</td></tr><tr><td>Persistent*</td><td>0</td><td>4</td><td>4</td><td>25</td></tr><tr><td>Memory**</td><td>100</td><td>4</td><td>4</td><td>25</td></tr></tbody></table><table><tbody><tr style="background-color: #fe6b00;"><th>Tablets</th><th>iPad 1</th><th>iPad 2</th><th>XOOM</th></tr><tr><td>OS / Ver</td><td>iOS/4.3</td><td>iOS/4.3</td><td>Android 3.0</td></tr><tr><td>Persistent*</td><td>0</td><td>0</td><td>20</td></tr><tr><td>Memory**</td><td>20</td><td>50</td><td>20</td></tr></tbody></table>* Persistent cache means after [restarting the process](http://www.technospot.net/blogs/ipad-tip-how-to-close-apps-running-in-background/) or power-cycling the device.

** Memory cache also applies to locking the screen or using other apps, as explained [below](#lockscreen)

### Safari Has no Persistent Cache?!

The finding that surprised us the most is seeing that iOS does not keep any cache after a browser restart. This is also true for Safari on Windows, but was not the case on a Mac, where Safari showed a respectable 100 MB of persistent cache.

Since this finding was so suspicious, we went out of our way to verify it, manually browsing various sites and test-cases, testing from multiple machines and platforms and using network sniffers and dev tools. All tests showed the same behavior – no persistent cache after browser restart.

When we say “Persistent” cache, we mean [after fully restarting the browser process](http://techie-buzz.com/mobile-news/how-to-multitask-in-ios4-on-iphone-ipod-touch.html). On windows such restarts are natural, but on iOS and Mac they are not the default behavior. Therefore, most users are likely affected by the memory cache size more than the persistent cache. Actions such as locking the screen and using other apps impacts the memory size (numbers are inconsistent, as explained [below](#memcache)), but don’t seem to clear it completely.

It’s not clear why Apple chose this behavior. On windows we assume it’s just a bug. On Mobile, it may be that Apple just doesn’t think the scenario was worth investing in. Another theory is the [historical belief](http://www.storagereview.com/demystifying_ssd_endurance) that flash storage will die after too many “writes” to it, meaning Apple is trying to preserve storage life at the expense of caching. Whatever the reason, the behavior seems pretty clear. On Safari – mobile or windows – cache doesn’t survive a process restart (we knew that about [iOS power cycles already](http://www.stevesouders.com/blog/2010/07/12/mobile-cache-file-sizes/)).

### Safari Quirks

The Safari browser displayed a couple of unpredictable behaviors during this research.

The first was the inconsistency between Safari on Windows vs Mac. On Mac, we got the expected decent size cache, but on Windows, we got no persistent cache.  
 Safari seems to store its cache in a single file Sqlite DB called Cache.db, stored in the user profile. This file never grew past about 10MB in our testing, even when the memory cache demonstrated much higher capacities. When opening this file with a SQL editor we could see some of the expected resources, but the browser failed to use them. So while the file gets modified and opened by the Safari process, it doesn’t use it in the expected manner.  
**UPDATE:**This behavior is indeed a bug. It reproduces only in certain scenarios – including a clean install on Windows 7, and we couldn’t reproduce it with the beta of Safari 5.1

The second quirk was an unusual behavior in Safari’s Web Inspector (its Built-In [Developer Tool](http://www.apple.com/safari/features.html#developer)) on a Mac. When loading a page with cacheable resources (e.g. www.apple.com), browsing to another page, and loading the original page again, Web Inspector clearly showed the resources as read from cache. Files took 0 ms to load, and showed to response information.

However, when restarting the browser and going to that page again, Web Inspector showed the resources as though they were fetched from the website again. A closer look (using server logs and network sniffers and inspecting the “Date” header) showed the resources were not really fetched, but were presented as though they were. Seeing the full details of the cached responses can be useful, but we found this more confusing than helpful.  
**UPDATE:**This is a [logged webkit bug](https://bugs.webkit.org/show_bug.cgi?id=45685), and still seems to exist in Safari 5.1.

### Memory Cache – Bigger but Unpredictable

Despite the lack of MobileSafari disk cache, memory cache was far bigger on iOS than on any other mobile platform. Memory cache was much bigger than disk cache for Opera on the desktop as well.

When trying to assess just how big that size is, we saw very inconsistent behaviors. The same device would give widely different results for repeated measurements, and the same iOS on different devices would vary greatly as well.

After more testing, we saw that if we primed the cache with a smaller amount of data, the cache behaved more predictably. For example, if we primed the cache with 10 MB of data, all of it will always be in the cache when checked. However, when priming the cache with 100 pages with 1MB of data each (see methodology for details), we often couldn’t find the last 2 pages we loaded in the cache (nor the first).

The numbers in the table above are the ones we managed to get at least 3 times in a row when only priming that exact amount. The max sizes we got were a bit bigger, but if we browsed more pages than the specified amount, we had no way of predicting which resources (and how many) would stay in the cache.

We can only theorize on why this is so, and our theories are:

1. Opera and the browsers on iOS and XOOM use available memory for memory caching, resulting in unpredictable cache sizes.
2. When the memory is reclaimed or released, it may affect any part of the cache, not in a clean Least-Recently-Used manner.

Note that all of our resources had the same type, size and expiry (just from different domains), so there shouldn’t be any reason for the browser to choose to keep one in the cache over another (besides last usage time). We’d be happy to hear other theories (or better yet, info from the browser teams) explaining this behavior.

### Android Tablet Cache Shows Good Intentions

On Android, both smartphones showed the same extremely small cache size. This is despite the Nexus S running a newer Android version and having slightly better specs.

The Motorola XOOM, however, had a significantly bigger cache size. While still notably smaller than the desktop cache sizes, it seems to show a clear intention to improve the browsing experience. This is consistent with the larger number of connections, allowing the browser to use more resources. Perhaps we’ll see these bigger sizes on smartphones in the next Android version for smartphones.

### Conclusions

Mobile Cache sizes are indeed small. Android’s smartphone cache is barely enough to hold 15 pages together ([average page weights 400KB](http://mobile.httparchive.org/interesting.php#bytesperpage)); iOS’s memory cache is highly unpredictable, and disappears when the browser restarts; Even the top cache sizes on the Blackberry Torch and the XOOM will be cycled through multiple times in an [average user’s usage](http://blog.flurry.com/Portals/41620/images/chart_mobileapp_vs_web_consumption-resized-600.png).

For website owners, the only solution is to use [HTML5 localStorage](http://diveintohtml5.org/storage.html) or make your site an [offline application](http://diveintohtml5.org/offline.html). These solutions are more complicated, but usually grant you 5 MB of dedicated storage, which is all but guaranteed to be there the next time the user browses your site.

For browser manufacturers, this might be an opportunity. I, for one, would be more than happy to sacrifice 500MB of my iPhone’s quota for a faster browsing experience. If one platform gives me that option and another doesn’t, that may be a strong selling point.

### Methodology

[Steve Souders](http://www.stevesouders.com/blog/2010/07/12/mobile-cache-file-sizes/) and [Ryan Grove](http://www.yuiblog.com/blog/2010/07/12/mobile-browser-cache-limits-revisited/) measured how big can a single resource be and still be cached, but not the total cache size on the device. We adopted some of their techniques, and created a new test case made up of two parts – priming the cache, and testing the cache.

Priming the cache works as follows:

1. Take in parameters dictating the number of pages we should test, plus the number and size of resources on each page.
2. Browse the first page on a test domain, where all subdomains lead to the same server (i.e. *.foo.com leads to foo.com). The first page browsed is 1..foo.com/page, using a different random value each time we run a new test, to ensure we don’t get conflicts with past records or cookies.
3. Each page, once fully loaded, loads the next page. For example, 1.rand.foo.com loads 2.rand.foo.com, and so forth, until it reaches the total number of pages (passed in as a query parameter).
4. Each page has X resources of Y KB each (from the input parameters), all of which are cacheable till the same far-future expiry date and served from the current page domain. Each resource sets a random value to cookie “A” when requested (more on why later).
5. By the end of this phase, we’ve loaded and attempted to store into the cache a known amount of pages and data, from multiple different domains. Manual tests showed that the fact we used the same top level domain didn’t matter to cache sizes.

Once the cache was primed, we tested it in the following way:

1. Load the last page for which we primed the cache (e.g. 100.rand.foo.com) with a different query parameter, which repeatedly loads pages backwards toward the first domain (e.g. 1.rand.foo.com).
2. Each time a page is loaded (including the first page), JavaScript checks if the cookie value at the start of the page is the same as it was at the end of the page.
3. Once the cookie value has been set, it implies a resource was fetched from the server and not the cache, meaning we ran out of space on the cache, and stop.

We ran the tests on different devices/browsers, repeating them both with and without restarting the browser after priming the cache. We also repeated the tests multiple times for accuracy, and saw the persistent cache sizes were consistent (memory cache sizes were not, as explained above).

We tried out multiple combinations of number and size of resources, and they all showed the same results. We therefore ran most of our tests were run with 4 resources per page, weighing 256KB each. On Safari we went as low as one 2KB resource when verifying the persistent cache findings, and on Android as low as 4 resources weighing 10KB each. We also tried squeezing more info the cache using multiple top-level domains, but the limits don’t seem to affected by the domain.


