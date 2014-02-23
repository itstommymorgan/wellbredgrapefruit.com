---
layout: post
title: "Introducing Nutrasuite"
date: 2012-02-18 22:45
comments: true
categories: 
---
Just a quick note about a little minitest-enhancing library that [Alan Johnson][commondream] and I hacked up today. [Nutrasuite][nutrasuite] is a dirt-simple gem
that adds contexts to minitest for you. It might do more in the future, it
might not. But for now you can get organization in your tests as easy as:
<!-- more -->
``` ruby
require 'nutrasuite'
class SomeTest < MiniTest::Unit::TestCase
  a "newly instantiated test object" do
    setup do
      some_setup_stuff
    end

    it "tests for stuff" do
      assert true
    end

    that "has some other setup stuff done" do
      setup do
        some_other_setup_stuff
      end

      it "tests for more stuff" do
        assert true
      end
    end

    teardown do
      some_teardown_stuff
    end
  end
end
```

A few notes:

* `a`, `an`, and `that` all define contexts. Between those three you should be
  able to put together grammatically-correct sounding tests.
* `it` defines a new test.
* Setups and teardowns are run for each test. This means that you can
  safely take advantage of minitest's natural test randomization.
* If you care about LOC (I don't, really, but it kind of speaks to the
  simplicity) this stuff is set up in 122 lines, and doesn't monkeypatch
  anything or pollute the kernel namespace. Hard to get much simpler than that.

So nothing too special, but we found a need for it and put it together (in
fact, if you're curious, it's already in use over on Timberline, which I
haven't announced yet). And by "we put it together" I mean Alan figured out how
it was going to work and I filled in the code and came up with a quirky name
(p.s. Nutrasweet - please don't sue me. You have better things to do).

[commondream]: http://www.commondream.net
[nutrasuite]: http://www.github.com/duwanis/nutrasuite
[timberline]: http://www.github.com/duwanis/timberline
