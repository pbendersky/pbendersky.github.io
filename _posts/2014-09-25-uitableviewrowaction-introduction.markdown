---
title: "UITableViewRowAction Introduction"
date: 2014-09-25 11:06:38 -0300
comments: true
categories: [iOS, Development]
description: Introduction to UITableViewRowAction for swiping on UITableViewCell
keywords: "ios, development, uitableviewrowaction"
published: true
---
iOS 7 saw the introduction of a new style for the swipe to delete in table view cells. The entire cell content was placed in a `UIScrollView`, and swiping would reveal the red Delete button. iOS Mail (and only that app) also sported an additional More menu item. This API, however was private.

In iOS 8, Apple finally made this API public [^ButKeptMorePrivate] for all of us to use, in the form of edit actions and `UITableViewRowAction`.

## Usage

In order to provide your `UITableViewCell`s with actions, you need to implement the method `func tableView(tableView: UITableView, editActionsForRowAtIndexPath indexPath: NSIndexPath) -> [AnyObject]?` of `UITableViewDelegate`.

The method retuns an _array_ of actions. The order is of course important: the first item in the Array will be the rightmost (or leftmost on RTL user interfaces) item when you swipe the cell.

A sample implementation follows:

{% highlight swift %}
    override func tableView(tableView: UITableView, editActionsForRowAtIndexPath indexPath: NSIndexPath) -> [AnyObject]? {

        let deleteClosure = { (action: UITableViewRowAction!, indexPath: NSIndexPath!) -> Void in
            println("Delete closure called")
        }
        
        let moreClosure = { (action: UITableViewRowAction!, indexPath: NSIndexPath!) -> Void in
            println("More closure called")
        }
        
        let deleteAction = UITableViewRowAction(style: .Default, title: "Delete", handler: deleteClosure)
        let moreAction = UITableViewRowAction(style: .Normal, title: "More", handler: moreClosure)
        
        return [deleteAction, moreAction]
    }
    
    override func tableView(tableView: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath) {
        
        // Intentionally blank. Required to use UITableViewRowActions
    }
{% endhighlight %}

This is how it looks when you swipe:

{% include image.html url="/assets/images/2014-09-25/UITableRowActionSample@2x.png" width="320" height="129" description="UITableViewRowAction Sample" %}

As you can see you need to return an array of `UITableViewRowAction` objects. `UITableViewRowAction.init` receives three parameters:

- `style`: either `.Normal` or `.Default`. `.Normal` doesn't have a color, similar to the `More` button in iOS 7 Mail, and `.Default` is the standard destructive action, with the red background color.
- `title`: the label that will be shown to the user when swiping the row.
- `handler`: a closure (or block) with the handler that will be called when the action is selected by the user. The handler receives two parameters: the `action` itself, and the `indexPath`. Sadly, the handler doesn't give you the `UITableView` as a parameter, so your handler needs to get a reference to that by other means.

As you may have noticed it in the sample code above: in addition to `tableView:editActionsForRowAtIndexPath:` you need to override `tableView:commitEditingStyle:forRowAtIndexPath:`, even though you can leave it blank. If the method is not present, the actions won't show up on swipe[^DocumentEmptyMethods].

In case you find it useful, you can [download an interactive playground](/assets/code/2014-09-25/UITableViewRowActionIntroduction.playground.zip) with this sample code.

[^ButKeptMorePrivate]: Interestingly, in iOS 8, Apple is again testing more `UITableViewCell` actions, like swiping _across_ the entire row to trigger an action, or swipe from both sides to reveal different options. Additionally, it appears iOS 8 private API also allows you to customize the background colors. I'm looking forward to iOS 9 (or 8.1) making these API public.
[^DocumentEmptyMethods]: As you can see in my sample code, this method has a comment indicating it's _intentionally_ blank. Any time you need an empty method implementation leave a comment, as these methods will be the first you (or other teammate) will target for removal when refactoring code.