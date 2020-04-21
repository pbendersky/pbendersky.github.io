---
title: "Reviving Shopster - Testing in Swift"
date: 2016-09-27T16:55:02-03:00
---
I'm trying to revive Shopster, an app we launched 2013, and I'm writing througout the process. If this is the first post you see, you can start [here]({% link _posts/2016-09-23-reviving-shopster-worth-it.markdown %}).

---

When I mentioned the [pile of technical debt](http://pablin.org/2016/09/23/reviving-shopster-worth-it/) in Shopster, I neglected to say that it doesn't have any unit nor UI tests[^NSConfTalk].

Now, with lots of changes to be done to fix deprecations, it's one of those times when you regret not having those tests that give you the confidence to move forward with the changes.

So with a better late than never attitude, I created a test target on the project with a slight twist: I'll write all the tests in Swift. This is a low friction place to start adding Swift to the codebase, without rewriting nor having to deal with too much interop. I'm pretty sure that as soon as I add new classes to the app, those will be Swift as well, but I haven't hit that yet.

[^NSConfTalk]: Even though [I talked about it](http://pablin.org/nsconfarg16/) during an NSConf Argentina!