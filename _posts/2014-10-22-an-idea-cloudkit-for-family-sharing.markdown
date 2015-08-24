---
layout: theme:post
title: "An idea: CloudKit for Family Sharing"
date: 2014-10-22 09:36:23 -0300
comments: true
categories: [Apple, CloudKit]
description: An idea for Apple to support CloudKit databases shared across members of your family sharing setup
keywords: "apple, cloudkit, family sharing"
published: true
---
CloudKit is the new syncing backend provided by Apple. Based on initial reviews and opinions, it appears to be great. I think, however, that there's a low hanging fruit for Apple to take advantage of: CloudKit for Family Sharing.

As it works now, CloudKit provides two kinds of storages: Public and Private. Public storage is a huge DB shared among all the users of your app. It appears to be tuned or thought for apps like Instagram, where there's a social graph and a large timeline.

The Private database is different: it works as part of each users iCloud storage and is limited in size by it.
This appears to be tuned to syncing across devices. It has great use cases, and several apps could benefit from it ([Vesper][^Sync] and [Things] come to mind).

Both options leave a slice of the sync market unattended: sharing _private_ data with other users. Think of apps like [Notabli], or our own [Shopster], where the most requested feature is by far syncing with other members of your household. 

Do you see a pattern yet? There's a big chance that several of the common sharing cases are among people in the same iCloud Family Sharing setup. Which brings me to the original idea: it would be great to have a third[^NotReallyThird] CloudKit database for Family Sharing. This one would be similar to the Private database and count against the organizer space quota, and ideally a new API would allow you to easily move data from your private cloud to the family shared one.

This third option could go a long way to address a common sync need that is currently unsolved by what Apple offers[^SharedDrive].

I've filed [a bug report](http://www.openradar.me/18735070) for this. If you think it may benefit you as well, feel free to use it as a reference.

[Vesper]: https://itunes.apple.com/us/app/vesper/id655895325?mt=8
[Things]: https://itunes.apple.com/us/app/things/id284971781?mt=8
[Notabli]: https://itunes.apple.com/us/app/notabli-childhood-archive/id580644870?mt=8
[Shopster]: https://itunes.apple.com/us/app/shopster-geo-learning-groceries/id613223118?mt=8

[^Sync]: it's no coincidence that anytime I think of syncing Vesper pops up. The entire blog series Brent did was a huge inspiration for me, which resulted in more frequent updates to this blog. If you haven't read it, [I highly recommend it](http://inessential.com/vespersyncdiary).

[^SharedDrive]: If Apple ever implements a feature like this, and given that iCloud Deive is claimed to be implemented on top of CloudKit, it wouldn't be far fetched to think a shared "family folder" could also be added to iCloud Drive.

[^NotReallyThird]: I don't know the implementation details, but I guess it's not really a third database, but flags on the actual `CKRecord`s. This way, if you decide or have to stop Family Sharing, your data could still be part of your Private database.