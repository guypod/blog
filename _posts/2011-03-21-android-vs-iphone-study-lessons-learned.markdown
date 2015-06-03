---
layout: post
title: Android vs. iPhone Study Lessons Learned
date: '2011-03-21 16:14:01'
---


Last week we published a report on Mobile browsing performance differences between iPhone and Android. We knew there would be a lot of interest in the story but were completely blown away by the level of coverage this story got. In some part, the escalated coverage was fueled by the controversy around the results.

 Upon pronouncing Android the clear winner in the race we were immediately set upon by the iPhone community calling the report flawed and misleading. Blaze was accused of being dishonest, biased and other nasty things. In a world where the sighting of a white iPhone is a headline news story, we should have expected a passionate response but the intensity definitely surprised us.

### What did we learn from the experience?

- **Mobile browsing performance is a big deal**. We at Blaze are passionate about performance but we were surprised at how much interest the larger user community showed. The report was the number one tech story of the day and continued to make headlines days after.
- **Measuring performance is not easy**. Building measurement apps and then running the tests over 45,000 times took a lot of effort. Past studies have been too small and used inaccurate, manual measurements. We felt that the scale of our tests, running on real phones on real sites would offer a bullet proof study. By using the embedded browsers we could automate the testing. This allowed for scale and also provided more accurate timing as we could stop the clock at the exact time the browser finished loading the page. While we improved on past studies in these areas, our approach uncovered a new issue. What we didn’t know was that Iphone’s embedded browser, UIWebView, did not include the latest iOS 4.3 optimizations. Therefore, our understanding of iPhone’s browsing speed is still limited to UIWebView’s performance but not Mobile Safari’s.
- **Constantly test your assumptions**. Our key assumption was that embedded browsers were identical to the external ones that users click on. This assumption appears to be correct for Android and perhaps for iOS 4.2, but not for iOS 4.3.
- **It’s important to be open**. By publishing our methodology, readers could quickly determine how we conducted the study and make their own decisions about its accuracy. Conversely, Apple’s lack of information on the differences between its internal and external browser led to lots of needless confusion and speculation.

### What can we take away from the findings of the study?

- **Android’s embedded browser is faster than iPhone’s**. We know that iPhone applications such as Facebook, Twitter and thousands of others that rely on the embedded browser will experience slower performance than their Android counterparts. Since users spend a large percentage of their time in apps vs. Safari, we would argue that our finding is still material to judging the performance of the phones. Furthermore, new reports are suggesting that all Web applications saved to [the home screen will also experience the slower speeds](http://www.theregister.co.uk/2011/03/17/apple_confirms_ios_webview_apps_dont_include_same_optimizations_as_safari/).
- **Our assumption that Android’s embedded browser is the same as the standalone one appears to be correct**. We have it on good authority that there are no material differences between the two. This means that our Android measurements are realistic and accurate. If anyone has information to the contrary, please let us know.
- **iPhone’s embedded browser did not receive the iOS 4.3 improvements that MobileSafari did**. After our study hit the press, Apple publicly disclosed this fact. To our knowledge there was no official statement about this prior to our reports’ publication. There has been much speculation as to why they didn’t update the embedded browser? Conspiracy theorists claim that it’s an economic decision to [penalize app developers that don’t use the app store](http://www.theregister.co.uk/2011/03/17/apple_confirms_ios_webview_apps_dont_include_same_optimizations_as_safari/). Our guess is that in an effort to push iOS 4.3 out the door for the new iPad, they ran out of time and were only able to complete the optimizations for half their browser.

### Conclusions and Next Steps?

Unfortunately we still don’t know which browser is officially faster. We are fairly certain that Android will still win, even with the iOS 4.3 updates. Given the size of the gap, our conclusions on JavaScript performance impact (based on the [Android measurements](http://www.guypo.com/iphone-vs-android-45000-tests-prove-whose-browser-is-faster/)) and the fact that iPhone’s caching improvements wouldn’t impact first view measurements, we can’t see iPhone catching up. Still, we need to rerun our tests to be sure. To do this we either need to find a way to automate Mobile Safari or wait for Apple to update UIWebView. In the case of the latter, we may never know the answer for iOS 4.3, as the next iOS release may introduce further changes.

Another question we would like to answer is how Blackberry, Windows Phone and others compare? We plan to add more devices to our [Mobitest](http://mobitest.akamai.com/) lab in the future to help answer this. We would also like publish information on how site owners can improve their speed on mobile devices.

We will endeavor to answer these and other questions in studies to come. We also look forward to rerunning our tests and learning from our past mistakes. We apologize to all those who felt misled by the study. This was not our intention. Finally, we hope that our study has helped to raise awareness of the issue of Mobile browsing performance. We appreciate all the good discussions and commentary – positive and negative about our study.


