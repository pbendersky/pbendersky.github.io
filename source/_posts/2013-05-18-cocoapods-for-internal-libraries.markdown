---
layout: post
title: "CocoaPods for internal libraries"
date: 2013-05-18 13:55
comments: true
categories: [Cocoa, Development]
description: "How to use CocoaPods to manage internal / private libraries"
keywords: "cocoapods, internal libraries, private libraries"
---
[CocoaPods] is a great way to manage dependencies (mostly open source code) in your iOS or OS X project. 
It implements a [Gemfile] like configuration file (called a [Podfile]) where you declare your dependencies
and provides all the means to keep those libraries up to date as your project grows.

I want to cover a less common use of CocoaPods: creating your own Pods repository
for internal use.

<!-- more -->

### Prerequisites
For simplicity I'm assuming you are already familiar with CocoaPods, and that you have access
to git repositories you can control.

I'm also assuming that you know how to write your own [Podspec].

### Creating your specs repository
The first thing you need to do, is create a git repository that you'll use to keep [Podspecs]. 
Let's assume this repository ends up in `git@git.example.com:MySpecs`.

### Add the repository to CocoaPods
To do that, you need to execute: 
`pod repo add MySpecs git@git.example.com:MySpecs`.  
If you check your `~/.cocoapods` folder, you should now see two folders: `master` and `MySpecs`.

### Create your Podspecs
You create a Podspec by running: 
`pod spec create NAME`

Let's say you run `pod spec create MyLibrary`, it will generate a file called `MyLibrary.podspec`. Now you
need to edit that Podspec to make it complete and lint. Once you do, you can lint it to make sure the project
compiles, by running `pod spec lint MyLibrary`.

If everything passes, you are good to go! If not, fix the errors in the Podspec and make sure it lints.

### Adding the Podspec to your repository
The final step is adding the Podspec to your private specs repository. To do that, you run
`pod push MySpecs MyLibrary`.

This step will lint the library again, and copy it to you local repo, and push that repo to git. If you don't
want it to push, as an extra measure, you can add the `--local-only` parameter[^LocalOnlySuggestion].

### Sharing with teammates
To share these specs with your teammates, they only need to add the new specs repository to CocoaPods by
running `pod repo add MySpecs git@git.example.com:MySpecs`.

### Conclusions
CocoaPods is an amazing tool. For [some companies](http://www.quadiontech.com) dealing with internal
reusable libraries is a daily business. Using CocoaPods as depicted has several benefits:

- Your internal specs can use the dependency management of CocoaPods, just as any open source library.
- Your projects use a single way of dependency management, both for open source libraries and internal
ones.
- It's straightforward to share an internal specs repository with teammates.

[CocoaPods]: http://cocoapods.org
[Podspec]: https://github.com/CocoaPods/CocoaPods/wiki/The-podspec-format
[Podspecs]: https://github.com/CocoaPods/CocoaPods/wiki/The-podspec-format
[Gemfile]: http://gembundler.com/v1.3/gemfile.html
[Podfile]: https://github.com/CocoaPods/CocoaPods/wiki/A-Podfile
[^LocalOnlySuggestion]: I strongly suggest you do this, and test the spec with your project before pushing. A common mistake for me in the past has been to forget header files or resources in the Podspec, which won't prevent it from linting, but will make the project that uses them fail.