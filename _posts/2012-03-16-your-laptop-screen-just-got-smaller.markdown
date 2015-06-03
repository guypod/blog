---
layout: post
title: Your Laptop Screen Just Got Smaller
date: '2012-03-16 11:41:14'
---


The new iPad shipped today, and a lot of reviews and comments are popping up. In the performance and mobile design world, those conversations revolve primarily around images in this new Retina iPad era.

The new iPad boasts an incredible resolution of 2048 by 1536. This is bigger than the full high definition resolution (1920×1080), and is definitely an impressive feat. To add a few more comparison points, my new 13’’ MacBook Pro’s resolution is 1280×800, and even on [mediaqueri.es](http://www.mediaqueri.es), an inventory site for Responsive Mobile Design, a desktop site is considered to be 1600px wide.

Soon enough we’ll have several millions of these new iPads spread across the globe, making this high resolution screen a new standard. My meager laptop, which has 3 times fewer pixels, will become a low-resolution screen in comparison. Based on the [W3C statistics](http://www.w3schools.com/browsers/browsers_resolution_higher.asp), only ~11% of users use a screen with HD resolution or higher. That statistic is about to change.


## What’s so bad about more pixels?

Problem is, high-resolution screens tend to make non high-resolution images look… fuzzy. So if you want your site to look razor sharp on the new iPad, you’ll need to up the resolution of your images, to meet the new standard. Doing so will significantly increase the image size. A not-entirely-accurate equation is that 3x the pixels means 3x the size. A sample image from the Apple website served to a [regular ipad](http://images.apple.com/home/images/ipad_hero.jpg) vs the [new ipad](http://images.apple.com/home/images/ipad_hero_2x.jpg) is indeed more than 3 times bigger (350K vs 110K).

According to the [HTTP Archive](http://httparchive.org/interesting.php), images account for about 60% of an average page size. If you increase the image size by 3x, you will more than double the average page size! This is clearly a doomsday prediction, as not all images would be resized accordingly, but it’s still a scary proposition, and one that goes against the trend we’re trying to set about making sites smaller. The general guideline for good mobile design is to “Lighten Up”, as [Stephanie Rieger put it](http://www.alistapart.com/articles/the-best-browser-is-the-one-you-have-with-you/), later reinforced by [Jason Grigsby](http://cloudfour.com/first-thing-you-should-do-to-optimize-your-desktop-site-for-mobile/) and others, not to double up.


## Ok, I’m depressed. Now What?

Beyond venting, what can we actually do about this new situation? It’s unlikely Apple would roll back the resolution, and chances are competitors will attempt to match it quickly enough. As a website owner or designer, two solutions that come to mind: Scalable Vector Graphics (SVG Images) and a revised version of Responsive Images.

[SVG](http://en.wikipedia.org/wiki/Scalable_Vector_Graphics) essentially means replacing your image with instructions to the rendering engine on how to draw it. These instructions create the pixels as needed on the current screens, and can create a sharp image on any display (if properly created). SVG support in browsers (and [specifically mobile browsers](http://mobilehtml5.org/)) is quite good, but not good enough. For instance, Android versions prior to Ice Cream Sandwich (meaning 98% of Android devices out there today) don’t support it.

The other angle is Responsive Images, but its logic may need to be flipped (to some extent). The idea of [Responsive Images](http://cloudfour.com/responsive-imgs/) is to **reduce** the resolution of images served to a mobile device, since the mobile device’s screen is smaller and supports a lower resolution, and users can’t appreciate the bigger images. The new version of Responsive Images may consider your laptop screen “small”, and downscale images from the lofty iPad resolution to avoid wasting bytes.

What I would love to see is browser support for responsive images, making it easier to load the appropriate image for your display. Jason Grigsby discussed some [good ideas](http://cloudfour.com/responsive-imgs-part-3-future-of-the-img-tag/) on how that can be achieved a while back, hopefully the browser folk are listening.


## Summary

The new iPad’s resolution is actually not much of a surprise. The race towards higher resolution has been going on for a while, and Apple has clearly indicated where it’s headed with the iPhone Retina display. It’s also hard to really argue it’s a bad thing. We all like a crisper display and the opportunities it opens.

So we just need to adapt, use the tricks in our bag today to serve the right image to the right screen, and hope the browser software catches up and provides the capabilities we need to handle the latest hardware improvements.


