---
layout: theme:post
title: "Writing Unit Tests in Swift for Objective-C Projects"
date: 2016-09-29T14:22:25-03:00
---

As I [wrote previously](http://pablin.org/2016/09/27/reviving-shopster-testing-in-swift/), I decided to write my (missing) unit tests for an old Objective-C app in Swift.

Immediately after starting, I run into an issue: I can't import the Swift module on the test target.
All the settings appear to be correct: I'm defining a module, including a bridging header. However, whenever I
import my module on Swift I get an error.

The only workaround I could find, was to include a Swift file in the Objective-C project. I'm not sure what dark magic this triggers in Xcode 8, but it makes the trick.

Here's in all its glory, `Dummy.swift`:

```swift
import Foundation

// This file is not used at all on the project, but triggers Xcode to properly
// create the module for testing purposes. ¯\_(ツ)_/¯
```
