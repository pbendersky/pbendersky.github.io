---
layout: theme:post
title: "Reviving Shopster - In Review"
date: 2016-10-30T16:42:06-03:00
---

I'm trying to revive Shopster, an app we launched 2013, and I'm writing througout the process. If this is the first post you see, you can start [here](http://pablin.org/2016/09/23/reviving-shopster-worth-it/).

---

After some time, I finally submitted Shopster 1.2 for App Store review. As I write this, it's currently "In Review", and I expect it to be available soon.

I wanted to wrap up this series with a summary of the changes and what I expect to do in the future.

## Final Metrics

I took some [baseline metrics] before I started. Here's the final summary:

```
github.com/AlDanial/cloc v 1.70  T=1.10 s (101.4 files/s, 6675.3 lines/s)
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
Objective C                     46           1176            494           3638
C/C++ Header                    53            295            368            477
Swift                            7            122             65            350
HTML                             1             19              0            159
JSON                             2              0              0            140
Markdown                         1              3              0             33
XML                              1             11              0             12
Bourne Shell                     1              2              4              5
-------------------------------------------------------------------------------
SUM:                           112           1628            931           4814
-------------------------------------------------------------------------------
```

Worth noting:

- I started using Swift. All new classes were written in Swift, and one small refactor (code I moved out from the AppDelegate) was rewritten in Swift as well.
- There are some shell scripts, JSON files, etc, that were not present before. I integrated [fastlane].

## Dependencies

In the process, I also removed some third party dependencies we used. The app is now much tighter and less dependent on third party code that I don't manage.

## Deprecations

I solved all the deprecation warnings except one: I'm still using [`ABCreateStringWithAddressDictionary`]. As far as I can tell, the suggested replacement (`CNPostalAddressFormatter`) doesn't quite cover my use case. I'll review it in the future, and maybe rewrite that method, as it's not that important for the app.

## UI and AutoLayout

All the screens now use Auto Layout. Where there were animations that changed the `frame`, I changed to update the constraints and animate that change instead.

## Short Term Work

Here's a short list of things I'd like to tackle in the near future (wihtout counting major architecture changes such as using CloudKit):

- Start using the default system font (San Francisco) instead of the custom font we are using.
- Adopt Dynamic Type across the app.
- Remove some graphics I think make the app look dated, like the logo on the Navigation Bar.
- Localise the app in Spanish[^Vergonzoso]. It's currently English only.
- Fix the missing deprecation warning.
- Use the new location based notification trigger. Currently, we observe significant location changes, and when they match the areas we want, we trigger a Local Notification immediately. The new `UserNotifications` framework, allows you to set a location as a notification trigger, which would help eliminate part of this code. Since this is core to the app, I didn't want to take on it now, as it will require more testing.
- Add a new onboarding screen. As soon as you start the app, you need to allow location services and notifications. We simply throw the system dialogs to the user. We can do better.
- Add `@3x` assets. I didn't have them, and wanted to ship soon. They'll come at a later update.
- Make the app free, and show ads only to new users (the ones who got the app for free, basically).

## Long Term Work

This list is shorter in items, but way harder, so I don't know if or when I'll do it:

- Take on major refactors for the app. Adopt MVVM.
- Start using CloudKit for sync.
- Adopt a freemium model, where a one time payment unlocks some functionality.

## Closing Thoughts

- Don't let your apps rotten! Either pull them from the App Store gracefully, or spend the required time to keep them up to date at the very least through major OS versoins.
- It's hard giving up on an app you put effort on. I couldn't, that's why this version is on review. This is probably not the best example to follow.

[baseline metrics]: http://pablin.org/2016/09/26/reviving-shopster-baseline-metrics/
[fastlane]: http://fastlane.tools
[`ABCreateStringWithAddressDictionary`]: https://developer.apple.com/reference/addressbookui/1624198-abcreatestringwithaddressdiction?language=objc
[^Vergonzoso]: My native language! Shameful.