---
layout: post
title: "Introducing CocoaPods thumbs"
date: 2015-05-18 17:41:52 -0300
comments: true
categories: [Development, Cocoa]
description: CocoaPods thumbs, a plugin for peer votes of CocoaPod specs
keywords: "cocoapods, cocoapods thumbs, cocoapods plugin"
---
Today we released [CocoaPods thumbs]. It's a CocoaPods plugin that allows users to check for peer votes of the dependencies
they are including in the project. We are using it at Quadion (our thumbs are public, you can check them [here](https://github.com/quadion/thumbs)).

## Motivation

We started using CocoaPods as soon as we discovered. It's great for seasoned developers, but can quickly turn into a double-edged sword. Searching for the magical Podspec that solves a specific problem quickly becomes second nature to any developer (only second to copying and pasting unknown code from Stack Overflow).

The problem is that there are tons of Podspecs, and the quality varies hugely. From long time abandoned code, to code that violates AppStore submission rules by using private APIs, to code with too many bugs.

So I wanted to improve on this, at least a little bit, and then CocoaPods thumbs was born.

## How it works

The use case is straigforward. There's a server that returns a JSON with a given format, including Podspecs (with version requirements) and votes (optionally with comments).

Users first register the server URL (which conveniently can be a GitHub URL to the RAW JSON file), and then simply run `pod thumbs` from a folder with a Podfile. When run, CocoaPods thumbs will:

1. Calculate the dependencies.
2. Check the server votes.
3. Match the resolved dependencies with the votes, and display it to the users.

There's also an alternative command to just display the votes of a single Podspec, useful to help you decide if a particular dependency has opinions among your team members.

## The future

I'm not sure how this will evolve. I'd love for this to become a part of CocoaPods daily usage for more people, and maybe consolidate our thumb lists instead of each team having their own. For now, if you use it, feel free to submig Pull Requests, or suggest enhancements on [GitHub][CocoaPods thumbs].

[CocoaPods thumbs]: https://github.com/quadion/cocoapods-thumbs