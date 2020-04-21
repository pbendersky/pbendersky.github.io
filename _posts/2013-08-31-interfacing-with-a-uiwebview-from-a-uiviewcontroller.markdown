---
title: "Interfacing with a UIWebView from a UIViewController"
date: 2013-08-31 14:59
comments: true
categories: [Cocoa, Development]
description: "Calling Javascript from a UIViewController and calling UIViewController methods from Javascript"
keywords: "uiviewcontroller, javascript, uiwebview, uiwebviewdelegate"
published: true
---
If you are into iOS development chances are that you have already discovered (or been told) that [`UIWebView`] is a powerfully beast that can be used from more the displaying remote web content.

In past projects I've relied heavily on `UIWebView` to display rich content format by using a local HTML file included in the main bundle (with external CSS and JS files also there). Doing so is straightforward, but sooner than later you might run into the issue of having to interface with what's running inside the `UIWebView` from Objective-C or vice-versa. Luckily both are easy to implement and add a extra layer of power to using UIWebView as a rich content display.

<!-- more -->

## Calling JavaScript from Objective-C
This one is straightforward: `UIWebView` provides the method `-stringByEvaluatingJavaScriptFromString:`. You pass it some Javascript code and get back the result of having it executed in the web view. Easy as it sounds.

## Calling Objective-C code from UIWebView
This one is trickier, as there is no direct way to do it. Here's what I do:  
[`UIWebViewDelegate`] provides the method `-(BOOL)webView:shouldStartLoadWithRequest:navigationType:`.
This method is triggered before a URL is about to load in the web view. The return value is "YES if the web view should begin loading content; otherwise, NO".

So the trick to bridge interactions in a `UIWebView` and Objective-C code is as follows:

1. Use some links with a dummy URL in the code, like `nativeAction://doSomething`.
2. Detect that URL scheme in the `UIWebView` delegate `webView:shouldStartLoadWithRequest:navigationType:` method.
3. If it matches your predefined URL, execute some native code and return `NO`.
4. Otherwise, return `YES` so the `UIWebView` loads as usual.

## Putting it all together
Let's put both things together with a small sample. Here's how the app looks:

![](/assets/images/2013-08-31/UIWebViewSample.png)

It's a `UIWebView` with a `UIToolBar` that has two buttons, to increase and decrease the font size (similar to several online publications would do). The app works as follows:

- Tapping the `UIToolBar` buttons change the size of the content in the `UIWebView`.
- By tapping the `UIWebView` the app hides (or shows) the `UIToolBar` (as is common in reading apps).

Here's the HTML (with embedded Javascript[^ResizeSource]) that the `UIWebView` loads:

{% highlight html %}
<html>
    <head>
        <script language="javascript">
            function resizeText(multiplier) {
                if (document.body.style.fontSize == "") {
                    document.body.style.fontSize = "1.0em";
                }
                document.body.style.fontSize =
                parseFloat(document.body.style.fontSize) +
                (multiplier * 0.2) + "em";
            }
            
            function touchStart(event) {
                sX = event.touches[0].clientX;
                sY = event.touches[0].clientY;
            }
            
            function touchEnd(event) {
                var parentNode = event.target.parentNode
                if ( parentNode != null && parentNode.nodeName.toLowerCase() == "a" )
                    return;
                
                if (event.changedTouches[0].clientX == sX &&
                    event.changedTouches[0].clientY == sY) {
                    
                    window.location = "nativeAction://hideShow";
                }
            }
            
            document.addEventListener("touchstart", touchStart, false);
            document.addEventListener("touchend", touchEnd, false);

            </script>
    </head>
    <body>
       Lorem ipsum dolor sit amet, consectetur adipiscing elit.
       Proin at justo malesuada, accumsan justo non, dapibus ante.
       Maecenas luctus at magna nec convallis. Suspendisse ac
       purus molestie mauris porttitor viverra sed a risus. Aliquam
       pellentesque gravida ante, eu mattis lectus.
    </body>
</html>
{% endhighlight %}

And here's the `UIViewController` that is also the `UIWebViewDelegate`:

{% highlight objective_c %}
//
//  QDNViewController.m
//  WebViewSample
//
//  Created by Pablo Bendersky on 8/31/13.
//  Copyright (c) 2013 Pablo Bendersky. All rights reserved.
//

#import "QDNViewController.h"

@interface QDNViewController ()

@property (nonatomic, strong) IBOutlet UIWebView *webView;
@property (nonatomic, strong) IBOutlet UIToolbar *toolBar;

- (IBAction)decreaseTextSize:(id)sender;
- (IBAction)increaseTextSize:(id)sender;
- (void)resizeTextAdding:(NSInteger)textSizeIncrement;

@end

@implementation QDNViewController

- (void)viewWillAppear:(BOOL)animated {
    NSURL *localFileURL = [[NSBundle mainBundle] URLForResource:@"content"
                                                  withExtension:@"html"];
    [self.webView loadHTMLString:
     [NSString stringWithContentsOfURL:localFileURL
                              encoding:NSUTF8StringEncoding
                                 error:NULL]
                         baseURL:nil];
}

- (IBAction)decreaseTextSize:(id)sender {
    [self resizeTextAdding:-1];
}

- (IBAction)increaseTextSize:(id)sender {
    [self resizeTextAdding:1];
}

- (void)resizeTextAdding:(NSInteger)textSizeIncrement {
    NSString *jsString = [NSString stringWithFormat:@"resizeText(%d)", textSizeIncrement];

    [self.webView stringByEvaluatingJavaScriptFromString:jsString];
}

#pragma mark - UIWebViewDelegate methods

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request
 navigationType:(UIWebViewNavigationType)navigationType {
    
    if ([request.URL.scheme caseInsensitiveCompare:@"nativeAction"] == NSOrderedSame) {
        [UIView animateWithDuration:0.3 animations:^{
            self.toolBar.alpha = (1 - self.toolBar.alpha);
        }];
    }

    return YES;
}

@end
{% endhighlight %}

You can grab the entire code from [this Github repository](https://github.com/pbendersky/pablinorg-UIWebViewSample).

[`UIWebView`]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/doc/uid/TP40006950
[`UIWebViewDelegate`]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intf/UIWebViewDelegate
[^ResizeSource]: The source code for resizing the HTML content was taken from [here](http://davidwalsh.name/change-text-size-onclick-with-javascript)