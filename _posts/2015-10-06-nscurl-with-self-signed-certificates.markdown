---
layout: theme:post
title: "Using nscurl With Self-Signed Certificates"
date: 2015-10-06T18:21:22-03:00
categories: [Development, iOS]
keywords: nscurl, ssl, ios
---
New with iOS 9 and Mac OS X 10.11 is [ATS](https://developer.apple.com/library/prerelease/ios/technotes/App-Transport-Security-Technote/), App Transport Security. To help you troubleshoot HTTPS settings, Apple is providing a command line tool called `nsurl`. 

As I'm using a Self-Signed certificate, `nscurl` fails for me with all its settings. Something like this:

```
---
TLSv1.2 with PFS disabled
2015-10-06 18:11:59.609 nscurl[39559:5040069] CFNetwork SSLHandshake failed (-9801)
2015-10-06 18:11:59.610 nscurl[39559:5040069] NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9801)
Result : FAIL
---
```

As far as I could find, there's no parameter to specify a certificate to be used. The only work-around I was able to use is:

1. Install the certificate on your Mac's keychain.
2. Using Keychain Access, change the trust settings of the certificate to "Always Allow".

With this change you should be able to run `nscurl` and see the results. Please remember to remove the certificate from your keychain afterwards.