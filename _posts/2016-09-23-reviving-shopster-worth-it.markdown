---
layout: theme:post
title: "Reviving Shopster - Worth it?"
date: 2016-09-23T16:23:53-03:00
---
We launched [Shopster] in 2013. It was the last app we developed we published under [Quadion]'s name.
Even though it had great launch coverage, [it tanked](http://pablin.org/2013/06/06/the-ios-appstore-in-2013/).

For 2016 standards, the app has piled up on technical debt. It currently:

- Doesn't dupport screen sizes larger than the 4' iPhone 5.
- Is not using auto-layout.
- Uses several deprecated APIs.
- Has workarounds for several things.

And, of course, it's 100% Objective-C, as Swift was not around by the time we wrote it.

So, what would be the motivation to keep investing in it?

1. We like the app. We still keep it in our home screens and use it.
2. It still has some users (iTunes reports about 100 active users per day).
3. Is an app we like to showcase.
4. We could try a new business model.

And finally, we got a ticket last week asking us to please, please, add support for iOS 10.

### On Sharing

When we launched Shopster, we neglected to include a sync feature. We figured it was not that important, but our users
kept asking for it.

By the time we had to make the call, the best choice was to roll our own syncing engine and incur in a monthly cost, knowing almost for certain,
that we might not be able to afford it in the long run.

Fast forward a few years, and Apple shipped CloudKit and more recently CloudKit sharing, which would allow us to enable sharing of shopping lists at no monthly cost.

### Will we do it?

I'm not sure. What I'll sure do (and write about) is see the state of our app, and what's the effort involved in bringing it back to life.

[Shopster]: http://www.shopsterapp.com
[Quadion]: http://quadiontech.com
