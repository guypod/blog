---
layout: post
title: iPhone vs. Android – 45,000 Tests Prove Whose Embedded Browser is Faster
date: '2011-03-17 08:00:14'
---


### iPhone 52% slower than Android, Android speeds past iPhone on 84% of sites<sup>*</sup>

<div style="padding: 10px;padding-bottom: 0px;background-color: #fbf9b5;width: 85%;margin-bottom: 10px;border: 1px solid #a9a9a9">**UPDATE: **Due to differences between the iPhone’s embedded browser vs the iPhones native browser the results may vary. [Read More](http://www.blaze.io/business/embeded-browser-vs-native-browser/)  
**UPDATE:** Adjusted post title adding “Embedded Browser” to reflect recent findings.  
**UPDATE:** Adjusted use of percentages

</div><div style="clear: right;width: 30%;float: right;border: 1px solid #a9a9a9;padding: 10px;margin-left: 10px">**Key Findings:**

- iPhone 52% slower on average
- Android was faster on 84% of the sites
- Android even faster on non-mobile sites
- iPhone 4.3 and Android 2.3 not much faster than previous rev

**Conclusions:**

- Browser vendors optimized for benchmarks, not real sites
- JavaScript Acceleration doesn’t impact most websites
- Android’s edge expected to impact tablets even further

</div>Browser performance is a big deal. Browser speed was a major bullet point – if not the top point – in practically every browser release this last year. In the mobile world, the latest iPhone version (4.3) and Android version (2.3) both focused on their improved JavaScript engine and faster browser. Browser performance is all the rage, and everybody says theirs is faster.

  
 We set out to discover which mobile browser is truly faster – when used on real sites. Our goal was to measure the true mobile browsing experience, and see which device comes out ahead. 45,000 page loads later, this report summarizes our conclusions. We can now give a definitive answer to the question: which browser is really faster, from a user’s point of view?  
 The results surprised us.

First of all, we found that Android’s browser is faster. Not just a little faster, the iPhone was a whopping 52% slower. Android beat iPhone by loading 84% of the websites faster, meaning Safari won the race only 16% of the time. While we expected to see one of the browsers come out on top, we didn’t expect this gap.  
[![](http://www.blaze.io/wp-content/uploads/2011/03/chart_winlossloadtime_v2-300x169.jpg)](http://www.blaze.io/wp-content/uploads/2011/03/chart_winlossloadtime_v2.jpg)  
 Secondly, we saw that despite the optimized JavaScript engines in the latest iPhone & Android versions, browsing speed did not get better. Both Apple and Google tout great performance improvements, but those seem to be reserved to JavaScript benchmarks and high-complexity apps. If you expect pages to show up faster after an upgrade, you’ll be sorely disappointed. Read on to get more info about both findings, as well as additional comparisons such as WiFi vs. 3G and mobile sites vs. regular sites.

### What Makes This Study Unique

Past comparisons usually focused on custom-created benchmarks, such as the SunSpider JavaScript benchmark. While useful, these benchmarks are very different than real world sites, and don’t reflect the actual user experience. This study measured the load time of 1000 real web sites, mimicking the experience users would get when browsing on their smartphones.

Other comparisons attempted to compare a small set of sites manually. However, we can’t draw conclusions from such small sets, and it’s hard to rely on the accuracy of manual measurement. Performance measurement is made up of many variables, and measuring the same page 5 times usually yields 5 different results. In this study, the large number of data points overcomes this variability. Having 9000 measurements on each device has the statistical strength to reliably say which browser is faster, across the different websites.

### A Few Words About Methodology

The study was done primarily on iPhone 4 and Google Nexus S. The websites used were those of the Fortune 1000 companies. Each page was loaded multiple times and on different days, measured primarily over WiFi. For each device, we used the median load time for the comparison. The total number of tests was over 45,000.

This study was made possible through custom apps we developed, used to measure page load time on mobile devices. These apps run on the actual devices, load a page on demand, and measure how long it took. These agents are available as a free service to measure your own site on [Mobitest](http://blaze.io/mobile/).

*Update: While Nexus S and iPhone 4 share similar CPU’s, measurements indicate iPhone 4 is running it at a 770Mhz speed. This was likely done to improve battery life at the expense of performance, and may account for some of the gap.*

For the full details about our methodology, see the “Detailed Methodology” Appendix below.

### iPhone 4.3 vs. Android 2.3

[![](http://www.blaze.io/wp-content/uploads/2011/03/chart_fastestmedloadtime-300x160.jpg)](http://www.blaze.io/wp-content/uploads/2011/03/chart_fastestmedloadtime.jpg)  
 Our primary comparison was, as mentioned, the latest Android vs. the latest iPhone. We wanted to find out whose browser is faster, and got a resounding answer:

Android’s browser is faster. MUCH faster. On average, iPhone 4.3 was 52% slower than Android 2.3, with a median load time of 2.144 seconds vs. iPhone’s median load time of 3.254 seconds. Both median load times are generally fast, but keep in mind the test was done over a fast WiFi connection, and both the devices and network weren’t doing anything else.

Android was faster than iPhone in 84% of the tested websites, and iPhone beat Android in 16% of the races. This demonstrates Android wasn’t just faster overall, but rather provided a faster browsing experience 4 times out of 5.

Android’s edge completely disappeared when looking at mobile specific sites. These are sites that were modified to match the mobile user experience, and tend to be smaller and lighter. On mobile sites, Android was only 3% faster, with a median load time of 2.085 seconds vs. iPhone’s 2.024 – effectively the same. On non-mobile sites, iPhone was 59% slower, with an average load time of 2.180 seconds compared to 3.463 seconds.

Android’s dominance in handling non-mobile sites is especially important when considering tablets. Tablets use the same OS and similar hardware phones do. However, users expect the full experience on tablets, not the simplified mobile sites. This means Android’s edge will make an even greater impact.

### JavaScript Speed Doesn’t Mean Browsing Speed

Comparing the last too OS versions of each platform resulted in one of the most surprising findings of this study. Both Android 2.3 and the recent iPhone 4.3 tout a dramatically improved JavaScript engine compared to their latest versions. Tests performed using the SunSpider JavaScript Benchmark proved that they indeed improved the JavaScript engine significantly, doubling its performance.

Therefore, we naturally assumed that the new versions will show significantly better load times. But we assumed wrong. When comparing iPhone 4.3 to iPhone 4.2 we saw no noticeable improvement, and Android 2.3 was only marginally faster than Android 2.2.

Comparing iPhone 4.3 and 4.2 yielded practically identical results. iPhone 4.3 was faster on 51% of the sites, but was 2% slower on average, with a median load of 3.253 seconds vs. 3.182 seconds on iPhone 4.2 The median gap between the two was exactly zero. We ran 9,000 measurements on each device, and the results were consistent – page loads remained the same.

For Android, since we couldn’t use exactly the same hardware, we compared the Samsung Galaxy S (running Android 2.2) to the Google Nexus S (running Android 2.3). Both devices are recent Samsung devices, and have very similar hardware. Android 2.2 averaged 2.370 seconds, and Android 2.3 averaged 2.144. While that is still a 10% boost, Android 2.3’s JavaScript engine is almost 40% faster than its predecessor. Android 2.2 still won 42% of the races, and the median gap between the two was merely 65 milliseconds. In other words, browsing speed didn’t change much.

**To verify our hardware, we ran SunSpider on these devices, and got the expected (different) results:**

- iPhone 4.3: 3,978.9
- iPhone 4.2: 10,303.9
- Galaxy S (Android 2.2): 5,840.7
- Nexus S (Android 2.3): 4,257.2

Our conclusion is that JavaScript performance doesn’t impact an average page load time. Apparently, JavaScript is already so optimized that it doesn’t play a big role in the time it takes to load a page. It’s likely that rich AJAX applications benefit from these improvements, but users should not expect their casual web surfing to move faster.

A secondary conclusion is that you optimize what you can measure. SunSpider created a predictable test, and Apple and Google did their best to get a better score. Measuring load time of real world sites is harder, in part due to lack of good measuring tools. As a result, the browsers scored higher on the expected test, but showed no progress in a surprise quiz.

### WiFi vs. 3G

If performance is a big deal for browsers, it’s an even bigger deal for cellular providers. Reliability and speed seem to be all cellular providers talk about – besides having the latest phones. Most of our tests were performed over WiFi, to minimize the impact of the network on our results, but we wanted to get a feel for how 3G will impact those results.

To do so, we ran the iPhone 4.2 tests over 3G as well. The results were better than we anticipated. As expected, loading over WiFi was faster in 82% of the cases. However, the gap was surprisingly small – only half a second! The median load time over WiFi was 3.182 seconds, compared to 3.607 over 3G.

It’s worth emphasizing that we tested from a good reception area, and at night. We did so to eliminate noise, to maintain consistent results. The download speed over 3G was 5.95 Mbps (measured using speedtest.net), while in the middle of the day it’s usually about 1Mbps. This test therefore demonstrates the potential performance of browsing over 3G, but more tests should be run under different networks, hours and reception conditions.

### Mobile Sites vs. non-mobile sites

[![](http://www.blaze.io/wp-content/uploads/2011/03/chart_loadtimemobilevsreg-300x170.jpg)](http://www.blaze.io/wp-content/uploads/2011/03/chart_loadtimemobilevsreg.jpg)  
 One way to improve your website’s mobile browsing experience is to create a mobile site. This is a good practice for improving usability, and is usually lighter in resources – and thus faster to load. We were curious to see whether mobile websites indeed provide the expected performance boost.

Out of the 1000 test sites, 175 had a website customized for mobile. On average, mobile websites were loaded in 2.062 seconds, compared to 2.857 – a significant 39% gap. The difference was even greater on iPhone, where mobile sites were 60% faster (2.085 vs. 3.463). On Android, mobile sites were merely 7% faster (2.024 vs 2.180).

In general, smaller sites load much faster. We defined small sites as sites with 30 or less resources (images, scripts, etc.) and big sites as sites with more than 30 resources. On iPhone 4.2, small sites loaded in 2.193 seconds on average compared to 4.412 for big sites – more than twice as fast.

### Summary

This study provided a lot of unexpected insights.  
 We assumed that similar hardware specs and the same WebKit foundation would make iPhone and Android’s browsers perform equally. We assumed that a faster JavaScript engine equals a faster browser. We assumed that 3G would be way slower than WiFi, even under good conditions.

All of these assumptions have been proven wrong when we actually measured those scenarios. Without measuring, you don’t know when and where you need to optimize. The SunSpider JavaScript benchmark pushed browsers to optimize JavaScript performance, because they could measure it. We hope that tools such as [Mobitest](http://blaze.io/mobile/) would eventually result in a similar improvement to the browsing experience.

### Appendix: Detailed Methodology

For test sites, we needed 1000 real websites. We chose to use the Fortune 1000 index as a good indicator for reasonable volume websites.

For clients, we used the latest devices to ensure a fair comparison. We used iPhone 4 for the iPhone measurements, and Google Nexus S for Android 2.3 measurements. For our Android 2.2 measurements we chose the Samsung Galaxy S, since it has almost identical hardware to the Nexus S, making the OS the primary difference. For network, we ran most of our tests over a high speed WiFi router, connected to the internet using a fast DSL connection. The fast internet helped reduce the variability caused by the network, and focus on the browser’s performance. The 3G tests were performed over the Bell Mobility HSPA network, and were made in good reception areas and at low usage hours (mostly at night).

To get the actual numbers, we measured each of the 1000 pages 3 times on each device. From each of the 3 runs, we saved the median scan. We repeated this test 3 times on different days, and filtered out results higher than 40 seconds or lower than 400 milliseconds, as those usually indicated network and server errors. We then chose the median result of the remaining scans on each page and each device. Using medians instead of averages helped us get a representative page load time, without being affected by network anomalies skewing the results.

To summarize, we ran 9000 tests over WiFi on each of these devices: iPhone 4.2, iPhone 4.3, Galaxy S (Android 2.2) and Nexus S (Android 2.3). We also ran 9000 tests on iPhone 4.2 over the Bell 3G network. Lastly, we ran 1000 tests on desktop browsers to separate mobile sites from non-mobile sites. The total number of tests performed for this study was 46,000.

The measurement itself was done using the custom apps, which use the platform’s embedded browser. This means WebView (based on Chrome) for Android, and UIWebView (based on Safari) for iPhone. Manual verification showed that page load performance of the embedded browsers, when properly configured, is effectively identical to the stand-alone browsers. The load times are calculated using the “Document Complete” callback from the browser, which is a standard way of measuring a web page’s load time. As mentioned above, the agents are now a part of a free service available at [http://blaze.io/mobile/](http://blaze.io/mobile/), and we encourage you to try it out.

To distinguish mobile sites from non-mobile sites, we loaded the same 1000 sites through IE8. We then compared the number of resources required to load the page on iPhone as compared to IE8. If the desktop browser required 30 additional resources or more, we flagged the website as mobile. Otherwise, it was flagged as “not mobile”, meaning it does not have a customized version for mobile.

### Disclaimers

We know there’s no such thing as a perfect web page load measurement. The number of variables involved in loading a single page is astonishing, including DNS resolution, packet loss, web server load and many others. While we couldn’t control all the variables, we did try to minimize them where possible: We used high connection speeds and WiFi to reduce the network speed variable; used median load times to filter timeouts; and repeated the test on different days to address web server load. For the rest, we rely on the large number of samples to assure accuracy.

If you’d like access to discuss these results or our conclusions, or want access to the raw data for further analysis, please contact us at [research@blaze.io](mailto:research@blaze.io), and we’ll be happy to share.

<span style="font-size: 9px">*This report is based on our own analysis leveraging the technology and methodology outline in this report. Blaze Software Inc. is in no way affiliated with Google or Apple.</span>


