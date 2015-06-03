---
layout: post
title: 'JavaScript Pre-Execution for Mobile: Taking Scripts out of the Loop'
date: '2011-11-09 08:00:10'
---


Today we’re launching a new optimization I’m very excited about – the ability to Pre-Execute JavaScript for Mobile Devices. Understanding JavaScript is critical when attempting to automatically understand web pages. Eliminating JavaScript from pages, to the extent possible,  is a major performance optimization opportunity.

Web Pages today are loaded with JavaScript. More specifically, scripts are used in abundance during page load. This type of usage is bad for performance even on a desktop PC. Browsers can’t anticipate dynamic content, and as a result they can’t optimize it. With no other option they resort to downloading files sequentially instead of in parallel, not performing DNS queries or opening connections ahead of time, etc.

While bad on desktop browser, dynamic content is terrible for Mobile devices. The slow and high-latency network aggravates the pain of not requesting files in parallel. The slow hardware makes even the evaluation and execution time of scripts a significant delay. The mobile-specific software includes optimizations like pipelining and smart connections, which dynamic content doesn’t leverage. All around, using JavaScript during page load is very bad for mobile web performance.


## JavaScript Performance on Mobile and its Page Load Implications

JavaScript performance on Mobile Device is dramatically worse on Mobile, as compared to desktop. The chart below shows the results of running SunSpider JavaScript benchmark on different evolutions of iOS. Using software updates, Apple has taken iPhone from needing 10 seconds to complete the benchmark, to completing it in under 4 seconds. By doubling the phone’s CPU, it improved further to taking roughly 2.2 seconds to complete. Even after these dramatic optimizations, a generic Mac Book Pro finishes the test in 230ms – a tenth of the latest iPhone with the latest iOS. Android numbers are comparable – dramatically slower than laptops, let alone desktops.

[![](http://www.blaze.io/wp-content/uploads/2011/11/sunspider.jpg)](http://www.blaze.io/wp-content/uploads/2011/11/sunspider.jpg)

A common misconception is that this performance only impacts highly sophisticated web applications, such as web-based games or online image editors. This is definitely not the case – the slower hardware impacts practically every web page, and to a significant extent.

To test that, we measured the Top 100 Sites in the US (as defined by Alexa) on iPhone 4, iPhone 4S and an iPhone Simulator running on a recent MacBook Pro. All tests were using iOS 5. To use the simulator, iPhone applications are recompiled for the host’s architecture, so each test was leveraging its local hardware. Lastly, we ran the tests over a fast network and overnight, to reduce any other variability.

The results showed hardware clearly matters. iPhone 4S, which has double the CPU power of iPhone 4 but is otherwise similar, was roughly 15% faster than the iPhone 4. The iOS Simulator, however, was much faster, loading pages 56% faster than iPhone 4, and roughly twice as fast as the iPhone 4S. Most of the measured sites were mobile sites, which tend to be light on JavaScript, we expect the gap to be even greater on the iPad.

![](http://www.blaze.io/wp-content/uploads/2011/11/pageloadtime.png)


## What Is JavaScript Pre-Execution

JavaScript Pre-Execution means exactly that – we execute the JavaScript on the page before the browser does, on its behalf. The browser then gets a mostly static page, and the scripts that must be left dynamic are deferred till after the page load.

At a high level, JavaScript Pre-Execution is made up of these steps:

1. Loading a web page ahead of time in a headless browser (in the relevant context)
2. Executing the JavaScript on it
3. Applying any changes it would make to the HTML content itself
4. Removing code that is fully utilized, and modifying the rest to avoid duplicating the dynamic content
5. Making the remaining code asynchronous

Let’s break down each of these steps.

**Step #1** means loading the page in a headless browser during the Blaze analysis phase. This browser has to be sophisticated enough to mimic different types of browsers, to ensure it processes the page as the specific browser would. The browser is also loaded with the relevant context, to answer JavaScript questions such as the current URL and more.

**Step #2** means loading the page to completion, letting all the JavaScript run, and understand what it did in the process. For instance, capture any content created using document.write(), or understand which information pieces were asked from the browser (e.g. did it inquire about the current user-agent?).

**Step #3** is closely tied to the way Blaze works, and creates Transformation Instructions that will guide the Blaze agents on how to modify the page in real time. The analysis itself is not done every time the page is fetched, but rather every time the page changes substantially. The conclusions drawn from the analysis are applied to every single page view, and are designed to work well with dynamic and personalized content.

**Step #4** uses algorithms and user configuration to decide whether a script was executed to completion, and can be entirely removed from the page. Even if the scripts are left on the page, they are modified to ensure the dynamic content isn’t generated again on the client side.

**Step #5** is the last step, where any remaining scripts are made asynchronous. These scripts will still suffer from the performance challenges the mobile web presents, but at least they won’t slow down the page load. Since all or most of the dynamic content has been generated already, in most cases the user would get a visually complete and usable page before they finish processing.


## Can everything be Pre-Executed?

No, some scripts cannot be pre-executed, but often times at least parts of the script can be processed ahead of time.

For instance, Google Analytics has to run on the actual browser each time. However, the integration script [Google recommends](mailto:http://code.google.com/apis/analytics/docs/tracking/asyncTracking.html) dynamically adds the analytics code to seamlessly handle http and https content with one script. Pre-Executing the script can turn that link into an embedded link, and allow browsers to process it more intelligently.

A similar example is the [Facebook Like button](mailto:http://developers.facebook.com/docs/reference/javascript/), where the init function must remain in JavaScript, but the script itself can be converted into a static link. If the context is accurate enough, even that script can be skipped and conveted into the IFrames it usually generates.

However, some content cannot be pre-executed at all. An example of that is when resources are intentionally added to the page using JavaScript, to influence the way the browser processes them. Some of our own Blaze optimizations fall under that category. Our algorithms attempt to identify such cases automatically, and can be tuned by the website owner where necessary.


## Isn’t Asynchronous JavaScript good enough?

Running scripts asynchronously is a great way to keep the scripts from delaying other parts of the page. However, on many pages – mobile and desktop alike – scripts create content that is necessary for the page to be usable by the user. Running the scripts asynchronously can therefore make the user experience no better, and sometimes worse, than it was before.

Using JavaScript Pre-Execution in combination with Async JS is the ideal solution. Any dynamically generated content – or at least most of it – would become static on the page and be downloaded and displayed quickly. The rest of the scripts, which hopefully are more invisible or handle post-load actions, would be deferred till after the page is loaded.


## The Cherry On Top

While JavaScript Pre-Execution is a great optimization by itself, it also enables a much better optimization for the entire page. Gaining visibility into dynamically generated links, and converting them into static content, allows us to then apply the rest of the Blaze optimizations to those links.

This means we can remove metadata from dynamic image links, and make those images Responsive. We can store CSS files linked to by dynamic content into our HTML5-based Scriptable Cache, and load them asynchronously as well. We can consolidate the dynamically linked JavaScript files with the static ones, and leverage Adaptive Consolidation to ensure they’re never downloaded twice.

There’s not doubt we have not yet tapped into the full opportunity JavaScript Pre-Execution presents. It’s a critical piece in our ability to automatically analyze and understand pages, and we plan to keep making it better and better.

****


