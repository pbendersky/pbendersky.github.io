---
layout: theme:post
title: "How could Apple extend AirPlay"
date: 2013-05-08 22:41
comments: true
categories: [Apple, Speculation]
description: "A speculation on how Apple could extend Airplay to better use the Apple TV processor"
keywords: "apple, airplay, speculation"
---
This is a highly speculative article. I'd like to be able to say it's based on educate guesses, but sadly
I think it's not. 
In any case, I consider it a good excersice and hope you can join me imagining a future we might not see.

AirPlay now
-----------
AirPlay allows for easy streaming of Audio and Video from iOS devices, and with newer devices even to be
used as a secondary screen in some apps. The effect is no short of impressing. You tap a few buttons,
and whatever is on your device's screen magically starts showing up in the scren, with the correct
aspect ratio (I'm actually waiting for an NBA game[^SpursGame] to start as I write this, streamed
from my iPad next to me)[^NBAArg].
 
There is, however, something that puzzles me when I use AirPlay: the processor on the Apple TV is mainly idle
aside for the (probably GPU bound) task of decoding a video stream.

AirPlay 2.0
-----------
What I envision is an extension of AirPlay that allows apps to offload part of their processing to the Apple 
TV.
I think it's feasible, and even more, some important pieces of the puzzle (APIs) are partially there.

With an API similar to [XPC], a future iOS API could allow devices to start a process remotely on the Apple
TV and provide means for communicating between the devices.
Want a Draw Something-like app? The controller that manages the turns might run on the Apple TV, while each device
connected to it acts as the drawing board for each of the teams.

There are already apps that do something similar to this, but they require one of the devices to be the
master and run the game for the rest. With the AirPlay approach, the Apple TV would take the role of the
master, leaving more processing power for the devices.

Why not Apple TV Apps instead?
------------------------------
I think Apple TV Apps are comming. It's just a matter of when, not if.  
However, I also think for a casual user, it might prove too complex to have to download a companion
Apple TV app in order to play a group game.
For common users it might be easier to  start a session from the device they already have [^ItCanAlsoAsk]
rather than requiring them to download an additional app on the Apple TV (which may be not their own, but a
friend's).

Conclusion
----------
I fully expect Apple to make a bigger move into the living room, and I expect that move to come from cleverly
written software rather than amazing hardware.

This AirPlay extension has been in my mind for a long time, and thought it was a good time to speculate
about it publicly.

[^SpursGame]: GS @ SA, Game 2 form the Western Conference semi finals. I'm a big Manu Ginobili fan.

[^NBAArg]: For some weird reason, if you set up an Apple TV in Argentina, you don't get the built in Apple TV app. Weirdly enough, you can get an iOS app that allows AirPlay.

[XPC]: http://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html

[^ItCanAlsoAsk]: If you want to go even further, a clever app could detect there's an Apple TV in the network and offer the networked mode automatically.
