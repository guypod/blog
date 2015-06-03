---
layout: post
title: Performance Implications of Responsive Design - Book Contribution
date: '2012-07-11 16:12:45'
---


I had the pleasure of writing a small contribution to[ Tim Kadlec](http://timkadlec.com/)‘s awesome upcoming book “Implementing Responsive Design”. The book is a great pragmatic guide for how to build a responsively designed website, including the basics, advanced use-cases and practical advice on how to overcome the challenges RWD faces today. You can read more about it and pre-order it today [here](http://www.implementingresponsivedesign.com/).

The book includes some great contributions from [top thinkers in the Mobile Web space](http://www.implementingresponsivedesign.com/#contributions), and I was honoured to be included in that list. My contribution is related to my recent “[Performance Implications of Mobile Design](slideshare.net/guypod/performance-implications-of-mobile-design)” presentation from a recent [Breaking Development](http://bdconf.com/) conference, and it’s included below to help you bridge the gap until you buy the the full book!


## Performance Implications of Responsive Design

Responsive Web Design (RWD) tackles many problems, and it’s easy to get lost in questions around how maintainable, future friendly or cool your responsive website will be. In the midst of all of these, it’s important to not lose sight of how **fast** will it be. Performance is a critical component in your user’s experience, and many case studies demonstrate how it affects your users’ satisfaction and your bottom line.

Today, smartphone browsers often get redirected to dedicated mobile websites, known as “mdot sites”, which tend to be significantly lighter in content and visuals than their desktop counterparts. This translates to having fewer images, scripts and stylesheets to download, which helps make those websites faster. The equation is simple – downloading fewer bytes with fewer requests is faster than having more of both.

Responsive websites, however, don’t follow this pattern. I recently ran a performance test on 347 responsive websites (All the websites listed on [http://mediaqueri.es/](http://mediaqueri.es/) in March, 2012). I loaded the homepage of each in a Google Chrome browser window resized to 4 different sizes, ranging from 320×480 to 1600×1200. Each page was loaded multiple times using [webpagetest.org](http://www.webpagetest.org), a web performance measurement tool.

The results were depressing. Despite changing their look across window sizes, the weight and load time of the website hardly changed. 86% of the websites “weighed” roughly the same when loaded in the smallest window, compared to the largest one. In other words, despite the fact the websites *look* like an mdot site when loaded on a small screen, they are still downloading the full website content, and are thus painfully slow.

While every website is different, three causes for this over-downloading repeated across practically all websites:

- Download and Hide
- Download and Shrink
- Excess DOM

**Download and Hide** is by far the top reason for this bloat. Responsive websites usually return a single HTML to any client. Even on “Mobile First” websites, this HTML contains or references all that’s needed to provide the richest experience on the biggest display. On a smaller screen, sections that shouldn’t be shown are hidden using the “display:none” style rule.

Unfortunately, “display:none” doesn’t help performance one bit, and resources referenced in a hidden part of the page are downloaded just the same. Scripts within hidden sections still run. DOM elements are still created. As a result, even when hiding the majority of your page’s content, the browser will still evaluate the page in resources and download all the resources it can find.

**Download and Shrink** is a conceptually similar problem. RWD uses fluid images to better match the different screen sizes. While visually appealing, this means the desktop-grade image is downloaded every time, even when loaded on a much smaller screen. Users cannot appreciate the high quality image on the smaller screen, making the excess bytes a complete waste.

**Excess DOM** is the third episode of the same story. RWD websites return the same HTML to all clients. Browsers parse and process hidden areas of the DOM despite being hidden. As a result, loading a responsive website on a small screen results in a DOM that is far more complicated than what the user experience demands… A more complicated DOM leads to higher memory consumption, expensive reflows, and a generally slower website.

These problems are not simple to solve, since they’re the result of how RWD and browsers work today. However, there are a few practices that can help you keep your performance under control:

- Use Responsive Images
- Build Mobile First
- Measure

**Responsive Images** are already discussed in this book at length, and help address the “Download and Shrink” problem. Since images are the bulk of the bytes on each page, this is the easiest way to significantly reduce your page’s weight. Note that CSS images should be responsive as well, and can be replaced using media queries.

**Build Mobile First **means going a step beyond designing a Mobile First website, and actually coding a dedicated website for the lowest resolution you care about. Once implemented, this website should perform as well as other mdot sites, and be reasonably lightweight. From that point on, only enhance the page using JavaScript or CSS, to avoid over-downloading. Clients that have no JavaScript support will get your basic experience, which should be good enough for these edge cases. Note that enhancing with JavaScript and keeping performance high isn’t simple, and best practices for it are not fully established yet – which brings me to my next point…

**Measure.** Treat performance as a core part of your website’s quality, and don’t ship without understanding and accepting its performance. If you know your mobile website weighs over 1 MB, you’re likely to delay its launch until you do something about it. Measurement tools vary, but I would recommend [Mobitest](http://mobitest.akamai.com/) for testing on real devices and [WebPageTest](http://www.webpagetest.org/) for testing on desktop browsers, resized using the “[setViewPortSize](https://sites.google.com/a/webpagetest.org/docs/using-webpagetest/scripting#TOC-setViewportSize)” script command.

In summary, Responsive Web Design is a powerful and forward thinking technique, but it also carries with it significant performance implications. Make sure you understand these challenges and design to avoid them, so that users won’t abandon your website before they got to experience your amazing visuals and content.


