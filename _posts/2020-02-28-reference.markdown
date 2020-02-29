---
layout: post
title: 'References! References everywhere! Or Why Is My String Modified?'
date: 2020-02-28
tags:
  - ruby
keywords:
  - ruby strings references
  - references ruby
  - ruby object
---

I have a class and I have a test. And everything is great until I add next test...

So, I had something like this:

<!--more-->

```ruby
class Formatter
  class << self

    TITLE = 'TEMPLATE TITLE'

    def format(records)
      output = TITLE # Initializing?

      records.each do |record|
        output << format_string_from(record) # Modifing `output` only?
      end

      output # Strange behaviour?
    end
  end
end
```

Looks pretty harmless from the first glance. But then you see awkward behaviour: output gets concatenated with previous calls of the `format` method.. why???

Let's try some `irb`ing

```ruby
2.7.0 :002 > string.object_id
 => 180
2.7.0 :003 > another_string = string
2.7.0 :004 > another_string
 => "I am a string. "
2.7.0 :005 > another_string.object_id
 => 180
 2.7.0 :006 > another_string << "And now I am modifying the original string"
 => "I am a string. And now I am modifying the original string"
2.7.0 :007 > another_string.object_id
 => 180
2.7.0 :008 > string
 => "I am a string. And now I am modifying the original string"
2.7.0 :009 >
```

So as you can see `another_string = string` looks like you are initializing `another_string`, but you actually
got another reference to the same string, and any modification of the value of `another_string` will be reflected in `string` because they are references for the same string.
You can modify string with `<<` or `[]`.

In case if you are reassigning (creating new string) you will receive new `object_id` and in that case old variable `string` will be unchanged

```ruby
2.7.0 :025 > string = "I am a string"
2.7.0 :026 > another_string = string
2.7.0 :027 > string.object_id
 => 240
2.7.0 :028 > another_string.object_id
 => 240
2.7.0 :029 > another_string = "New"
2.7.0 :030 > another_string.object_id
 => 260
2.7.0 :031 > string.object_id
 => 240
2.7.0 :032 > string
 => "I am a string"
```

So as you can see from the example above, after reassignment of `another_string` it received new `object_id` 260 and old `string` stayed 240

Same applies to any object in Ruby, i.e. arrays, hashes.

And yeah, I should've used `TITLE = 'Text'.freeze` for my constant 😅

Have a great weekend ahead!
