---
title: "CocoaPods Best Practices"
date: 2014-05-04 15:11:41 -0300
comments: true
categories: [Cocoa, Development]
description: "CocoaPods Best Practices. Best pratices for using CocoaPods to manage your project dependencies"
keywords: "cocoapods, best practices"
---
I've been using [CocoaPods] for about a year now, and I've changed my mind several times on certain best practices. I'd like to share what
I currently think is best.

## On choosing what Pods to use
CocoaPods has tons of source code available. This is great, but may also be a double-edged sword: given the option of using several libraries to accomplish the same, what should you look for? Below is the criteria I tend to use:

- Is the project alive? If you check its GitHub page, does it show activity? Are new commits coming in? What about tickets? Unless it's a very simple library, I'm sure some users have encountered certain issues with it and may have reported them. Were these addressed?
- Do you know any big project that relies on that Pod you are about to include? If you trust the project or the developer, that may be a good sign.
- Is the codebase something you feel you could rewrite from scratch or work around it instead? I tend to feel more comfortable working with code that I feel I could have wrote. There are exceptions, of course (maybe [Facebook Pop] is over my head, but the previous rule could prevail here).
  This is not a matter of time (you are using an external library not to spend time rewriting it from scratch on the first place) but a matter of skills. If you don't feel you could write that library, you might not have the proper understanding on how it works, which will help you work around any issues you may encounter.
- Is the license something your project can use? Developers tend to overlook this, but it will be a huge letdown to find out some dependency you are using is GPL when you are about to ship[^LicNote].

## Version Control and CocoaPods
Following the [official advise][CocoaPods Usage], I commit everything to version conotrol:

- `Pods` folder.
- `Podfile`.
- `Podfile.lock`.

The rationale behind this is that you should look at CocoaPods as a _development tool_, and not as part of the build process. Any other developer downloading the repository should be able to run the project without having to fiddle with `CocoaPods` or RubyGems.

## Update your Pods as a sanity check
Since you've committed your `Pods` to version control, you might feel compelled to never run `pod update`. It's fine, but I suggest you run it every now and then as a sanity check. Running it may trigger errors in your build environment (Podspecs needing an updated CocoaPods installation, for example), or show a newer minor fix to a library you are using.

## Use a Ruby version manager such as rbenv
If you are Ruby developer, you are certainly familiar with this topic. If you are not, Ruby verison managers allow you to install different versions of Ruby side by side, with different sets of Gems. In addition, they are installed in your home folder, so you don't need to use `sudo` everytime you want to update a `gem` (like `CocoaPods`).

I use [`rbenv`][RBENV], but I've also tried [`rvm`][RVM] and both work great.

## Use Homebrew
I'm sure you have already installed this, and if you not, you have probably seen the need to do so for the Ruby version manager. [Homebrew] is a package manager for OS X. It's way better than [`MacPorts`][MacPorts] and will help you install and manage Unix packages on your Mac.

[CocoaPods]: http://cocoapods.org
[FaceBook Pop]: https://github.com/facebook/pop
[CocoaPods Usage]: http://guides.cocoapods.org/using/using-cocoapods.html
[RVM]: http://rvm.io/
[RBENV]: https://github.com/sstephenson/rbenv
[Homebrew]: http://brew.sh/
[MacPorts]: http://www.macports.org/
[^LicNote]: While I haven't checked this, so far I haven't run into very strict licences (everything I've use has been BSD or MIT).