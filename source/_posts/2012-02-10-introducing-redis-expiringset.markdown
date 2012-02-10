---
layout: post
title: "Introducing Redis::ExpiringSet"
date: 2012-02-10 11:23
comments: true
categories: 
---
Yesterday I released a new gem: [Redis::ExpiringSet][res-gh]. It creates a new
datatype for use with Redis - an Expiring Set.

#### What's an Expiring Set and why should I care?
Expiring Sets behave exactly like normal Sets in Redis, with one exception: each
item in the set has an expiration time, and once that expiration time is past
the item will be removed from the set. This is particularly useful for keeping
track of recent items.

Suppose, to create a contrived example, you want to keep statistics on the
number and type of emails you've sent from your site in the last 24 hours.
Suppose that this data has a string representation. Every time you send an email
you can just put that data into an expiring set along with an expiration time:

``` ruby
require 'redis-expiring-set'

# assume @redis is your redis database connection,
# and @email is your email job
expset = Redis::ExpiringSet.new(@redis)

expset.xadd("email_data", @email.to_s, Time.now + 1.day)
```

Now if you want to access your data, you can just call xmembers, and you'll be
guaranteed to only get info from the past 24 hours:

``` ruby
expset.xmembers("email_data")
```

There's also a "monkeypatch" file you can include to make all of this available
directly off of your Redis objects:

``` ruby
# again, assume @redis is your redis database connection
require 'redis-expiring-set/monkeypatch'

@redis.xadd("email_data", @email.to_s, Time.now + 1.day)

@redis.xmembers("email_data")
```

This isn't the default behavior because I didn't want to force you to
monkeypatch the Redis library if you didn't want to.

More details and documentation are available at the [github page][res-gh].


[res-gh]: http://github.com/duwanis/redis-expiring-set "redis-expiring-set on duwanis's github profile"
