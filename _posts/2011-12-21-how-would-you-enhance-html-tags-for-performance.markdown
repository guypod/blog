---
layout: post
title: How Would You Enhance HTML Tags For Performance?
date: '2011-12-21 12:15:32'
---

A little while ago [Jason Grigsby](http://twitter.com/#%21/grigs) wrote a [great post](http://www.cloudfour.com/responsive-imgs-part-3-future-of-the-img-tag/) outlining his “wish list” for how the &lt;img&gt; tag should change to simplify mobile design (and specifically Responsive Images). The post created a lot of good conversation, and hopefully some standards activity too. At the end of the day, browser makers and developers share a common goal of making the web better, so this type of feedback helps.

Ever since I read that post I’ve been thinking about my own wish list, focused on performance this time. How would I change HTML to make performance optimization easier? As it turns out, my wish list was pretty long… Below are three of my top items – add yours in the comments below!


## &lt;img&gt; tag: Responsive & On-Demand Images Support

Two key optimizations for images are [Responsive Images](http://www.cloudfour.com/responsive-imgs-part-2/) (loading a small image for a smaller screen) and [Images On-Demand](../the-impact-of-image-optimization/) (“lazy loading” images, requesting them only if they’re in the visibile area). Grigs already covered Responsive Images well, so I’ll focus on the latter.

I’d like the &lt;img&gt; tag to allow an optional “lazyload” attribute. If present and set to true, the browser will only request the image if it’s in the visible area, whether it’s “above the fold” or the user scrolled or resized the window and it came into view.

The default behaviour should be “false” (load images immediately), for backward compatibility, but the  tag should be able to override that default. If it holds a *lazyload-images=”true”* attribute, images on the page would be lazy-loaded by default, and the website would have to explicitly exclude beacons or other “special” images.


## &lt;script&gt; tag: Grouped async

The latest versions of all major browsers support the “async” attribute, which means a script won’t block the download or execution of other resources. Once marked async, these scripts are also independent of each other, and may get executed in any order, to keep one async script from delaying another.

The challenge is when scripts are related to each other. It’s very common to have an inline script set an analytics account ID, and then load the full analytics script, or to load jQuery and then use it to load a piece of the page. There’s no easy way to create a group of scripts, and make them async as a group.

I would love to see an “async-groups” attribute, which includes one or more groups this script belongs to. These scripts would be async, but within a group scripts will always be executed in the order they appear in the DOM. One script could belong to multiple groups, and the order within each group would be maintained regardless. Note that this does not mean one group delays another.

Consider the following HTML fragment:

```js
<script src=”critical-1.js”></script>
<script src=”jquery.js” async-groups=”social,analytics”></script>
<script src=”social.js” async-groups=”social”></script>

<script async-groups=”analytics”>
  var analyticsAcct=12345;
</script>

<script src=”analytics.js” async-groups=”analytics”></script>
<script src=”critical-2.js”></script>
```

Let’s take it apart:

1. **critical-1.js** and **critical-2.js** will be downloaded and evaluated inline, blocking other files
2. **jquery.js** and **social.js** make up the “social” group, will not block the rest of the page, but **jquery.js** will always be executed before **social.js**
3. **jquery.js**, the inline script and **analytics.js** make up the “analytics” group, will not block the rest of the page, but will run in the order they appear.
4. **jquery.js** belongs to two groups, and so will block execution of subsequent scripts in each group, but won’t block **critical-2.js** or the rest of the page.
5. The scripts do not have the “async” attribute, for backward compatibility.
 Older browsers will run them inline, preserving execution order (though not as performant). Newer browsers will set an implied “async” attribute on scripts with an “async-groups” attribute.

There are probably various ways grouped async can be implemented – this is just one suggested implementation. As long as the end goal of having a group of scripts run in order but asynchronously is met, I’ll probably be happy with it.


## &lt;link&gt; tag: Defer & Async CSS

CSS link tags block a lot on the page. They block rendering, block most subsequent resources, and are always processed one after the other. This behaviour is designed to avoid FOUC (Flash of Unstyled Content), but it doesn’t contemplate the case of a less important (or less urgent) CSS file.

The best example of a less urgent CSS is a print stylesheet. If a page links to a CSS with a **media=”print”** attribute, chances are the rules in this file only matter when the page is printed. The browser, however, has to process this CSS like all others, since the CSS file *may* set rules for other media types. As a result, the file delays page load for no good reason.

It would be great if the “async” attribute was supported for CSS &lt;link&gt; tags. The behaviour could match the &lt;script&gt; tag’s async and defer attributes, downloading the CSS and applying its rules asynchronously if async is set, or after doc complete if defer is used instead. The other advantage is that backward compatibility is implicitly achieved, as older browsers would simply keep processing this file inline.


## What else?

These are but three of the changes I’d like to see, but they can each have a substantial impact on load time. They revolve around empowering developers, letting them guide the browser on how to load their page.

What’s your wish list? What would you like to see browsers add to make performance optimization easier?
