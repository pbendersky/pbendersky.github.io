---
title: "Overthinknig the Next Apple TV"
date: 2015-08-25T09:14:03-03:00
categories: [Apple, Speculation]
keywords: apple tv, tvOS, speculation
---

There's been rumors of an updated Apple TV for years now. Before WWDC 2014, there were [rumors about a new Apple device
to control home automation devices](http://pablin.org/2014/05/26/what-if-the-rumored-apple-home-automation-is-the-next-apple-tv/). 
Before WWDC 2015, [rumor was that the Apple TV was delayed](http://www.macrumors.com/2015/06/03/redesigned-apple-tv-not-ready-for-wwdc-debut/) at the very last minute. 
Fast forward to now, September 2015, and [everyone](http://9to5mac.com/2015/08/17/what-will-septembers-new-ios-9-based-apple-tv-bring-to-the-living-room/) [seems](http://daringfireball.net/linked/2015/08/21/new-ipad-event) to agree an Apple TV is comming.

Here's what I've been thinking about the next Apple TV.

## Operating System and App Store

I think the Apple TV will run (as it has since the black hardware we currently have) a version of iOS. I'm guessing we'll get a formal name for it: tvOS[^tvOS].

tvOS will be the same familiar set of frameworks for iOS with some new top level UI elements (think the equivalent of `UIKit`) and support for a different interaction model (remote control clicks and selections instead of tapping, or swiping on a pad).

There's no doubt in my mind that tvOS will support an App Store. I'm looking forward to apps like [Plex](https://plex.tv) to be natively available, as well as apps for TV channels and sports. The current channels on the Apple TV are limited in both functionality and features, while having a full blown App Store will allow developers to create better experiences for the TV.

If my guess is correct, all the existing apps will be easy to port, so we'll have a large number of apps in weeks.

[^tvOS]: And if we don't get it, at least I'll get to use it for the rest of this post.

### The Multiuser challenge

Let's think of some scenarios:

- If you have a tvOS running Apple TV with an App Store, it will make sense to have it support Handoff. Say you start watching a YouTube video on your iPhone, as soon as you enter your living room and turn the TV on, you can hand it off to the YouTube app running there, and you can leave your phone.
- Say you use Spotlight on tvOS to search for a TV show. It will display results from the Netflix app, but Netflix supports different user profiles, some even filtering kids only content.

The main challenge I see here is that while iPhones are strictly personal devices, the Apple TV is a _shared resource_ in a _shared space_ at home. I don't have an answer to this, but I'd love for tvOS to ship with an implementation of multi-user support. All iOS apps are already multi-user ready (whenever you get a folder from the OS, you use the same API as OS X that returns the current users folder), so this is doable.[^iPadMultiUser]

[^iPadMultiUser]: And if we get this feature, Apple will be running out of reasonable excuses to not include multi-user support on the iPad.

## Gaming

iOS is an amazing platform for gaming. Not only for indie games, but for [AAA games](https://en.wikipedia.org/wiki/AAA_(video_game_industry)) as well. With a powerful hardware and [Game Controller support](https://developer.apple.com/library/ios/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html) the possibilities are huge.

I think the Apple TV has the potential of becoming a huge Game Console for casual (or why not all) gamers.

## HomeKit

At the WWDC 2014 keynote we got [two new frameworks announced](https://youtu.be/w87fOAG8fjk?t=5665): HealthKit and HomeKit.

Given the time they got on stage, my guess is that Apple think of them as a big deal. HealthKit got its device with the Apple Watch. I think HomeKit is about to get its with the Apple TV.

As our homes become full of connected devices, having a central hub that is part of our home LAN and can be always on will be essential for controlling the devices while away.

## Hardware

I won't speculate on the hardware, but from what [9to5mac published](http://9to5mac.com/2015/08/17/what-will-septembers-new-ios-9-based-apple-tv-bring-to-the-living-room/) it appears that will get a fairly modern A-series CPU and a new remote.

I do have a few comments, though:

- The CPU and GPU don't need to be as speed-limited as in mobile devices. Even without a fan, I'm confident they can run it at higher clock speeds, or don't have them throttle that fast.
- Having IR on the remote doesn't make much sense to me, unless Apple wants to make its remote a universal remote, which I don't think. Universal Remotes are a mess[^UniversalRemoteOpportunity], and how well they work depends on the other components manufacturers[^UniversalRemoteState], which is something Apple doesn't like.

[^UniversalRemoteOpportunity]: So there's an opportunity.
[^UniversalRemoteState]: If your TV set doesn't support discrete on and off codes, for example, the remote won't be able to track if it's state.

## Conclusion

The buildup for this Apple TV revision has been large. We can easily see traces of it in frameworks that originally shipped with iOS 8. I'm eager to see both new hardware and how Apple solves the challenges in the OS side.
