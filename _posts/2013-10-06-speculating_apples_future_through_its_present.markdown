---
title: "Speculating Apple's future through its present"
date: 2013-10-06 14:47
comments: true
categories: [Apple, Speculation]
description: "A speculation some technologies Apple laid out for future products."
keywords: "apple, speculation, ios, game controllers"
---
Trying to guess what future products Apple will unveil is a game you are bound to loose. That said, I still find it a game
I enjoy playing.

Right now, I think we are in a rare position where Apple has laid out the foundation for several different things they can pursue
in the future. I don't know if any of these will materialise (and if they do, how) but I love speculating about it.

Here are a few technologies Apple has recently launched, and is basically [hiding in plain sight](http://www.asymco.com/2011/12/12/hiding-in-plain-sight/).

## M7, Core Motion and Wearables
The iPhone 5s ships with a motion coprocessor called the M7. Basically the M7 is a small processor (sidekick in Apple's parlance) that gathers information from the accelerometer, gyroscope, and compass and keeps it to be easily accessible through the [Core Motion API](https://developer.apple.com/library/IOs/documentation/CoreMotion/Reference/CoreMotion_Reference/_index.html).

The M7 is a great concept: keep a small processor always on, so not to drain the battery of the device by powering a more hungry A7 chip.

Core Motion seals the deal: it provides you simple APIs to gather that data based on time intervals, so your app can easily get that information and present it to the user with no data gaps.

Here's where I think wearables could be tied: imagine an M7 processor on your wrist, linked to your iPhone with Bluetooth LE. Core Motion can then work as a data aggregator and present the developer consolidated information from either the built-in M7 in the device, or a paired remote M7 processor in a wearable device.

## iOS 7 Game Controllers support and the Living Room
Besides all the user facing changes, iOS 7 introduced several under the hood changes as well. One of them was support for third party game controllers developed under the [MFi program](https://developer.apple.com/programs/mfi/). iOS 7 was released, but the controllers were not (at least, not yet).

At the same time, the Apple TV (which also runs iOS, in case you were distracted) was updated to version 6.0, based on iOS 7. I can only guess that the support for controllers is baked in there. Moreover, besided [the new setup feature](http://support.apple.com/kb/HT5900?viewlocale=en_US&locale=en_US) in the Apple TV, its Bluetooth capability was always a mistery to me (pairing a keyboard? ...).

I hope Apple is thinking to make a bigger entry into the living room and using the existing Apple TVs as a trojan horse, as opposed to shipping a full fledged TV set. I still would love to see the [extension to AirPlay I discussed here](http://pablin.org/2013/05/08/how-could-apple-extend-airplay/), although an App Store (or channel store) would be welcome as well.

## Auto Layout and Bigger Screens
With the transition to iOS 7, Apple [strongly suggested](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/TransitionGuide/index.html) developers to adopt [Auto Layout](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/AutolayoutPG/Articles/Introduction.html). It's reasonable, since native controls in both iOS 6 and iOS 7 have different intrinsic sizes, so basing a UI on sizes instead of constraints wouldn't work.

However (and I don't think unexpectedly by Apple), this opens the door for easier support of more screen resolutions. The push for bigger screens on phones is here to stay, and while I like Apple's 4 inch form factor a lot, there's a huge market for bigger devices as well.  
Doubling the resolution is no longer an option (or at least, an option that would make any sense), so the only feasible direction without supporting other resolutions would be to base a bigger iPhone in the DPI of the Retina iPads [as speculated by Marco Arment](http://www.marco.org/2013/01/31/iphone-plus-speculation). While I agree that this is the simplest path for having a bigger size display, now that most apps will be supporting Auto Layout, having a slightly different screen size might not hurt as much[^Skeu].

[^Skeu]: The transition away from textures helps as well. In iOS 6 it was common to include PNG files exactly sized for nav bars and other stuff. With iOS 7, a lot is conveyed with colors and blurs, making that obsolete while helping a transition to multiple resolutions as well.

---

With an event in October all but confirmed by Apple, let's see if any of these speculations materializes. As always, any feedback if appreciated!