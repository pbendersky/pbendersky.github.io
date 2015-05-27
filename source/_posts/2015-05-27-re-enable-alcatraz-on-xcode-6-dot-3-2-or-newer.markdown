---
layout: post
title: "Re-enable Alcatraz on Xcode 6.3.2 or newer"
date: 2015-05-27 10:33:07 -0300
comments: true
categories: [Development, iOS]
description: Re-enable Alcatraz on Xcode 6.3.2 or newer
keywords: "xcode, alcatraz"
---
I've been using [Alcatraz] to manage Xcode plug-ins for some time now. After updating to Xcode 6.3.2 and restarting, I was
prompted with this:

{% img center /downloads/images/2015-05-27/alcatraz@2x.png 532 282 %}

Without paying too much attention, I clicked "Skip Bundles", and all my Xcode plugins were disabled.

It turns out, Xcode now has a whitelist / blacklist of bundles you enable. You can check it from the Terminal by running:

```
$ defaults read com.apple.dt.Xcode DVTPlugInManagerNonApplePlugIns-Xcode-6.3.2
{
    allowed =     {
    };
    skipped =     {
        "com.mneorr.Alcatraz" =         {
            version = 1;
        };
        "com.onevcat.VVDocumenter-Xcode" =         {
            version = 1;
        };
        "com.travisjeffery.ClangFormat" =         {
            version = 1;
        };
    };
}
```

The bad news is that the prompt to load the bundles won't show again, _even if you reinstall Alcatraz_. The fix is simple though, just delete the whitelist / blacklist by running:

`defaults delete com.apple.dt.Xcode DVTPlugInManagerNonApplePlugIns-Xcode-6.3.2`

and re-open Xcode to be prompted again (and this time make sure you click "Load Bundles").

[Alcatraz]: http://alcatraz.io