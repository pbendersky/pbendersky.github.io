---
title: "Add MIME types to GroundControl remote config"
date: 2013-07-04 14:11
comments: true
categories: [Cocoa, GroundControl]
description: "Adding mime types to GroundControl remote configuration"
keywords: "cocoa, groundcontrol, mime types"
---
[GroundControl] is an amazing library by [Mattt Thompson][Mattt] that provides you a simple way to enable
remote configuration of your Cocoa app.

[GroundControl] is part of [Helios], a bigger set of libraries worth checking out, and is based on [AFNetworking], which you might already be using.

One of the issues I run into when using GroundControl for the first time, was that the remote `plist`
was hosted in Dropbox, and the returned MIME type was `plain/text` instead of the expected
`application/x-plist`.

Since GroundControl is based on AFNetworking, this is easy to fix. Just add the MIME Type to 
`AFPropertyListRequestOperation` as follows:

{%highlight objective_c %}
NSURL *url = /* your URL */;
[AFPropertyListRequestOperation addAcceptableContentTypes:[NSSet setWithObject:@"text/plain"]];
[[NSUserDefaults standardUserDefaults]
    registerDefaultsWithURL:url
                    success:^(NSDictionary *defaults) {
                                                      }
                    failure:^(NSError *error) {
                                              }
 ];
{% endhighlight %}


[GroundControl]: https://github.com/mattt/GroundControl
[Helios]: http://helios.io/
[Mattt]: https://twitter.com/mattt
[AFNetworking]: https://github.com/AFNetworking/AFNetworking