---
layout: theme:post
title: "Adopting Adaptive"
date: 2014-08-07 14:51:40 -0300
comments: true
categories: [Apple, iOS]
description: Adopting Adaptive UI in iOS. Summary of discussion around Dynamic Type, Adaptive UI
keywords: "ios 8, adaptive uikit, universal apps"
published: true
---
One of my biggest takeaways of WWDC 2014 was Adaptive Apps. If you haven't yet, you can check the video for session 216, 
[Building Adaptive Apps with UIKit](https://developer.apple.com/videos/wwdc/2014/#216).

In the past days, there have been a few articles about it (I got to them via [Brent Simmons](http://inessential.com)):

- [Developers, The iOS Line and Reactive Design](http://www.libertypages.com/clarktech/?p=315)
- [Thinking In Terms Of iOS 8 Size Classes](http://carpeaqua.com/2014/06/14/thinking-in-terms-of-ios-8-size-classes/)
- [Device Sizes](http://inessential.com/2014/08/05/device_sizes) (updated to include a Tweet I sent Brent in reply).

Over the last three years (as I think is usual with Apple), we've seen a succession of technologies that we were "suggested to adopt":

- iOS 6 brought us Auto Layout, as a declarative way of creating constraints between our views.
- iOS 7 introduced Dynamic Text, allowing users to easily tune the sizes of all the fonts in their devices.
- iOS 8 is adding Adaptive Apps in UIKit and [Presentation Controllers](https://developer.apple.com/videos/wwdc/2014/#228).

Adaptive apps are sold to us for two main reasons: supporting both iPhone and iPad with the same codebase with less specific cases, and supporting the new rotation. New device form factors are -of course- never mentioned in any WWDC session, nor is the ability to resize apps on an iPad, as [apparently will be the case](http://9to5mac.com/2014/06/11/heres-the-ipad-split-screen-app-mode-apple-is-working-on-in-ios-8-video/).

If the next iPhone indeed ships with a bigger screen, the main reason would be to have more content on screen at the same time, not to have something akin to an `@3x`. So, if we assume the screen will be bigger (to fit more content), we only have one question: will the screen have the same DPI as the existing iPhone?

Let's explore the ramifications of each option.

### Same DPI as iPhone 5s

Ever since the original iPhone, objects on screen have had the same physical size. The move to retina only increased the pixel count, but the actual size remained the same.

Keeping the same DPI will be an impact similar to when the iPhone 5 shipped: our apps will need to adapt and show more content. If the phone is wide enough, there's a chance the Horizontal Size Class in Portrait of this new device changes to Regular as opposed to Compact, and our apps will need to adapt to it (`UISplitViewControllers` will became more common, for one). With the same logic, the Vertical Size Class in Landscape might change to Regular.

All in all, this change will probably be the easier to deal with, as typography and images will look the same.[^AnyGivenText]

### Higher DPI than the iPhone 5s

As [I tweeted to Brent Simmons](https://twitter.com/pbendersky/status/496718827116568576), here's where I think it gets interesting.

If Apple wants to have the text _look_ the same size, they might tune the Dynamic Text default sizes on the new device to compensate for the higher DPI[^Still2x].

On session [226, What's new in Table and Collection Views](https://developer.apple.com/videos/wwdc/2014/#226) you can see how they use the multiplier in Auto Layout constraints so that `UITableViewCell`s change in height as the Dynamic Text size changes, and look better when bigger fonts are in use. 
I can see this approach being the recommended way forward, basing the layout around the text sizes, so Apple can tweak them based on the hardware they ship.

## Other thoughts

### What about `userInterfaceIdiom`?

My notes from session 216 were that `UIUserInterfaceIdiom` was deprecated. What they actually said is:

> In the past you've used UIInterfaceOrientation and UIUserInterfaceIdiom to differentiate between portrait and landscape
> and iPhone and iPad, but in iOS 8, we're recommending against using these two concepts, and instead we're advocating this > new concept that we call size classes.

(source: [asciwwdc.com](http://asciiwwdc.com/2014/sessions/216))

Yet, [`UITraitCollection`] contains a `userInterfaceIdiom` trait. Why was this kept, if Apple is suggesting against basing the UI based on the device? My understanding is that it's there because we'll still need it to determine the _intent_ of the user. Our apps need to know if the user has launched our app on his iPhone, probably on the move or doing something else, or on an iPad, where we can assume he's more comfortable and about to have a longer use session.[^Capabilities]

### Business implications

I don't think there's a huge business implication for this right away. If you want to ship to separate apps for iPhone and iPad, I don't expect Apple to prevent you from doing it.

However, since you'll have to support iPhones with multiple screen sizes, it's very possible that going from a multi-sized iPhone app to a Universal app will be a simpler transition. This, I'm confident, might have the nice outcome of combating what Marco Arment called [App Rot] and provide more quality universal apps for the users.

### Implications for font heavy apps

There are some apps that rely a lot on custom fonts, such as [Vesper]. Since Dynamic Text does not allow you to use custom fonts, I wonder how those apps will work around a different Dynamic Text default size. There are tons of valid workarounds, like fetching the system font size and base your custom font size on it, but an Apple blessed solution would do a lot to help in this direction.[^CSS]


[^AnyGivenText]: Any given text, at the same Dynamic Text setting will look the same on the iPhone 5s and on the new iPhone.
[^Still2x]: I'm assuming the new device still runs with a display scale of 2.0, as the retina screens we currently have.
[^Capabilities]: I first thought this had to do with device capabilities, such as to tell if the app needs to highlight phone numbers or not. I think that's a blurrier line, as we don't know what capabilities future devices will have, and well behaved apps need to check for specific capabilities instead, not make assumptions based on the `userInterfaceIdiom`.
[^CSS]: While I'm not sure what the best solution is, I'll be happy to have something similar to a very simple CSS file, where you can specify for each Text Style the font name, size and weight you want it to take.
[`UITraitCollection`]: https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitSet_ClassReference/index.html
[App Rot]: http://www.marco.org/2014/07/28/app-rot
[Vesper]: https://itunes.apple.com/us/app/vesper/id655895325?mt=8