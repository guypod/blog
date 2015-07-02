---
layout: post
title: Mobile Browser Cache Sizes - Round 2
date: '2012-08-28 17:44:37'
---


Just over a year ago [I posted the results](http://www.guypo.com/understanding-mobile-cache-sizes/) of my research about Mobile Browser cache sizes. At the time, there was general consensus mobile browser cache was small, but a lack of concrete numbers to support it. I created some cookie-based measurement tool, which helped confirm mobile browser cache sizes were indeed far smaller than desktop browsers.

A lot has changed since then. Google shipped Chrome for Android, which [explicitly stated](http://gent.ilcore.com/2012/02/chrome-fast-for-android.html) it addressed the small cache size. Android’s native browser has also improved dramatically, as did Apple’s Mobile Safari. In addition, Firefox released an Android browser, and both Google and Yahoo released hybrid browsers for iOS (Chrome for iOS and Yahoo Axis), which mix Apple’s UIWebView with some of their own software.

With all those changes, plus some probing from Steve Souders, I figured it’s about time to rerun my tests.


## Summary Table

Starting from the bottom line, here is a summary table of the max cache sizes per browser and OS, including the new and old results.


|OS|	Browser|	Max Persistent Cache Size
|:---|:----:|:----:
|iOS 4.3	|Mobile Safari	|0
|iOS 5.1.1	|Mobile Safari	|0
|iOS 5.1.1	|Chrome for IOS	|200 MB+
|Android 2.2	|Android Browser|	4 MB
|Android 2.3	|Android Browser|	4 MB
|Android 3.0	|Android Browser|	20 MB
|Android 4.0 – 4.1	|Chrome for Android|	85 MB
|Android 4.0 – 4.1	|Android Browser|	85 MB
|Android 4.1	|Firefox Beta|	75 MB
|Blackberry OS 6	|Browser|	25 MB
|Blackberry OS 7	|Browser|	85 MB


Note that the tested Android devices and OS versions were: Samsung Galaxy S (2.2), Google Nexus S (2.3), Motorola XOOM (3.0), Samsung Galaxy S 3 (4.0), Google Nexus 7 (4.1), Google Nexus S (4.1).

As you can see, with the exception of Mobile Safari, cache sizes seem to have grown significantly over the last year. Now on to the details…


## Persistent vs Memory Cache

In last year’s tests I attempted to measure both persistent cache and memory cache. Persistent cache meant cache that lives through a restart of the browser process or a power cycle of the device. Memory cache, in contrast, only lives while the browser process lives, and often shrinks if the system requires more memory.

Memory cache proved – then and today – to be extremely unpredictable. Repeated tests showed dramatically different results, with one test often implying a cache size 2-3 times bigger than the other’s. Regardless of whether memory cache is indeed this volatile, or the methodology (explained below) doesn’t measure it well, there’s no point posting these numbers.

For those reasons I decided not to post memory cache sizes this time. Suffice to say I’ve seen cases where memory cache held more than 100 MB of data, and cases where it held less than 10, on the same device. Beyond that, this year’s numbers reflect persistent cache sizes only.


## Android Results

For Android, the new “magic number” for a max cache size seems to be 85 MB. This number is used by Chrome for Android on tablets and phones, and in Android’s default browser on both Ice Cream Sandwich and Jelly Bean phones.

It’s worth noting that Tony Gentilcore [stated](http://gent.ilcore.com/2012/02/chrome-fast-for-android.html) Chrome for Android calculates its cache size based on available disk space, and [Matt Welsh](https://twitter.com/mdwelsh) [mentioned](https://twitter.com/guypod/status/217324361168855040) in his [Velocity 2012 presentation](http://cdn.oreillystatic.com/en/assets/1/event/79/Taming%20the%20Mobile%20Beast%20Presentation.pdf) (Slide #47) Chrome for Android has a cache of 250 MB, though he didn’t state if that was memory or persistent cache.

So while my measurements consistently showed 85 MB of cached data for Chrome (on multiple devices), it’s possible it could go higher.


## iOS Results

The biggest disappointment was Mobile Safari. Unchanged from last year’s tests, Mobile Safari has no persistent cache whatsoever. Whenever browser is restarted or the device power-cycled, all browser cache is cleared.

In fact, in iOS 5.1 Apple has even [made localStorage non-persistent](http://www.sencha.com/blog/html5-scorecard-the-new-ipad-and-ios-5-1/) for apps that use UIWebView (an embedded browser), going against [W3C’s guidelines](http://www.w3.org/TR/webstorage/#dom-localstorage).

However, hope is not lost for iPhone users. In my tests, Chrome for iOS maintained a persistent cache of 200 MB (!!!). It’s possible the cache was even larger, I stopped the test at 200 MB. On a different iOS device, I could never get Chrome to cache more than 35 MB, strengthening the theory that it determines its limits based on available disk space.

To confirm this isn’t just a feature of UIWevView, I tested Yahoo’s Axis pseudo-browser and Twitter’s embedded browser. Both had any persistent cache, making me believe Google wrote some custom code to handle caching – not surprising considering they handle the networking in that browser as well.

Clearly caching is feasible on iOS devices, and it’s possible better caching is one of the reasons [many users feel](http://www.geekfori.com/chrome-for-ios-is-better-than-safari/) Chrome is faster than Mobile Safari. Hopefully Apple learns from this and bumps up its native browser’s cache.


## Other Browsers Results

I also tested two additional browsers – Firefox on Android and the browser Blackberry 9900 (OS 7).

Firefox (Beta) for Android held a 75 MB persistent cache. However, various conversations I’ve had and past browser hacks showed Firefox has a more complicated cache than most. For example, some prefetching techniques showed it holds images and scripts in separate caches. If those caches don’t share a quota, it’s possible the max cache size is bigger.

On Blackberry, the 85 MB magic number surfaced again. This is an improvement from the 25 MB cache on OS 6. That said, it’s not necessarily the future, since Blackberry is now focusing on QNX as their main platform, with a completely new browser.

I did try to test the Playbook’s browser, but my tests indicated it has no memory cache at all. This seems extremely unlikely to me, and it’s easier for me to believe my testing techniques just doesn’t work well on that browser.


## Methodology

The detailed methodology is the same one [I used in my tests last year](http://www.guypo.com/understanding-mobile-cache-sizes/#methodology). In a nutshell, it primes the cache by loading a sequence of pages (each page loads the next one). Each page holds 4 scripts weighing 256 KB each. All the scripts are cacheable, and the HTTP response of the script sets a unique value to the cookie “A”.

Once the cache is primed, the pages load each other in reverse order, and compare the value of “A” at the beginning and end of the page load. If the resources are loaded from cache, nothing changes the cookie value and it remains the same. If at least one of the resources is fetched over the network, the cookie value changes, marking the end of the cache (with a resolution of 1 MB).


## Summary

Mobile browsers evolve quickly, and most seem to have heard the complaints from the web performance community. The average  mobile browser cache size has definitely increased, and hopefully will get even bigger with time. The clear winner is Chrome for iOS, which surprisingly demonstrated a bigger cache than Chrome for Android.

Except for Chrome for iOS, browser cache is still small on mobile compared to desktop.  Desktop browsers claim disk cache sizes such as [250 MB](http://blogs.msdn.com/b/ie/archive/2011/03/17/internet-explorer-9-network-performance-improvements.aspx) for IE 9, 200 MB for Chrome (based on chrome://net-internals#httpCache) and 540 MB for Firefox (based on about:cache). At least Chrome, and perhaps others, calculate max cache size based on available disk space on both Mobile and desktop, which may account for this gap.

There’s definitely reason to be optimistic about mobile browser cache given these improvements, but we can’t let off mobile browser vendors just yet. I’ll be happy when all mobile browsers match Chrome for iOS for cache size, and think we may just get there after all.
