---
layout: theme:post
title: "Praising Synology's Customer Support"
date: 2014-02-20 06:52:51 -0300
comments: true
categories: [Hardware]
description: How Synology's customer support logged in remotely to my DiskStation to fix a rare problem.
---
A few months ago, after hearing Synology's products being discussed at [ATP] and researching a bit, I decided to give it a try. 

As I wasn't sure how was it going to work for me, I got a [DS213j][][^ChooseWisely] and (rookie mistake) only put inside a _single_ [Western Digital Red] hard drive. On February 18th, I woke up to find my DiskStation was not showing its volume and any of its shares.

While most of the information I have there is (yet) not critical, I am using a combination of [Photo Station] and [DS Photo+] as a Photo Stream replacement, so this crash found me with some pictures and videos of my kids having its sole copy on the DiskStation. 
I Googled a little bit, and even tried some terminal commands on the DiskStation, but before breaking something, decided to contact Synology's support to see how they could help me.

I opened a ticket around 7:30AM, and a few hours later, by noon, I got an answer asking me for the admin password and SSH access. An hour after I had answered, my issue was solved and the volume was mounted back. Amazing.

The only thing I would have liked more, is to know exactly what the technician did, so I could reproduce it should it fail again in the future, but since [BusyBox] lacks a history command, I can't retrace his steps.

---

I still don't know what happened, but my hard drive is showing a SMART error, so I'll ask for a [RMA] as soon as a new disk arrives so I can mirror the data (and when I get a replacement drive back, I'll have both set up in a RAID 1 arrangement).

## Lessons learned

* Synology support is amazing, contact them if you run into any issue.
* Always configure your DiskStation with RAID and have some sort of redundancy. I was always planning on doing it, but I started storing crititcal information before I did.

[ATP]: http://atp.fm/episodes/29-computerized-garden-gnome
[DS213j]: http://www.amazon.com/gp/product/B00CRB9CK4/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=B00CRB9CK4&linkCode=as2&tag=pablinorg-20
[Western Digital Red]: http://www.amazon.com/gp/product/B00EHBERSE/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=B00EHBERSE&linkCode=as2&tag=pablinorg-20
[Photo Station]: http://www.synology.com/en-us/dsm/home_multimedia_photo_station
[DS Photo+]: https://itunes.apple.com/us/app/ds-photo+/id321493106?mt=8
[RMA]: http://en.wikipedia.org/wiki/Return_merchandise_authorization
[^ChooseWisely]: Now that I've used it for a few months, I think I made a mistake with my decision. A NAS is a longer term investment than an external HD enclosure, so if you can affort it, choose one that's more expandable (4 bays instead of 2), and has more features. Mine is lacking front USB and SD ports (which I might have used a few times) and doesn't have the ability to run [Plex Media Server].
[Plex Media Server]: http://www.synology.com/en-global/company/news/article/340
[BusyBox]: http://en.wikipedia.org/wiki/BusyBox