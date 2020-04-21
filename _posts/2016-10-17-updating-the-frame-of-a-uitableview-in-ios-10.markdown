---
title: "Updating the Frame of a UITableView in iOS 10"
date: 2016-10-17T11:07:33-03:00
---

In Shopster, when you edit an element in the main `UITableView`, we show a "ruler" to allow for selecting the item quantity. When the ruler is shown, we make the table view narrower, so the text is still visible to the user.

In iOS 10, however, Apple slightly changed the behaviour of `UITableView`, and the animation started failing as follows:

{% include video.html url="/assets/images/2016-10-17/UITableView-broken-animation.mov" width="360" height="342" %}

In the animation closure, we are simply changing the `UITableView`'s frame. As you can see on the video, this makes the animation "jump". The disclosure arrows move before the animation even start.

The solution, luckily, is simple: just wrap the frame update in a `beginUpdates` / `endUpdates` pair as follows:

{% highlight swift %}
self.tableView.beginUpdates
// assumes currentFrame is set previously.
self.tableView.frame = currentFrame
self.tableView.endUpdates()
{% endhighlight %}

If you want to play around with the example, [here's an interactive playground][playground] you can download to reproduce the issue or see the workaround.

[playground]: /assets/code/2016-10-17/UITableView-Resize.playground.zip