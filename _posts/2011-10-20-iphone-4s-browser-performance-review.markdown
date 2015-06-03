---
layout: post
title: iPhone 4S Browser Performance Review
date: '2011-10-20 10:44:23'
---


After [reviewing Apple’s latest software update in iOS 5](http://mobitest.akamai.com/ios5-top10-performance-changes/), we wanted to test the new hardware as well. We got an iPhone 4S as soon as it was released, and ran it through a battery of measurements.

Before digging into the results, a reminder about the hardware differences. In a nutshell, iPhone 4S doubled both the CPU and GPU capacity.  
  
 Here’s a more detailed spec, including the iPad for comparison, [courtesy of Tom’s Hardware](http://www.tomshardware.com/reviews/iphone-4s-a5-performance,3051-2.html):

<table border="0" cellpadding="0" cellspacing="0" style="margin-bottom: 20px"><tbody><tr><th>**Apple**</th><th>**iPad**</th><th>**iPhone 4**</th><th>**iPad 2**</th><th>**iPhone 4S**</th></tr><tr><td valign="top">**SoC**</td><td colspan="2" valign="top">**A4**</td><td colspan="2" valign="top">**A5**</td></tr><tr><td valign="top">**Processor**</td><td valign="top">1 GHz ARM Cortex-A8 (single-core)</td><td valign="top">800 MHz ARM Cortex-A8 (single-core)</td><td valign="top">1 GHz ARM Cortex-A9 (dual-core)</td><td valign="top">800 MHz ARM Cortex-A9 (dual-core)</td></tr><tr><td valign="top">**Memory**</td><td valign="top">256 MB LP-DDR (single-channel)</td><td valign="top">512 MB LP-DDR (single-channel</td><td colspan="2" valign="top">512 MB LP-DDR2 (dual-channel)</td></tr><tr><td valign="top">**Graphics**</td><td colspan="2" valign="top">PowerVR SGX535 (single-core)</td><td colspan="2" valign="top">PowerVR SGX545MP2 (dual-core)</td></tr><tr><td valign="top">**L1 Cache****  
****(Instruction/Data)**</td><td colspan="2" valign="top">32 KB / 32 KB</td><td colspan="2" valign="top">32 KB / 32 KB</td></tr><tr><td valign="top">**L2 Cache**</td><td colspan="2" valign="top">640 KB</td><td colspan="2" valign="top">1 MB</td></tr></tbody></table>### JavaScript Performance

The iPhone 4S showed considerable JavaScript Performance improvements across the board, but most notably within MobileSafari. Here are the results of the SunSpider JavaScript Benchmarks, run in the embedded browser (UIWebView), the native browser (MobileSafari) and a page saved to the home screen (Home-Screen Page). We also noted the delta between the new and old hardware, alongside the cumulative delta with both the new hardware and new software.

<table style="margin-bottom: 20px"><tbody><tr><th>SunSpider JS Benchmark</th><th>iPhone 4  
 iOS 4.3</th><th>iPhone 4  
 iOS 5</th><th>iPhone 4S  
 iOS 5</th></tr><tr><td valign="top" width="168">**UIWebView**</td><td valign="top" width="79">10,044</td><td valign="top" width="120">12,101  
<span style="color: #ff0000">(20% slower)</span></td><td valign="top" width="227">8,955 <span style="color: #008000">(26% faster than iPhone 4, 11% faster in total)</span></td></tr><tr><td valign="top" width="168">**MobileSafari**</td><td valign="top" width="79">4,052</td><td valign="top" width="120">3,574  
<span style="color: #008000">(12% faster)</span></td><td valign="top" width="227">2,215<span style="color: #008000"> (38% faster than iPhone 4, 46% faster in total)</span></td></tr><tr><td valign="top" width="168">**Home-Screen Pages**</td><td valign="top" width="79">10,528</td><td valign="top" width="120">4,551  
<span style="color: #008000">(57% faster)</span></td><td valign="top" width="227">2,227 <span style="color: #008000">(52% faster than iPhone 4, 79% faster in total)</span></td></tr></tbody></table>So the new hardware is clearly faster, and even makes up for the reduced performance in UIWebView. However, the improvements aren’t equal, and the Nitro JS Engine powered browsers (Home Screen Pages and MobileSafari) clearly leverage the new hardware more.

### Rendering Performance

A significant improvement in iOS 5 was adding GPU accelerated rendering for various actions in the browser. This manifested in a 20x improvement over iOS 4 in Microsoft’s Speed Reading test. Combined with the improved GPU on iPhone 4S, iPhone 4S skyrockets to 30x faster than iPhone 4 running iOS 4.3.

Here are the results, measured in Frames Per Second (FPS):

<table border="1" cellpadding="0" cellspacing="0" style="margin-bottom: 20px"><tbody><tr><th valign="top" width="148">Device / OS</th><th valign="top" width="148">FPS</th></tr><tr><td valign="top" width="148">**iPhone 4  iOS 4.3**</td><td valign="top" width="148">2 FPS</td></tr><tr><td valign="top" width="148">**iPhone 4  iOS 5**</td><td valign="top" width="148">40 FPS</td></tr><tr><td valign="top" width="148">**iPhone 4S iOS 5**</td><td valign="top" width="148">60 FPS</td></tr></tbody></table>So the new graphics engines that iPhone gamers would enjoy would also manifest in browsing performance, at least for rich HTML5 applications. These results were consistent across the different browser modes (UIWebView, Home Screen Pages and MobileSafari).

### Page Load Times

After all the benchmarks, we also measured page load time of the Alexa Top 500 Sites in the US on both phones. iPhone 4S showed significant improvements here as well.

These measurements were performed using [Mobitest](http://mobitest.akamai.com/), which uses UIWebView, and were performed over a fast WiFi network overnight. The goal was to measure performance differences, mitigating network effects as much as possible. Each page was measured 3 times on each device, and the suite of tests was run twice, adding up to 6,000 measurements in total.

On average, iPhone 4S was 13% faster in loading pages, compared to the iPhone 4 (both running iOS 5). This delta was very consistent across the two sets of tests, but the iPhone 4S was faster only on 60% of the pages. This is likely because the performance improvements would only manifest on sites built in a certain way, for instance heavy JavaScript sites or sites rich with images.

### Summary

The iPhone 4S is a good upgrade from a browsing performance perspective. Combined with iOS 5, it offers a significant performance boost, especially for rich web applications. The new software seems to leverage the new hardware well, as we can see in the GPU accelerated rendering and the significant JavaScript boost the Nitro engine gets.


