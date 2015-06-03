---
layout: post
title: 'UPDATE: Embedded Browser vs. Native Browser'
date: '2011-03-17 15:36:31'
---


Earlier today we released a [massive study comparing the performance of iPhone and Android’s Browsers](http://www.blaze.io/uncategorized/mobile/iphone-vs-android-45000-tests-prove-whose-browser-is-faster/). Our study showed Android’s browser to be 52% faster than iPhone’s. The study stirred a lot of chatter online, as this is a topic close to the heart of many.

To perform the measurements, we made use of purposefully written apps that used each platform’s Embedded Browser (as stated in the initial report). Embedded browsers are software components available to mobile apps to invoke the browser, and are the only ways both platforms allow users to interact with a browser. It’s important to emphasize – we used each platform’s embedded browser, not our own browser.  
  
 Embedded browsers are expected to behave, for the most part, the same as the regular browser. However, Apple is now stating that their embedded browser, called UIWebView, does not share the same optimizations MobileSafari does. In a [quote given to CNET](http://news.cnet.com/8301-30685_3-20044325-264.html?tag=cnetRiver), Apple stated that

> “… they only tested their own proprietary app, which uses an embedded Web viewer that doesn’t actually take advantage of Safari’s Web performance optimizations”.

 The CNET article goes on to state

> “It’s not just JavaScript, though. Safari on iOS 4.3 also has multithreaded, asynchronous page-loading and some HTML5 caching”

Implying the gap between the native browser and the embedded browser is even greater.

To the extent of our knowledge, this is the first time Apple has openly made such a statement. Given the information that various optimizations are not included in the embedded browser, it’s quite possible the iPhone page loads could be faster. We stand behind the statement that Android’s embedded browser is faster than iPhone’s. We hope Apple will help us enable those optimizations and repeat the measurement. Until then, for all we know the missing optimizations may not make a big impact.

Apple’s official response was that Apple

> “regards the tests as flawed because Blaze used its own proprietary application that doesn’t take advantage of Apple Safari browser’s Web-performance optimization”

Stated by [Natalie Kerris](http://topics.bloomberg.com/natalie-kerris/), a spokeswoman for the Cupertino, California-based company.

Lastly, Kerris also noted that

> “Despite this fundamental testing flaw they still only found an average of a second difference in loading Web pages”.

 We see this is a bad interpretation of our results. First and foremost, our tests were run over networks and conditions more favorable than the average user browsing on his mobile device. Second, on many sites the gap was greater in absolute terms (for example, on wsj.com we saw a 5-10 second gap). The median gap was only one second, thanks in part to the great network conditions.


