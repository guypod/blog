---
layout: post
title: Responsive Images Meetup – A Subjective Summary
date: '2013-09-13 19:03:57'
---


Earlier this week I was at a W3C Responsive Images Community Group (RICG) meetup in Paris. It was a great opportunity to connect with smart folks, some of which I’ve never met before, and I had some great conversations, both as part of the agenda and over drinks.

For those who weren’t able to enjoy the [Parisian views](https://twitter.com/guypod/status/377824680532799488), this blog post holds my (not at all official) summary of what took place, plus my opinions of it. Flo [also wrote](http://shoogledesigns.com/blog/blog/2013/09/13/responsive-images-meetup-coming-together-is-a-beginning/) a blog post with his summary, which I highly recommend. I suspect there will be more detailed and accurate reports coming from the [W3C RICG](http://responsiveimages.org/).


## A bit of background

If you haven’t been following the conversation, let’s start with a quick background on where things stand. This is a short (and again, subjective) summary, you can find the full details on the [RICG website](http://responsiveimages.org/),.

The need for Responsive Images can be broken down into three use-cases:

1. [**DPR Switching**](http://usecases.responsiveimages.org/#resolution-switching): Serving higher res images **only **to devices with higher Device Pixel Ratio (aka Retina devices)
2. [**Viewport Switching**](http://usecases.responsiveimages.org/#resolution-switching): Serving smaller images to smaller or lower-resolution screens
3. [**Art Direction**](http://usecases.responsiveimages.org/#art-direction): Serving a different (often cropped) version of an image to a smaller screen, highlighting the important parts of a picture instead of just shrinking it.

Today, responsive images can only be achieved using JavaScript, CSS or server-side detection, which are harder to implement and have performance implications compared to regular img tags. The current efforts by the RICG aim to enhance HTML (or HTTP) to support responsive images without requiring such workarounds. However, there’s more than way to skin this cat…

Here are the primary not-mutually-exclusive alternatives on the table:

- **[Picture](http://picture.responsiveimages.org/)**: an HTML element similar to HTML5’s <video> tag, which uses media-queries to determine which image to load and is uber flexible.
- **[srcset](http://dev.w3.org/html5/srcset/)**: An <img> tag attribute specifying different URLs to load for a given DPR and possibly resolution.
- **[Client Hints](https://github.com/igrigorik/http-client-hints)**: HTTP header(s) the browser would send, indicating the client’s DPR (and more), allowing the server to serve the correct image.

There are a few other proposals (like Yoav Weiss’s “[Magical Image Format](http://blog.yoav.ws/2013/09/Responsive-Image-Container)”) which have their own merits, but are not as far along.


## Current Status

The first part of the meetup included presentations about real world usage of responsive images today, given by [Mat Marquis](https://twitter.com/wilto) ([Filament Group](http://filamentgroup.com/)), [Mark McDonnell](https://twitter.com/integralist) ([BBC News](http://bbcnews.com/)) and myself ([Akamai](http://www.akamai.com/html/solutions/aquaion.html)). While interesting, they reviewed the past and present and not the future, so I won’t discuss them further here. The next part of the meetup was dedicated to current status of each of the standardization proposals.

Right now, no browser supports any of these proposals, but there has been some progress in their implementation. There are JavaScript polyfills for [picture](https://github.com/scottjehl/picturefill) and [srcset](https://github.com/borismus/srcset-polyfill), and code patches for Chrome & Firefox were written or are underway. Client Hints is a bit behind, but also has a [Chrome Extension](https://chrome.google.com/webstore/detail/client-hints/gdghpgmkfaedgngmnahnaaegpacanlef) to simulate it and a Chrome “[intent to implement](https://groups.google.com/a/chromium.org/forum/#!msg/blink-dev/c38s7y6dH-Q/bNFczRZj5MsJ)”.

However, srcset has been getting more love than that. Both WebKit & Firefox implementations of srcset for DPR only (no viewport) are “almost ready”. The Apple rep there couldn’t confirm (or deny), but I got a strong sense that this feature will show up in Safari soon enough.

We also briefly discussed the [Resource Priorities](https://dvcs.w3.org/hg/webperf/raw-file/tip/specs/ResourcePriorities/Overview.html) proposal, discussed in the W3C [Web Perf working group](http://www.w3.org/2010/webperf/), which suggests new “postpone” and “lazyload” attributes on various elements. Resource Priorities includes a way to avoid downloading hidden images, making it very complementary to srcset (which offers no such method). Note that Resource Priorities is only in draft mode, and has no implementations I’m aware of.


## The Jaded “Implementer”

Amongst those who showed up for the meetup were members of the Chrome, Firefox, IE and iOS/WebKit teams, and I found it especially interesting to hear their perspective. [Ted O’Connor](https://twitter.com/hober) & [David Baron](https://twitter.com/davidbaron), from Apple and Firefox respectively, were particularly blunt in voicing a somewhat jaded perspective on anything that was proposed… Which was sometimes depressing but still good to have. Below are three concrete cases where such feedback was given.

The picture element was practically sneered at by the implementers. It was seen as a “kitchen sink” proposal, which tries to address every possible use-case for this specific need. This may sound appealing, but some needs are actually broader than responsive images, and should be addressed in a platform wide approach (like [Resource Priorities](https://dvcs.w3.org/hg/webperf/raw-file/tip/specs/ResourcePriorities/Overview.html)). Other use-cases may be doable in other ways, and others still are simply not important enough to modify HTML for.

When discussing the use of media queries for determining which resource to load, whether as part of the picture element or not, there was… some unease from the browser folks. Media queries are more complex to evaluate, and thus would require more CPU; they can’t always be evaluated by the preprocessor (think about viewport width in an iframe); and they may be enhanced in the future in ways that would be even harder to process. Unlike picture, though, I didn’t feel like using media queries was entirely dismissed. If there’s enough push and value, I think browsers may agree to take this path.

Lastly, on Client-hints, there was push back against adding headers. One explicit statement was that “while we (browser implementers) aren’t saying we’ll ***never ***add new headers, we’re only one step away from that”. They pointed out that while some header-based content negotiation techniques worked in the past (e.g. Accept-Encoding), many failed (e.g. Accept-Language, Accept-Charset), leaving a sense that this is a wrong approach. Personally, I don’t think past failures are a reason not to try again, and feel like an [opt-in mechanism](https://github.com/igrigorik/http-client-hints/issues/8) for Client Hints would mitigate much of the concern.


## Srcset – the most likely path

As open as the discussion was, at the end of the day these decisions are at the mercy of the browser implementers. And the implementers like srcset, and have actually been implementing it (as implementers often do). And so, it seems pretty clear that srcset will happen.

DPR support in srcset is likely to start showing up very soon. However, the extended [srcset spec](http://www.w3.org/html/wg/drafts/srcset/w3c-srcset/), which includes viewport sizes, is not a foregone conclusion. The main problem lies with the hard-to-parse micro-syntax used in srcset, used to describe permutations of viewport dimensions and DPRs. For example, consider this img tag:

*<<span style="color: #339966;">img</span><span style="color: #ff0000;">src</span>=”a.jpg” <span style="color: #ff0000;">srcset</span>=”**b.jpg 400w 2x, b.jpg 800w 1x, c.jpg 600h 2x**”>*

This small example demonstrates multiple problems:

1. The format isn’t easily readable. You need to know, for instance, that the specified dimensions indicate the “max” dimensions (e.g. a screen 400px wide or less, with a DPR of 2x or less), and not a “min” dimension.
2. There is no way to modify the definition to be a “min” dimension, which conflicts with how “[mobile first](http://www.lukew.com/resources/mobile_first.asp)” media queries are written.
3. The format requires repetition of URLs. In this case, b.jpg was created for screens that are 800px wide. This includes both a 400w/2x screen and an 800w/1x screen. Long URLs and multiple permutations can really make the attribute long.
4. The mix of height and width create ambiguity. For instance, if a 2x device was 400×600, should it load b.jpg or c.jpg? The algorithm dictates what to do, but it’s not at all intuitive.
5. The resolution is always specified in pixels (implied), not supporting large font sizes or zoomed in views.

This conversation was, in my opinion, the most productive one in the meetup, and came up with a few conclusions that most people agreed with:

- For #1, we’ll just deal with it through education. It’s not brain surgery.
- For #2, again, we’ll just deal with it. You can still describe the same conditions, albeit by using funky numbers like 3.99.
- For #3, we discussed supporting OR conditions, but eventually dropped it. We don’t want to over complicate the format, and gzip compresses repetitions very well anyway. We also discussed using media queries, but – as mentioned above – that didn’t go over well.
- For #4, everybody agreed we should drop “h”, but keep “w”. This makes the ambiguity go away, and nobody could figure out a strong need for “h” anyway.
- For #5, we *could* support other resolution units, but there wasn’t much discussion about it.

One important clarification is that srcset is a *recommendation*, not a rule. This means a browser can choose to download a 1x image despite being on a 2x display. It may do so to preserve battery life, reduce bandwidth, accelerate the page, and may do so automatically or based on user preferences. This is different than media queries, which the browser must obey.

This flexibility means all images in a srcset should hold the same image, differing in quality only, since you can’t be certain which one the browser would load. This, in turn, means srcset cannot be used for Art Direction – which gets me to the next topic…


## Not heading in the Art Direction

Some of the conversation was around priorities. Pretty much everybody agreed that the top priority was addressing high DPR (Retina) displays, and most agreed that handling smaller viewports was a close second. However, there was a strong lack of conviction about the Art Direction use-case.

The most common use-case for Art Direction is cropping – only showing a portion of an image on a smaller screen. Cropping can be achieved by serving a different image, but it can also be achieved with CSS overlays, showing only a portion of the downloaded image. This approach means you’ve downloaded wasted bytes, but if the image is already adjusted to the client’s resolution, it’s unclear just how much you’d save with server-side cropping.

For the use-case of showing different images in different viewports, you would need to stick to JavaScript or CSS. The sense in the room was that this is not a strong enough use-case to justify modifying HTML for it.

At some point I suggested adding an optional marker (in a separate attribute or in the micro-syntax) that forces the browser to respect srcset – let’s call it the *strict* marker for now. The *strict* marker would allow us to serve different images to different devices, including art direction needs. The image we serve would need to cover all the viewports in this device (e.g. landscape & portrait), but that’s still smaller than the full image in many cases. We debated this a bit, but didn’t reach any final conclusion.


## Personal Perspective

While the conversation was very interesting, we didn’t really change the direction much… Browsers will stay on course implementing DPR for srcset (likely to show up soon), and my expectation is that they’ll add support for viewport width shortly after.

Personally, I arrived at the meetup favoring picture, but left it favoring srcset… assuming we add viewport and the *strict* marker, and that we implement Resource Priorities. With that combination, I believe we tackle the main performance needs: serve low res images to low res devices (srcset); avoid excessive client-side cropping (*strict*); and avoid downloading hidden images (Resource Priorities). I think it’ll be nice if we also added support for non-pixel viewport definitions.

I still think there’s room for Client Hints, especially if you want your images optimized through an automated tool, like Akamai’s FEO or Pagespeed. In addition, Client Hints is useful for more than just responsive images, and we need to judge it based on its complete value, not a specific hint.

All in all, I found the meetup very educational, and – while they were somewhat antagonistic – I saw a fair bit of merit in the implementer remarks. Hopefully you find this summary useful as well.


