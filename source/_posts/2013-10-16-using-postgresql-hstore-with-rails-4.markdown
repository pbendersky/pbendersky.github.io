---
layout: post
title: "Using PostgreSQL hstore with Rails 4"
date: 2013-10-16 14:43
comments: true
categories: [development, rails]
description: How to use PostgreSQL hstore with Rails 4.
---
Rails 4 has native support for PostgreSQL [hstore](http://www.postgresql.org/docs/current/static/hstore.html).

Sadly, the [tutorials](http://railscasts.com/episodes/345-hstore) and documentation I could find online, only point to how to use them with Rails 3, and only mention there will be "native" support with Rails 4.

<!-- more -->

Luckily, using hstore in Rails 4 is as easy as it should be. Native support means *native*: nothing needs to be done.

While previously you would need something like:

    serialize :properties, ActiveRecord::Coders::Hstore

within your `ActiveRecord` subclass, now you just need to create the field with the appropriate migration

    class AddPropertiesToProduct < ActiveRecord::Migration
      def change
        add_column :products, :properties, :hstore
      end
    end

With this, the `Product` class will automatically serialize `properties` as to an `hstore` and present it to you as a Ruby `Hash`.

Two additional tips:

1. If you want the default value to be an empty hash, in the migration you need to specify it as `{}`.
2. I recommend using this [gist](https://gist.github.com/tehpeh/3735174) or something similar, to enable the `hstore` extension when doing a `rake db:schema:load`. Usually, you enable `hstore` in a migration, but during development it's very common to drop and recreate the database. Since `hstore` is now part of your schema, it will fail when you run `rake db:schema:load` as the migration hasn't run yet and `hstore` is not enabled. This gist will fix that.