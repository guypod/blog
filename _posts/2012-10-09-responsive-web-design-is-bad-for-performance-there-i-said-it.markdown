---
layout: post
title: Responsive Web Design Makes It Hard To Be Fast
date: '2012-10-09 19:38:50'
---


Update: updated title and reference to mdot site below, following feedback

I like Responsive Design. Heck, I LOVE Responsive Design. I think it’s a brilliant methodology, which address true challenges in a very good way. But no matter how fond you are of RWD, I think you have to face the music – RWD makes it very hard to write a fast website.

I’m not saying you can’t write a high performance responsive website. I’m not saying you shouldn’t use RWD (Responsive Web Design) – I would actually recommend it to most organizations. However, RWD makes pages inherently more complicated, and all in all would make the mobile web slower.


## What’s with all the doom and gloom?

What triggered this post was a [great recent post](http://timkadlec.com/2012/10/blame-the-implementation-not-the-technique) by Tim Kadlec, titled “[Blame the Implementation, not the Technique](http://timkadlec.com/2012/10/blame-the-implementation-not-the-technique)”. The post challenges all these absolute, black and white statements, and says good implementation can address most problems. One of his examples is that RWD websites **can** have high performance, if you just implement them correctly.

I agree with Tim’s general point. I’ve been [talking about the common performance problems with RWD](http://slideshare.net/guypod/performance-implications-of-mobile-design-perf-audience-edition) for a while now, and I’m certain most responsive websites out there have terrible performance simply due to poor implementation. I even work on a product that [does that sort of acceleration automatically](http://www.akamai.com/html/solutions/aquaion.html). You can probably make most responsive websites load dramatically faster – often 4 or 5 times faster – on a mobile screen.

However, there’s no escaping the fact RWD – by its very design – puts some real roadblocks in the way of a website trying to be fast.


## So what’s the problem?

The problem is complexity.

In the world of the mobile web, the primary two options are a responsive site or a dedicated mobile website (a.k.a. mdot). To be clear, for the purposes of this post, a dedicated website site means a website design purely for a single (or small range) of screen sizes and contexts. For instance, an mdot site design for smartphones.

An average mdot site – a dedicated smartphone-oriented website – is a very simple site. For starters, it usually has a small HTML, that can be quickly delivered in a handful of packets.  In addition, it has very few scripts, CSS files and images, and each of those tends to be small. While mdot sites can have horrible performance due to various mistakes (see [this deck](http://www.slideshare.net/guypod/step-by-step-mobile-optimization) for examples), they’re just naturally biased in favor of performance (compared to RWD).

Responsive websites, on the other hand, are complex. A responsive page – when loaded on a smartphone – would still require the browser to download and contend with a big HTML file. After the HTML, the site would need to take extra care to avoid running certain scripts, loading certain CSS and download certain images. Avoiding these resources is possible – and desirable – but it’s definitely not easy. In addition, even if perfectly implemented, avoiding those resources requires code and complexity, and has its own performance price.

You can’t escape this fact. A responsive website tuned to perform the best it can would not be as fast as a dedicated mdot site tuned equally well. Or more realistically, an average responsive website would always be slower than an average mdot site.

Update: It’s worth noting that responsive websites can also use a small HTML with scripts that dynamically generate more HTML on bigger screens. This is a good way to avoid DOM bloat, but it only replaces it with new performance problems created by these  blocking scripts, which delay rendering and get in the way of browser optimizations.


## I don’t care. I’m going responsive whether you like it or not.

I know you are. And I would encourage you to do so. As much as it hurts me to say it, performance is not the **only** thing that matters for web pages. It’s also not the first time the web has gone against the performance track.

If you jump back to the early 90s, you’ll find many websites that are purely textual. These websites would perform AMAZINGLY on any browser. They’ll be faster than Google’s home page, and automatically adjust themselves to a screen on any size – as [future friendly](http://futurefriend.ly) as it gets (especially when you consider they were created 20 years ago).

And yet, the user experience on those sites is horrible. The only value in making them fast is that users would very quickly realize they should go to another website. Optimizing your user’s experience must **include** performance, but I’m not naïve enough to think performance is all that matters, nor would I recommend that approach.


## Then what should we do?

We should do what we are currently doing – keep pushing RWD, and make sure we push for performance awareness with it. Push developers and designers to consider performance to be a core requirement, and do the best to address it from day one.  Push browser vendors to make it easier to create fast responsive websites, like the effort to standardize responsive images. Push business to set performance goals and see bad performance as a show-stopping quality bug. On the implementation side, we should do what we can to overcome the main hurdles. Some good places to start [here](http://slideshare.net/guypod/performance-implications-of-mobile-design-perf-audience-edition), [here](http://filamentgroup.com/lab/ajax_includes_modular_content/) and [here](https://github.com/scottjehl/picturefill).

And at the same time, we should accept that RWD means a slower website. Just like Retina images give users richer but slower performance, RWD has its costs. We should accept that as a fact, and do what we can to mitigate the performance cost.


