---
layout: post
title: "An iOS Developer WWDC 2013 Speculation"
date: 2013-05-20 14:36
comments: true
categories: [Apple, Speculation]
published: true
description: "A speculation on WWDC 2013 software announcements"
keywords: "apple, speculation, wwdc 2013"
---
As [WWDC] 2013 closes in, speculation season begins for Apple enthusiasts. In addition to that 2013 has been
a very quiet year for Apple so far (in terms of events and announcements) and a lot has been written about
a UI refresh on iOS and the usual expected hardware updates.

As an iOS developer, I've been following the development of iOS for years and integrating the new OS
features as they were rolled out. I think we can make some educated guesses based on the state of some APIs, and how they have been rolling out
in the past few years.

For starters, I think Apples does this intentionally: a new API is rolled out, Apple recommends the devs to
start using it, and at some point there are so many apps using it, that adding a new OS level feature
related (or requiring) that API is easy and makes all the apps support it immediately.

Here are some technologies / APIs that I've identified in the last few releases of iOS and OS X that I
believe can shed some light on the future of iOS.

### Autolayout
Apple has been very conservative when changing display sizes. They impelemented pixel doubling when moving
to retina displays, making apps run at native resolution with no effort from the developers.

Later, with the iPhone 5, they only changed the height of the screen which made supporting the new screen
straighforward[^TallScreen].

iOS 6 brought us [Autolayout] to manage layout constraints, instead of the old method of springs and strouts.
While great, I think autolayout is overkill just to support taller screens, where springs and strouts
suffice.

Does this open the door for other screen resolutions, and aspect ratios? Let's only hope we don't end up
with the crazy world [Android is](http://opensignal.com/reports/fragmentation.php).

### SceneKit
I haven't used [SceneKit] as I'm mostly an iOS developer (at least so far). SceneKit provides an
[Objective-C API][SiracusaSceneKit] for 3D, neatly wrapping OpenGL calls.

I've only heard good comments about the API. It seems to be solid, and [Delicius Library 3] recently
launched doing its UI with it.

Are 3D interfaces in our future? I think it's possible.  
Could this power new games on an Apple TV that allows apps? I can't tell.  

The only thing I'm sure is that SceneKit in Mountain Lion seems might only the tip of the
iceberg. Remember that Core Animation was introduced in
[Mac OS X 10.5][SiracusaCoreAnimation], way before it became the engine behind UIKit (at least for the
public).  

### XPC
According to the [XPC] docs:
> XPC provides a lightweight mechanism for basic interprocess communication integrated with Grand Central
> Dispatch (GCD) and launchd. The XPC Services API allows you to create lightweight helper tools, called XPC 
> services, that perform work on behalf of your application.

iOS is definitely lacking means of inter-app communication, and XPC seems to fit as an API to provide it.

Apple appears to be using it under the hood when it uses [Remote View Controllers](http://oleb.net/blog/2012/10/remote-view-controllers-in-ios-6/), so it's not far-fetched to
think it will take a more relevant place this time around.

Maybe it's not XPC the technology we'll see, but a higher level version of it like Remote View Controllers,
but I do expect XPC to be a part of some -very needed- interapp communication in iOS 7.

### UIAppearance
[UIAppearance] made its debut in iOS 5. It provides a proxy class to change the
[appearance](http://nshipster.com/uiappearance/) of your native controls such as `UIButton`, 
`UINavigationBar`, etc.

I think `UIAppearance` may have played (and at the time of this writing is playing) two very important roles:

1. It provided a good incentive for developers to stick with standard controls instead of trying to
roll their own. To me, this means that if iOS 7 has an updated UI that appeals to some developers that
decided to use `UIAppearance` to change the look of some OS controls, it will be pretty easy for them
to revert and start using the native iOS 7 look.

2. It might have been a good way for Apple designers to play and try different designs and reskin the OS
default controls during the development stages of iOS 7.

### Closing comments
While trying to figure out what Apple will come out with may seem akin to [Kreminology], in the past
Apple openly tested new frameworks with early versions of the OS only to later start
enforcing their use, or beef up the provided funcionality to entice more developers.

We are only a few weeks away from WWDC, and I can't wait to see what's in store. Here's hoping for
great incremental updates as well as some unexpected surprises.

[WWDC]: https://developer.apple.com/wwdc/
[^TallScreen]: In iOS whenever you are in a phone call or recording a Voice Memo the status bar gets bigger, so apps already support this mode.
[Autolayout]: https://developer.apple.com/library/mac/#documentation/UserExperience/Conceptual/AutolayoutPG/Articles/Introduction.html
[SceneKit]: http://developer.apple.com/library/mac/#documentation/3DDrawing/Conceptual/SceneKit_PG/Introduction/Introduction.html
[^CoreAnimation]: Remember CoreAnimation launched as a small OS X and later became the graphics foundation for iOS.
[XPC]: http://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html
[UIAppearance]: http://developer.apple.com/library/ios/#documentation/uikit/reference/UIAppearance_Protocol/Reference/Reference.html
[9to5iOS7]: http://9to5mac.com/2013/04/29/jony-ive-paints-a-fresh-yet-familiar-look-for-ios-7/
[SiracusaSceneKit]: http://arstechnica.com/apple/2012/07/os-x-10-8/16/#scene-kit
[Delicius Library 3]: http://www.delicious-monster.com
[SiracusaCoreAnimation]: http://arstechnica.com/apple/2007/10/mac-os-x-10-5/8
[Kreminology]: http://en.wikipedia.org/wiki/Kremlinology