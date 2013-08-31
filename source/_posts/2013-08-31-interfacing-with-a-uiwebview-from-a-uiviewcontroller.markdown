---
layout: post
title: "Interfacing with a UIWebView from a UIViewController"
date: 2013-08-31 14:59
comments: true
categories: [Cocoa, Development]
description: "Calling Javascript from a UIViewController and calling UIViewController methods from Javascript"
keywords: "uiviewcontroller, javascript, uiwebview, uiwebviewdelegate"
published: true
---
If you are into iOS development chances are that you have already discovered (or been told) that <code>[UIWebView]</code> is a powerfully beast that can be used from more the displaying remote web content.

In past projects I've relied heavily on `UIWebView` to display rich content format by using a local HTML file included in the main bundle (with external CSS and JS files also there). Doing so is straightforward, but sooner than later you might run into the issue of having to interface with what's running inside the `UIWebView` from Objective-C or vice-versa. Luckily both are easy to implement and add a extra layer of power to using UIWebView as a rich content display.

<!-- more -->

## Calling JavaScript from Objective-C
This one is straightforward: `UIWebView` provides the method `-stringByEvaluatingJavaScriptFromString:`. You pass it some Javascript code and get back the result of having it executed in the web view. Easy as it sounds.

## Calling Objective-C code from UIWebView
This one is trickier, as there is no direct way to do it. Here's what I do:  
<code>[UIWebViewDelegate]</code> provides the method `-(BOOL)webView:shouldStartLoadWithRequest:navigationType:`.
This method is triggered before a URL is about to load in the web view. The return value is "YES if the web view should begin loading content; otherwise, NO".

So the trick to bridge interactions in a `UIWebView` and Objective-C code is as follows:

1. Use some links with a dummy URL in the code, like `nativeAction://doSomething`.
2. Detect that URL scheme in the `UIWebView` delegate `webView:shouldStartLoadWithRequest:navigationType:` method.
3. If it matches your predefined URL, execute some native code and return `NO`.
4. Otherwise, return `YES` so the `UIWebView` loads as usual.

## Putting it all together
Let's put both things together with a small sample. Here's how the app looks:

{% img /downloads/images/2013-08-31/UIWebViewSample.png 320 568 %}

It's a `UIWebView` with a `UIToolBar` that has two buttons, to increase and decrease the font size (similar to several online publications would do). The app works as follows:

- Tapping the `UIToolBar` buttons change the size of the content in the `UIWebView`.
- By tapping the `UIWebView` the app hides (or shows) the `UIToolBar` (as is common in reading apps).

Here's the HTML (with embedded Javascript[^ResizeSource]) that the `UIWebView` loads:

{% include_code HTML and JS code 2013-08-31/content.html %}

And here's the `UIViewController` that is also the `UIWebViewDelegate`:

{% include_code HTML and JS code 2013-08-31/QDNViewController.m %}

You can grab the entire code from [this Github repository](https://github.com/pbendersky/pablinorg-UIWebViewSample).

[UIWebView]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/doc/uid/TP40006950
[UIWebViewDelegate]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intf/UIWebViewDelegate
[^ResizeSource]: The source code for resizing the HTML content was taken from [here](http://davidwalsh.name/change-text-size-onclick-with-javascript)