---
layout: post
title: "iOS 8 Wish List"
date: 2014-05-26 19:38:20 -0300
comments: true
categories: [Apple, iOS]
description: iOS 8 Wish List
keywords: "ios 8, ios, wish list, wwdc 2014"
published: true
---
With WWDC 2014 around the corner[^WWDCAttending], it's time to think out lout about my iOS 8 wish list.

While I have several small things I think could be improved, there are a few I'd like to see make the cut this year.

## Multi User Support on the iPad
Ever since the first launch of the iPad, multi user support was a rumored feature. I always thought Apple's rationale behind not providing multi user support was two-fold: it would add complexity to the platform, and it might take away some sales (why get a second iPad if I can just create a user for the kids  ?).

I've matured into thinking the only reason is complexity, as I don't think having multiusers would hamper sales.

The good news is that the APIs to support multiple users are all there. Any app that wants to get the location of the documents folder needs to call an API (which coincidentally on the mac, returns the Documents folder of that user). The sandboxed nature of the platform also helps here: if your app is limited to its Sandbox, it won't care if it's the user's sandbox or the app's sandbox.

There are some challenges, however, that Apple would need to solve, which I think are hard and might take this feature away from us:

- Backups. iCloud backups are tied to an Apple ID. If you were to support multiple users, one Apple ID would have to be set as the primary user, and the system backup tied to it.
- Global storage per app. Applications would need to change slightly to support storing some information in the app sandbox and some in the user's sandbox. Say an In-App purchase. All the users of a given iPad will expect it to be available. However, if the app stores a flag in the user's defaults, other users won't see it. This requires tweaking on all the apps to support this.
- User switching. Luckily here, iOS apps have always supported state preservation, so switching to another user would require the system to preserve the state on all the apps it hasn't already, and just switch. Easy, and no app changes should be required.

## Alternative: Kids folder
Windows Phone 8 supports a feature called [Kids Corner]. With that feature enabled, you can set up some apps with a dedicated start screen. When you hand your phone to your kid, just swipe the home screen to the Kids Corner, and they will only able to use those apps and switch between them.

With SpringBoard Folders, I figure Apple could easily implement a feature like this to provide something less strict than Guided Access while guaranteeing your kids do not switch to another app.

## Some sort of File Management
Sadly here, I don't see Apple doing any sort of File Manager on iOS. Even more, I think they are moving in the opposite direction with iCloud storage on Mac apps, where each file type is tied to the app that created it. But given that this is my wish list, I fiture it's fine to dream.

The Mac, even when using iCloud storage, provides both tags and an All Files view in the Finder, where you can find apps inside any app's sandbox. iOS could easily do the same without breaking the sandbox. Provide a simple app to browse all your data files, filter by app, name or tag and have the appropriate app open the app.

## System Contributions from apps
Ideally, apps should be able to provide more flexible content to the OS. There are abundant examples of this in iOS already:

- The Clock and Calendar apps change its icon.
- When using driving directions, the maps app takes over the Lock Screen.
- Podcasts[^PodcastsStore] provides a custom rewind and fast forward buttons on the lock screen, that moves the podcast by 30 seconds each tap.
- iTunes Match has a custom view in Control Center.
- Reminders shows a checkbox alongside the notification on the lock screen.

Ideally, I would like Apple to provide a few places where app developers can tap into some system provided features to customize the experience. I guess the risk here is that it can quickly turn into a huge mess, with apps cluttering the lock screen or notification center with crappy content, but Apple can control this in at least two ways:

- They can easily provide very specific controls, without giving us developers much ability to customize what they can do.
- It can get strict during the review process to reject apps that fail to comply with some guidelines.

## OS X / iOS integration
In Apple's ecosystem you have your Mac, and a powerful device you carry around at all times. Macs are usually more stationary and used for longer periods of time.
I'd like for Apple to leverage connectivity between the two (maybe using Bluetooth 4.0) and provide new interactions:

- Trigger certain notifications only when your phone is away from the Mac.
- Establish a secure link between the Mac and iOS and use Touch ID to unlock your Mac.
- Have an iPad act as an AirPlay display if you want to, etc.

----

I'm looking forward to this years WWDC Keynote (and to be _in the room_ as it happens). Let's hope Apple has some surprises on its sleeve.

[^WWDCAttending]: I'll be attending WWDC 2014!
[^PodcastsStore]: Podcasts appears to have special blessing for some features, even though it's distributed through the App Store. It recently gained Siri integration as another example.
[Kids Corner]: http://www.windowsphone.com/en-us/how-to/wp8/people/set-up-kids-corner