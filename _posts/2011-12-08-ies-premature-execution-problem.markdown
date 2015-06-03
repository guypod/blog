---
layout: post
title: IE’s Premature Execution Problem
date: '2011-12-08 11:21:02'
---


Recently we came across a problem so bizarre I was amazed I never saw it before: IE executes dynamically created scripts before they’re added to the DOM!

This problem took a while to figure out, and has many gotchas, and so it seemed worthwhile to share our experiences, and perhaps save you some debugging time.


## A bit of background

Adding scripts to the DOM is a standard practice. Scripts often need to add dynamic elements to the page, for reasons such as simplifying a 3<sup>rd</sup> party tool integration, manipulating the browser’s loading logic and more. While there are some cases where you can use document.write() and still sleep at night, in the vast majority of cases you’re better off using DOM insertion.

Adding a script to the DOM is pretty simple. Here’s a simple example taken from Stoyan’s [recent Perf Calendar post](http://calendar.perfplanet.com/2011/the-art-and-craft-of-the-async-snippet/):

 <script>(function(d) { var js = d.createElement('script'); js.src = "http://example.org/my.js"; d.getElementsByTagName('head')[0].appendChild(js); }(document));</script>


## IE Strays from the pack

If you’re deeply familiar with browser inner workings, you may already know that IE doesn’t handle the code above like everybody else. The difference lies in when does it download the script to execute.

Most browsers would only make the request to http://example.org/my.js  after line 4, when the script got added to the DOM. IE, however, would start the download as soon as the src attribute is set, after line #3.

Personally, I find IE’s behaviour less intuitive, since before inserting an element to the DOM, I still see it as more of a variable than a true component of the page. However, some tools, such as [LABjs](http://labjs.com/), seem to prefer it, and use it as a way to preload scripts into the page.

In addition, the [HTML 5 spec](http://www.w3.org/TR/html5/scripting-1.html#running-a-script) explicitly calls out that

> “user agents may start fetching the script as soon as the attribute is set, instead, in the hope that the element will be inserted into the document”.

 And in fact, most browsers fetch an image URL as soon as they encounter a***“new Image().src=…”*** call.


## IE Goes Too Far

So far this was just a summary of the past. What I recently discovered was that – under certain conditions – IE in fact *executes* the script before it’s ever added to the DOM! This is a big no-no, and goes against the next line in the HTML5 spec, which says

> “If the UA performs such prefetching, but the element is never inserted in the document, or the src attribute is dynamically changed, then the user agent will not execute the script, and the fetching process will have been effectively wasted”.

The trigger to make IE execute the script prematurely is appending it to a parent element, regardless of whether the element is in the DOM or not. This script will demonstrate the problem:

<div> var container = document.createElement("div"); var elem = document.createElement("script"); elem.src = "notify.js"; container.appendChild(elem);

</div> 

In all browsers but IE, the code above will not even download notify.js. In IE, the code is supposed to only fetch it. But in reality, the script is downloaded and executed in IE. If you want to see a complete HTML, you can find an HTML performing and logging a few actions [here](http://www.blaze.io/experiments/ie-preexec.htm).

A few more details:

- It doesn’t matter if the src attribute is set before the appendChild or vice versa. Either way, the script is executed once there is a parent and src attribute.
- It doesn’t matter (as far as I can tell) what is the parent node, including parents that don’t really script children, such as <textarea>, and including documentFragment
- The problem occurs at least in IE 6-9, in all “modes”.


## The Inevitable Bugs

This type of weird behaviour often comes with various subtle bugs it creates, or at least inconsistencies. I did some digging, and saw a few quirks you should be on the lookout for. Most of these are represented in [this example page](http://www.blaze.io/experiments/ie-preexec.htm).

### readyState updated before DOM Insertion

Since IE fetches and executes the script, it’s unclear which load events should be called for it. Our testing shows that the onreadystatechanged event, and the corresponding elem.readyState value, all get updated as soon as the script gets executed. This means the element is likely to be in a “loaded” state before it ever gets added to the DOM.

If the element was in that “loaded” state before being added to the DOM, no additional events will be called after the element gets added to the DOM. This is an important fact to remember if you’re waiting for the script after adding it to the DOM.

### IE 9: If a Script is cached, execution time may differ

If this obscure IE bug wasn’t bad enough, how about throwing some inconsistent behaviour into the mix?

As it turns out, in IE 9 the script *may* only get executed when it’s truly added to the DOM. More specifically, if the script is already in the cache, it will often get executed only AFTER it’s added to the DOM (like all other browsers do).

[Click here](http://www.webpagetest.org/result/111207_Z8_b03976badec57fd71f05ae8009ba98be/) for a demonstration of this behaviour, using WebPageTest to load the test page in IE9. The line in bold marks where the script got executed, note how that happens before appending it to the DOM in the first view, and after appending it to the DOM in the repeat view.

In our tests we’ve seen it happen in various scenarios, so there may be other factors at play.

### IE 9: Script may run in the middle of another script

As per the example above, if the script DOES get executed when the element gets added to the DOM, it gets executed *immediately*. This means the external script gets executed in the middle of another script, and not right after the current script is done.

While dynamic script execution order is never guaranteed, executing a script in the middle of another one is a concern. JavaScript is designed to alleviate the concern of race conditions or competing threads, and this case breaks that promise.

<div>For example, consider an inline script that looks like this

 addExternalFile('loadUserData.js'); initUser('joe');

Assume addExternalFile() adds an external script. If line #2 initializes user settings that loadUserData.js requires, the code above would still work. The current script always finishes running before the external script is invoked. However, in the behaviour we’ve see in IE9, this guarantee breaks, the external script may run inline, and the page may break.

</div>### IE 8: Script may never get executed

I don’t have an exact understanding of what triggers this, but we’ve run into a few cases where a cached script was set to a “loaded” state as soon as the src attribute was set. After that, when the script actually got added to the DOM, the scripts readyState moved *backwards* into “loading”, firing the onreadystatechange event in the process.

On IE 9, these scripts then proceeded to a “complete” state, and only got executed when the script got added to the DOM. On IE8 they *usually* didn’t. Instead, they reverted to the “loading” state and never got out of it. In such cases, the script was never actually executed.

### DOM Scripts out of sync at execution time

This quirk is a bit easier to understand. Since the script gets executed before it actually got added to the DOM, it won’t be in the DOM if you look for it…

Some scripts, such as [scriptaculous](http://script.aculo.us/), traverse the DOM looking for themselves. Scriptaculous does this so it can exact a query (Facebook Connect and Dojo sometimes do that too), but there are various reasons to do so. When the script getting executed isn’t in the DOM yet, these lookups will obviously fail, regardless of how they are done.


## Summary & Workarounds

This problem feels like a real annoyance. It will likely only manifest in very specific cases, but the inconsistent nature of it and the lack of clear detail will make it challenging to detect. If you did or plan to do any more digging into this, do share in the comments below.

As for working around it, the best option is probably to set the src attribute only after adding the script element to the DOM. Doing so should address most of the problems described above. The only problem that will remain is one script running in the middle of another script, which can be resolved using standard programming – just run your dependencies before referencing the external scripts.

If you’re a Blaze FEO customer, don’t worry – we’ve got this covered!


