---
title: "Reviving Shopster - Make It Build"
date: 2016-09-23T17:52:19-03:00
---
I started [analyzing](http://pablin.org/2016/09/23/reviving-shopster-worth-it/) wether it was worth reviving [Shopster] or not.

So first step into the plan: clone the project's repo, and see what breaks.

For starters, I did not follow my own [CocoaPods suggestion]({% link _posts/2014-05-04-cocoapods-best-practices.markdown %})[^NoWonder], so the `Pods` folder is not part of the repository.

Luckily, Shopster's Podfile is pretty short:

```
platform :ios, '7.0'

xcodeproj 'Groceries'

pod 'PSAlertView'
pod 'TestFlightSDK', '~> 3.0'
pod 'BlockAlertsAnd-ActionSheets', '~> 1.0'
pod 'FMMoveTableView'
pod 'OHAttributedLabel', '~> 3.4'
```

Apart from these Pods, we are using [Crashlytics](http://fabric.io) and [AskingPoint](https://www.askingpoint.com), which I no longer would like to have in the app.

First step to fix the Podfile then: remove old dependencies (TestFlight was acquired by Apple long ago!) and adopt the new syntax. The resulting `Podfile` looks like this:

```
platform :ios, '7.0'

project 'Groceries'

target 'Shopster' do
    pod 'PSAlertView'
    pod 'BlockAlertsAnd-ActionSheets', '~> 1.0'
    pod 'FMMoveTableView'
    pod 'OHAttributedLabel', '~> 3.4'
end
```

This new Podfile has this warning: `[!] OHAttributedLabel has been deprecated in favor of DTCoreText`. I'll deal with that later.

I also removed `AskingPoint.framework`. The project still fails to build. The `pch` (remember those?) is including `<TestFlightSDK/TestFlight.h>`, and of course, I'm initializing it in the `AppDelegate` and using it in a reporter class. Removing these makes the project build!

Current state: Shopster runs on the iOS 10 Simulator, just like it runs on my iPhone. All the low hanging fruit has been taken care of.

[Shopster]: http://www.shopsterapp.com
[^NoWonder]: No wonder why: the post is from May 2014, and Shopster is from June 2013. A testament of how can I change my opinion on tools usage in a year.