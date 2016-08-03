---
layout: theme:post
title: "Prevent Your Mac From Sleeping With Caffeinate"
date: 2016-08-03T18:07:37-03:00
categories: macOS, Terminal
---
I've known `caffeinate` for a long time, but I found a new use today that prompted me to write about it.

For those you don't know, `caffeinate` is a command line tool that prevents your Mac from going to sleep. From it's `man` page:

> caffeinate creates assertions to alter system sleep behavior.  If no
> assertion flags are specified, caffeinate creates an assertion to prevent
> idle sleep.  If a utility is specified, caffeinate creates the assertions
> on the utility's behalf, and those assertions will persist for the duration of the utility's execution.
> Otherwise, caffeinate creates the assertions directly,
> and those assertions will persist until caffeinate exits.

Let's say you need to run a long task, like restoring a DB in postgres with `pg_restore`. The way you use `caffeinate` would be to run:

    $ caffeinate -i pg_restore database database.dump
    
But let's say you forgot to run `caffeinate` and your process has already been running for too long to cancel and restart. Before today, my approach was to use [Caffeine] to prevent the Mac from going to sleep. However, there's a second way to invoke `caffeinate`:

    $ caffeinate -i -w 12345
    
where `12345` is the pid of the process you want to finish without the Mac going to sleep.

[Caffeine]: https://itunes.apple.com/us/app/caffeine/id411246225