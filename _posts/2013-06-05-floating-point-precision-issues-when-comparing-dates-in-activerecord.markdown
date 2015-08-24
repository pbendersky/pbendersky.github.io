---
layout: theme:post
title: "Floating point precision issues when comparing dates in ActiveRecord"
date: 2013-06-05 16:14
comments: true
categories: [Development, Rails]
description: "A workaround for floating point precision issues when comparing dates in Rails (ActiveRecord)"
keywords: "rails, floating point, activerecord, precision"
---
I just run into a weird issue when comparing dates in Rails. In years of development, I think it's one
of the few times I've run accross floating point precission issues.

<!-- more -->

For a syncing engine with an iOS app, I'm looking for records on a Rails app where their `updated_at` field
is newer than a parameter I'm sending.

My original code, in the controller, looked like this:
``` ruby Original Controller Code
def index
  if params[:since]
    timestamp = Time.zone.at(params[:since]))
    @products = Product.where("updated_at > ?", timestamp)
  else
    @products = Product.all
  end

  respond_to do |format|
    format.html # index.html.erb
    format.json { render json: @products }
  end
end
```

and I was calling this with the following URL: `/products.json?since=1370362746.67886`.

The date corresponded to the last `updated_at` date on the database, so it should have not returned any
object. To my surprise, I was still getting an object. Here's the `irb` session:
``` ruby Original Code irb Session
1.9.3p194 :001 > ts = Time.zone.at(1370362746.67886)
 => Tue, 04 Jun 2013 16:19:06 UTC +00:00 
1.9.3p194 :002 > Product.where("updated_at > ?", ts)
  Product Load (0.2ms)  SELECT "products".* FROM "products" WHERE (updated_at > '2013-06-04 16:19:06.678859')
 => [#<Product id: 5, created_at: "2013-06-04 16:19:06", updated_at: "2013-06-04 16:19:06", name: "E", deleted: false, global_uuid: "B6ABA6EF-30FA-4728-A304-C39D232BBB51">] 
1.9.3p194 :003 > 
```

If you check the query that is being generated, you'll see that the date is sent as
`2013-06-04 16:19:06.678859`, even though the original timestamp is `1370362746.67886`.

What's going on then? Of course, [floating point](https://en.wikipedia.org/wiki/Floating_point)
precission issues!

Check this `irb` session:
``` ruby Floating point test irb session
1.9.3p194 :001 > Time.zone.at(1370362746.67886).to_r
 => (5747717949846129/4194304) 
1.9.3p194 :002 > Product.last.updated_at.to_f
  Product Load (0.2ms)  SELECT "products".* FROM "products" ORDER BY "products"."id" DESC LIMIT 1
 => 1370362746.67886 
1.9.3p194 :003 > Product.last.updated_at.to_r
  Product Load (0.3ms)  SELECT "products".* FROM "products" ORDER BY "products"."id" DESC LIMIT 1
 => (68518137333943/50000) 
1.9.3p194 :004 > 
```
As you can see, the float representation of the last `updated_at` value is the same I'm sending. However,
when using `to_r` to create a `Rational`, you can see the two numbers are slightly different.

## Workaround
The workaround I settled for is to use `Rational` instead of `floats` when first converting the floating
point representation to a date.

Here's the current version of the code:
``` ruby Fixed Controller Code
def index
  if params[:since]
    timestamp = Time.zone.at(Rational("#{params[:since]}"))
    @products = Product.where("updated_at > ?", timestamp)
  else
    @products = Product.all
  end

  respond_to do |format|
    format.html # index.html.erb
    format.json { render json: @products }
  end
end
```

And here's an `irb` session showing it working as expected:
``` ruby Fixed Code irb Session
1.9.3p194 :001 > ts = Time.zone.at(Rational("1370362746.67886"))
 => Tue, 04 Jun 2013 16:19:06 UTC +00:00 
1.9.3p194 :002 > Product.where("updated_at > ?", ts)
  Product Load (0.2ms)  SELECT "products".* FROM "products" WHERE (updated_at > '2013-06-04 16:19:06.678860')
 => [] 
1.9.3p194 :003 > 
```

It's very important to note that the parameter you send to `Rational` has to be a `String` and not a
number, otherwise it will be coalesced into a `float` and lose precision before creating
the new `Rational` instance.