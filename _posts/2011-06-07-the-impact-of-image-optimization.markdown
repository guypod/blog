---
layout: post
title: The Impact of Image Optimization
date: '2011-06-07 11:33:11'
---


Most of the conversation in the WPO community revolves around HTML, scripts and stylesheets. These are clearly the more complex items on pages, and so there are many variations on what can be done with them. However, images account for over 50% of the total page size, the largest single component.

The total images size grew more than 10% over the last year, as shown by the [HTTP Archive](http://www.httparchive.org/compare.php). In the last upgrade to Blaze, we added and improved several image optimization techniques. These optimizations can make a material impact on your load time and on your bill, by reducing the download size per page.

**In this post I’ll explain 4 key image optimizations:**

- Lossless Image Compression: Remove surplus info from image
- Lossy Image Compression: Match image quality to screen resolution
- Image Format Conversion: Replace encoding to minimize size
- Images On-Demand: Only load images as they scroll into view

### Lossless Image Compression

In most websites, images are provided by a graphics designer. This designer works with imaging software like Photoshop, which supports very rich color histograms, keeps metadata about dates and edits, and is generally designed to make editing simple – as opposed to creating the smallest output file.

When you serve up the page, you don’t need this data. If you don’t need your visitors to open and edit your website images, you’re sending them a lot of wasted bytes they’ll never use. This metadata is similar to comments in HTML/JavaScript/CSS. They’re useful when developing (or designing), but are just wasted space on a page.

Lossless Image Compression is the process of removing such metadata and excess information, and possibly rearranging the data, without impacting the image quality.

### “Lossy” Image Compression – Reduce to Screen Quality

Lossless image compression eliminates excess data but the image may still be of higher quality and size then your users could possibly appreciate. A basic camera today takes pictures at a 10 Megapixel quality. A monitor displaying a 1024×768 resolution shows less than 1 Megapixel of data. Even a full HD resolution of 1920×1080 is only about 2 Megapixel.

So, if you take a picture and post it on the web, you’re sending 5-10x more data than what the users would use. If you expect your users to print high-quality printouts of your pages, it may prove useful. But otherwise it’s just a waste. An interesting fact is that even your camera performs this trade-off, and generally converts the TIFF image it takes into JPEG format, losing some quality in the process. Only a small fraction of users have a need for the extra data.

Lossy Image Compression attempts to reduce the image to match screen quality. These algorithms aren’t simple, but good algorithms with good default settings will drastically reduce the image size with no visible impact (on a screen).

### Image Format Manipulation

There are many ways to save an image. Each type is best suited for different usage scenarios, ranging from animated GIFs to ICO icons to JPEG photos. For the web, the most common formats are PNG, JPEG & GIF, but practically all formats can be found on different sites.

In a web page, choosing the right format can save significant bytes. Even within a format, different encodings may impact the result. For example, JPEGs can be saved as “progressive” or “baseline”, [each more efficient for some images](http://www.yuiblog.com/blog/2008/12/05/imageopt-4/). Choosing the format should be done as a part of the Lossless or Lossy Image Compression.

Blaze converts every image into multiple types of formats and encodings, and chooses the best result. At the end of the day, that’s the only way to squeeze the last bytes out of every image.

### Load Images On-Demand

No matter how hard you work at getting your images to be as efficient as possible, the right size for an image that isn’t viewed is 0 bytes. This means that if a user had to scroll to see an image and didn’t – your bytes are wasted. Most websites have below-the-fold content that is never actually viewed. Google released a nice visualization of how much of your site is visible to what percent of users: [http://browsersize.googlelabs.com/](http://browsersize.googlelabs.com/) (Update: The tool is being integrated with Google Analytics, here’s an [alternate one](http://www.thewebshowroom.com.au/uploads/above-below-fold-tool/)).

The solution to this problem is to only download the images on-demand. Images that are “above the fold” will be loaded right away, while images below the fold would only be loaded when they scroll into view. If your pages auto-refresh, or if your users use low-resolution screens (or mobile screens), the impact would be even greater. The page will load faster thanks to the reduced download, and your bandwidth costs will see a significant drop.

### Image Optimizations On Real Sites

In the process of developing image optimizations in Blaze, we applied the different optimizations to many websites. It became clear that even highly optimized websites stand to benefit from image optimization, sometimes by huge margins.

Here are some examples of the original and optimized page sizes after each optimization:

### Page Sizes (KB)

<table border="1" cellpadding="0" cellspacing="0" style="text-align: center;" width="90%"><tr><th>URL</th><th>Original</th><th>Lossless</th><th>Lossy</th><th>On-Demand</th></tr><tr><td>**[www.cnn.com](http://www.cnn.com)**  
% Savings:</td><td>**1,032**</td><td>**880**  
15%</td><td>**863**  
16%</td><td>**986**  
4%</td></tr><tr><td>**[www.thesun.co.uk](http://www.thesun.co.uk)**  
% Savings:</td><td>**1,852**</td><td>**1,729**  
7%</td><td>**1,657**  
11%</td><td>**1,275**  
31%</td></tr><tr><td>**[www.techcrunch.com](http://www.techcrunch.com)**  
% Savings:</td><td>**3,616**</td><td>**3,458**  
4%</td><td>**2,944**  
19%</td><td>**2,131**  
41%</td></tr></table>### Summary

Image Optimization can make a significant impact on most sites. It reduces bandwidth considerably, saving precious dollars, and reduces load times in the process.

Lossless Image Compression and loading images on-demand are usually a no-brainer. They benefit most sites, and rarely have a negative impact. Lossy Image Compression sounds scary, but it’s in fact designed to have no visible impact, and can provide great value. You should at least try it before making a decision. Lastly, image formats and encoding techniques can further improve the result, at times reducing sizes by an extra 10-20%.

If you want to see the impact image optimization would have on your site’s load times and bandwidth, submit your URL on our [home page](http://www.guypo.com). If you’d like to experiment with the different image optimizations and see their individual impact on your pages, sign up for a [free trial account](http://www.guypo.com/trial/)..


