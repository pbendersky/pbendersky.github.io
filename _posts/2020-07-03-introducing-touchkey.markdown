---
title: Introducing TouchKey
date: 2020-07-03T17:26:56+00:00
---

## Short version

TouchKey is a small Mac application that lives on your status bar and on the Control Strip of your TouchBar (if you Mac has one).

You can download TouchKey [here][TouchKey download link], try it for a week, and activate it for USD 5.99 if you find it useful.

You can configure TouchKey to send keyboard shortcuts to any running app, or to a Safari tab matching a given URL.

TouchKey was born out of necessity. In this time when we are all working remotely, I found myself using Zoom, Slack, and Google Meet for conferencing, and I wanted a quick way to mute any of those tools quickly, from the TouchBar or the status bar.

![TouchKey status bar menu](https://photos.collectednotes.com/photos/20/51debd5e-307c-4500-82e4-d1f974904d52)

### Alternatives

When researching for TouchBar, I found several competing alternatives, like [Mutify](https://mutify.app), or [Mic Drop](https://getmicdrop.com).

These apps work by muting the actual microphone in your Mac, something more akin to a hardware switch.

TouchKey, on the other hand, is definitely a software switch. The main advantage I see (when used for muting) is that your peers at the call can quickly see your mute status, which turns out to be useful if you tend to start speaking without realising you are muted.

Also, since TouchKey is a generic tool, you can configure it to send any keyboard shortcut to any running app. Do you open a new private Window Safari often? You can configure that shortcut.

## Slightly Longer version

Ever since I got my first Mac, I was fascinated with small apps that were almost invisible for the user, but did a useful task.

I remember back then (when the tool was Project Builder and not Xcode) trying to understand how to develop those small utilities, but failing both to understand basic Cocoa concepts but also to come up with a useful idea on what to develop.

The combination of working a lot from home lately, and having conference calls using several different tools finally made it for me: I needed a global mute button on my Mac.

I tried developing TouchKey with SwiftUI, but quickly found a few bugs that ended up being showstoppers, so mid-development I switched to the old and trusty AppKit, the same I’ve been scared more than 10 years ago.

[TouchKey download link]: https://quadion.tech/touchkey/release/TouchKey.dmg
