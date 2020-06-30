---
layout: post
title: 'Create nested arrays in Ruby'
date: 2020-06-30
tags:
  - ruby
keywords:
  - ruby matrix
  - ruby 2d array
  - ruby nested arrays
dont_show_excerpt_separator: false
---

I needed to create a nested array(2d array, matrix). Usually, I do:

<!--more-->

```ruby
array = []
#or
array = Array.new
```

More from official doc [here](https://apidock.com/ruby/Array)

And it usually works, because, often I don't need nested arrays, I initiate array and I can start doing #push, #[], etc.

But the problem is when you need to create

```ruby
array = [[]]
```

I expected that this is enough for adding as many arrays to outer array as i want. To my surprise(again 🤦‍♀), when I wanted to add second element, I saw `NoMethodError`:

```
2.7.0 :003 > array=[]
2.7.0 :004 > array[0]=1
2.7.0 :005 > array[2]=3        <- dynamically can add elements
2.7.0 :006 > array
 => [1, nil, 3]
2.7.0 :007 > array2d=[[]]
2.7.0 :008 > array2d[0][0] = 0
2.7.0 :009 > array2d[0][1] = 0
2.7.0 :010 > array2d
 => [[0, 0]]
2.7.0 :011 > array2d[1][0] = 0            <- fail to add second array, obviously, because it doesnt exist
Traceback (most recent call last):
        4: from .rvm/rubies/ruby-2.7.0/bin/irb:23:in `<main>'
        3: from .rvm/rubies/ruby-2.7.0/bin/irb:23:in `load'
        2: from .rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/irb-1.2.1/exe/irb:11:in `<top (required)>'
        1: from (irb):11
NoMethodError (undefined method `[]=' for nil:NilClass)
2.7.0 :012 >
```

We failed to add second array, obviously, because it doesn't exist. `array2d` is `[[]]`, which means when you call `array[1]` it will return nil, and on `nil` I was trying to call `[]`. And that's why `NoMethodError`

Ok, so we know the problem. I need to initiate array of arrays with right amount of arrays inside. [Official doc](https://apidock.com/ruby/Array) shows how it can be done:

```ruby
Array.new(3) { Array.new(3) }  # please, see the block
Array.new(3, Array.new(3)) # no block was passed
```

what happens here, let's break down:

```
2.7.0 :012 > a = Array.new(3) { Array.new(3) }              # block is given
2.7.0 :013 > a
 => [[nil, nil, nil], [nil, nil, nil], [nil, nil, nil]]
2.7.0 :014 > a[0][1]=2
2.7.0 :015 > a
 => [[nil, 2, nil], [nil, nil, nil], [nil, nil, nil]]      # as expected

2.7.0 :016 > b=Array.new(3, Array.new(3))                  # default parameter is given
2.7.0 :017 > b
 => [[nil, nil, nil], [nil, nil, nil], [nil, nil, nil]]
2.7.0 :018 > b[0][1]=2
2.7.0 :019 > b
 => [[nil, 2, nil], [nil, 2, nil], [nil, 2, nil]]          # HA, and here we passed by reference
```

As you can see, b was array that was created with default parameter, and array is passed by reference. Which means `b=Array.new(3, Array.new(5))`, `Array.new(5)` was evaluated once, and same object was passed to each element of outer element. To prove the point let's check `object_id`s:

```
# a is created with block given a = Array.new(3) { Array.new(3) }
2.7.0 :020 > a.map(&:object_id)
 => [180, 200, 220]

# b is created with default value b=Array.new(3, Array.new(3))
2.7.0 :023 > b.map(&:object_id)
 => [240, 240, 240]
2.7.0 :024 >
```

From official docs:

<blockquote>
<p>Note that the second argument populates the array with references to the same object. Therefore, it is only recommended in cases when you need to instantiate arrays with natively immutable objects such as Symbols, numbers, true or false.
</p>
<p>
To create an array with separate objects a block can be passed instead. This method is safe to use with mutable objects such as hashes, strings or other arrays:
</p>
</blockquote>

Alternatively, I could create outer array, and then use `#push` for pre-created internal arrays:

```ruby
result = []
internal_array = [1,2,3,4]
result.push(internal_array)
internal_array = [2,3,1,2]
result.push(internal_array)
```

(repetition here just to show the point)

results:

```
2.7.0 :026 > result = []
2.7.0 :027 >
2.7.0 :028 > internal_array = [1,2,3,4]
2.7.0 :029 > result.push(internal_array)
 => [[1, 2, 3, 4]]
2.7.0 :030 > internal_array = [2,3,1,2]
2.7.0 :031 > result
 => [[1, 2, 3, 4]]
2.7.0 :032 > result.push(internal_array)
 => [[1, 2, 3, 4], [2, 3, 1, 2]]
2.7.0 :033 >
```

In addition to arrays, you may want to check about strings too: [references in ruby]({% post_url 2020-02-28-reference%})
