---
layout: post
title: Google Analytics Site Speed vs. Google Webmaster Tools Site Performance Explained
date: '2012-02-02 15:13:06'
---


![](http://www.blaze.io/wp-content/uploads/2012/02/wmtvsga-sitespeed.png)

Google is probably the biggest evangelist of speeding up the web, and spends millions each year on making the web faster. More specifically, Google offers users insight into their page load times, using two separate “Real User Monitoring” views: “Site Performance” in [Google Webmaster Tools](http://www.google.com/webmastertools) (WMT), and “Site Speed” in [Google Analytics](http://www.google.com/analytics/) (GA).

While both tools serve a similar purpose, the problem is they hardly ever match. The average load times are often dramatically different. On [www.blaze.io](http://www.blaze.io), for example, GA reported load times over 50% longer than what WMT displayed.

In this post I’ll share my understanding of these tools. Note that WMT is not an open-source tool, and so our knowledge of how it works is based on some experimenting, limited information pieces from Google (plus some from Google employees), and some speculation. I try to point out what is fact and what is theory in the sections below.


## Short and Sweet

If you don’t have time to read through, here’s a quick reference table explaining the differences between the two tools:

<table border="1" cellpadding="0" cellspacing="0"><tbody><tr><td valign="top" width="177"></td><th valign="top" width="189">Webmaster Tools</th><th valign="top" width="224">Google Analytics</th></tr><tr><td valign="top" width="177">**What is measured**</td><td colspan="2" valign="top" width="413"> Time from Initial Page Request to “Document Complete”</td></tr><tr><td valign="top" width="177">**How it’s measured**</td><td valign="top" width="189"> Google Toolbar</td><td valign="top" width="224"> Navigation Timing + Google Toolbar on IE</td></tr><tr><td valign="top" width="177">**Browsers measured**</td><td valign="top" width="189"> Firefox 2-4  
 IE 6-9  
 with Google Toolbar & PageRank Enabled</td><td valign="top" width="224"> IE 9+  
 Chrome 6+  
 Firefox 7+  
 Android 4+  
 IE 6-8 with Google Toolbar</td></tr><tr><td valign="top" width="177">**Sampling rate**</td><td valign="top" width="189"> All Clients (assumed)</td><td valign="top" width="224"> 1% of clients, max 10K/day</td></tr><tr><td valign="top" width="177">**Averaging techniques**</td><td valign="top" width="189"> 7-day running **weighted** average (popular pages matter more)</td><td valign="top" width="224"> 30-day simple average (average the individual page’s average load times)</td></tr></tbody></table> 

**Now to the details…**


## What they Measure

Both tools attempt to measure the same thing – the time between making the request (clicking a link, choosing a bookmark, etc) and the browser’s “onload” event, when the browser progress indicators stop. This is the same metric used by webpagetest.org and many others to indicate the page is loaded.

In the past, WMT showed the time it took to Googlebot to get the HTML alone, which is how Matt Cutts explains it in [this 2009 video.](http://www.youtube.com/watch?v=nndi1mxl4vE) The statements in this video are no longer accurate, Google should probably take it down.


## How they Measure

While the two solutions try to measure the same thing, they do it in different ways.

WMT relies [solely on the Google Toolbar](http://www.google.com/support/webmasters/bin/answer.py?hl=en&answer=158541) for measuring page load, getting data from users who also enabled the PageRank feature. There is some speculation that Chrome may have some built in measurements sent to WMT, but I could find no evidence of that.

GA primarily uses the recently standardized [Navigation Timing](https://dvcs.w3.org/hg/webperf/raw-file/tip/specs/NavigationTiming/Overview.html) API to collect load times. More specifically, it uses the formula (loadEventStart –  navigationStart). This is a much better understood and more transparent technique. You can see these timings for your page by submitting it on webpagetest.org using Chrome, IE 9 or Firefox as the browser.

GA also uses the Google Toolbar to capture information from IE 6-8, but not from Firefox 2-4.


## Which Clients they Measure

One impact of the different measurement techniques is that the two products take measurements from very different clients.

The Google Toolbar is only supported on [Firefox 2-4](http://support.google.com/toolbar/bin/answer.py?hl=en&topic=15356&answer=1342452) & [IE 6 or newer](http://support.google.com/toolbar/bin/answer.py?hl=en&answer=9154). It further requires that users enable the PageRank feature, and WMT presumably captures measurements from **all** users that meet those conditions.

GA’s use of Navigation Timings limits it to newer browsers. This means most Chrome versions, Firefox 7+, IE 9+ and Android 4+ ([full list here](http://caniuse.com/nav-timing)). Of those, GA samples 1% of users, to a max of 10K users a day (this sampling rate [can be slightly increased](http://code.google.com/apis/analytics/docs/gaJS/gaJSApiBasicConfiguration.html#_gat.GA_Tracker_._setSiteSpeedSampleRate)). Google also gets measurements from IE 6-8 using the Google Toolbar, it’s unclear whether those fall under the same sampling limitations or not.

This difference means the two tools measure very distinct populations. WMT will get measurements mostly from older browsers, and almost entirely from Windows users. GA will sample a much larger set of browsers, though it will still get very little mobile traffic, and not include Safari.

Curiously, this difference *should* make GA’s numbers smaller (since newer browsers are generally faster), but in reality WMT usually shows a smaller number. Weird.


## How they Average

In GA, you have a lot of ways to slide and dice the data you received. The default view shows the last 30 days and includes all pages, showing a simple average of each page’s average load time.

In WMT, the only number shown is the average, and sometimes a few “sample pages”, chosen based on unknown criteria. There is [some speculation](http://www.google.com/support/forum/p/Webmasters/thread?tid=67196e777882674f&hl=en) this is a 7-day moving average, which makes sense given the lack of controls on choosing a date range (they do provide trend over time). As per [their docs](http://support.google.com/webmasters/bin/answer.py?hl=en&answer=158541) “The site-level “average” load time is traffic-weighted (i.e., more popular pages have a bigger say)”.

The difference in the time window averaged is only a factor when you’ve made some significant performance changes (like rewriting your code, or implementing Blaze). In that case, you won’t see the impact in WMT for a while (GA lets you choose your time frame). If you haven’t made any major changes, sampling 7 days or 30 days should usually give you similar results.

The weighted vs. simple average, however, is very significant. Most sites have many separate pages, but a small number of those get the bulk of the visits. Those pages often get more attention from development, and are often better optimized and thus faster.

For example, assume your home page loads in 1 second and gets 50% of your traffic, while the other 100 pages all load in about 4 seconds. The weighted load time will be 2.5 seconds, while the simple average will be 3.97.

This difference is the best reason I can find for why WMT often shows smaller numbers than GA.


## Which Pages they Measure

GA measures the pages GA was loaded on. This is in your control, and the GA interface lets you easily see and control the pages you’re reviewing.

WMT measures the pages on your site browsed by clients with the Google Toolbar installed and PageRank enabled. It’s hard to say which pages are included in this set, but if your site traffic is big enough, it’s likely to include all pages.


## SEO Implications

A key reason these two Google tools have gotten attention is Google’s statements that Site Speed is now a factor in its ranking algorithms. Here’s an excerpt from [Matt Cutts’ blog](http://www.mattcutts.com/blog/site-speed/) almost 2 years ago:  *“**Google’s webmaster console provides *[*information very close to the information that we’re actually using in our ranking*](http://googlewebmastercentral.blogspot.com/2009/12/your-sites-performance-in-webmaster.html)*.*”

While Google never fully exposes such information, this statement implies the number in Webmaster tools is more likely to be the number to care about in SEO. That said, it’s likely that in the real SEO algorithms, page speed is calculated on a page-by-page basis, and not the site average presented by WMT.


## Recommendations

Given these differences, GA is clearly superior tool for understanding your page load times. It captures data from more browsers; gives you better tools to drill into specific pages, period in time and other filters; and uses a standardized open technique for measuring.

The main downfall of GA Site Speed is the use of a simple (not weighted) average, so make sure you monitor the performance of your key pages separately.

Keep in mind [less than half of the top 10K websites use Google Analytics](http://trends.builtwith.com/analytics). Google has to rely on other tools to feed its search algorithms, and WMT seems closer to how those tools may work. Therefore, you should keep an eye on your WMT numbers.

Personally, I find it hard to believe that Google’s search algorithms don’t include information from newer browsers. The WMT numbers are already outdated, and don’t reflect real user experiences, and the gap is only going to grow. I expect more from Google, especially given their dedication to speed.

As mentioned above, we had to deduce some of this in formation instead of getting the straight details from Google. Do you know anything about this topic we didn’t call out? Do you know any of these are mistakes, or missing data? Please share your thoughts and additions in the comments below.


