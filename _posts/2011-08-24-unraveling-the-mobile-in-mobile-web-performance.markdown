---
layout: post
title: Unraveling the Mobile in Mobile Web Performance
date: '2011-08-24 10:16:27'
---


Everybody agrees the mobile web is big. In the US, Mobile browsing has now surpassed desktop browsing, every 3rd person owns a browsing-enabled phone, and Android devices are selling like hotcakes. With the importance of the Mobile Web comes the importance of Mobile Web Performance, and naturally that spawns a lot of research and conversation.![](http://www.blaze.io/wp-content/uploads/2011/08/mobilequestion.jpg)

In the midst of this craze, it’s worth stopping for a second and asking what exactly is the mobile web? What does “Mobile” mean? Is a windows laptop with a 3G stick a mobile device? What about an iPad on WiFi? Defining Mobile is critical if we want to understand and respond to mobile performance.

I find the best way to understand “Mobile” is to split it into 3 categories: Network, Hardware and Software. Each category comes with its own performance implications, and understanding those is important to keeping your site fast despite the different environment.  
  
 Note: this post discusses smartphone and tablet devices, and not feature phones or older hardware. Such devices are very important and prevalent, but play a lesser role in mobile web performance.

### Mobile Network

Mobile network means a cellular network, such as the current 3G and the newer 4G/LTE networks. Mobile networks are inferior to most wired connections such as DSL or Cable. They provide lower bandwidth and have a higher latency, often topping 100ms. They are less reliable, dropping far more packets. Lastly, they don’t maintain a constant connection between device and cell tower, resulting in a 1-3 second delay each time a new connection is established after an idle network period.  
![](http://www.blaze.io/wp-content/uploads/2011/08/mobilesignal.jpg)  
 These combined problems create a challenging environment for WPO. For example, using Consolidation can help alleviate the impact of the higher latency, but also means a lost packet may cause a more significant delay to the page load. On the other hand, the high latency also makes it more costly to open a new connection, so fragmenting your resources may slow you down even more.

The best advice I can offer for optimizing on a mobile network is to reduce bytes as much as possible, and try to load as much of the page as possible in an asynchronous manner. Less bytes mean less opportunity for failure, and asynchronous loading means a delayed resource won’t block others.

### Mobile Hardware

The next part of “Mobile” is the device itself. A mobile device’s hardware is very different than that of a laptop or desktop computers. It’s smaller, likely uses a touch-screen, has a weaker CPU, etc. Each such difference carries potential performance implications: limitations, opportunities and plain differences.

The performance limitations revolve around the computing strength of the device. Due to limited space and power, mobile devices ship with less CPU and memory than even the simplest laptops. As a result, any client-side scripting takes much longer to run. JavaScript benchmarks show scripts to be about 10-20 times slower, and inefficient CSS or DOM manipulations take a higher toll as well. Even loading JavaScript code and parsing functions, deemed insignificant on desktops, is estimated to take 1-10ms/KB on mobile.

To keep your site snappy on the limited hardware, the obvious solution is to reduce client-side processing. Removing unused code, evaluating JavaScript on-demand and pre-executing code on the server are examples of how that can be achieved.

Performance opportunities surface thanks to the smaller form-factor. A smaller screen means we don’t need to download big screen images. It means we naturally have less room for heavy and slow clutter. The known iPad resolution means we can anticipate exact locations and reduce the complexity of our CSS. These opportunities can help counter the previous limitations, and taking advantage of them is a great way to achieve good mobile web performance.

Lastly, mobile devices are sometimes simply different. They use a touch-screen, not a mouse. They include motion sensors and flip from landscape to portrait modes. These differences aren’t better or worse for performance; they’re just different. If you try to force the desktop mindset on them, you can find yourself triggering expensive reflows when switching orientation, or long click actions when using onclick and not ontouch. Trying to understand the mindset the device was built with is likely the best way to pre-empt such problems.

### Mobile Software

Last but not least, the 3rd leg of mobile is Mobile Software. This includes the mobile operating system, mobile browser, mobile apps and more.  
![](http://www.blaze.io/wp-content/uploads/2011/08/mobileos-300x111.jpg)  
 For Mobile Web Performance, Mobile Software is a friend, not a foe. The software often tries to overcome the hardware and network limitations, for example by disallowing computation-heavy Flash in Mobile Safari, or limiting the total number of connections to 4 on Android. Mobile software developers appreciate the concerns about performance, and try to address them as best they can.

The Mobile Browsers themselves tend to be modern and feature rich, providing good HTML5 and CSS 3.0 support. When optimizing your site, you should take advantage of those capabilities, while trying to avoid undoing the performance improvements already built in.

### Summary

The Mobile Web is a complicated beast, and mobile web performance is equally challenging. It’s much harder to build fast sites when dealing with slower and unreliable networks, along with weak and diverse hardware. However, it’s not all doom and gloom. The modern mobile software, the more predictable form-factors, and smart front-end optimization techniques are opportunities to try and offset the slowing factors. We just need to ensure we make the most of them.


