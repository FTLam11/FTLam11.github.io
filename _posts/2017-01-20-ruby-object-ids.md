---
layout: post
title:  "Ruby Object Ids"
date:   2016-1-20
tags: [ruby]
---
I assumed Fixnums in Ruby had the same object_id as their value. However, check this out:

{% highlight ruby %}
(1..10).to_a.map { |num| num.object_id }
=> [3, 5, 7, 9, 11, 13, 15, 17, 19, 21] # result = 2N + 1
{% endhighlight %}

Explanation: The Ruby interpreter and some of the core libraries are written in C. Fixnums have their bit representation shifted 1 position to the left, while assigning the lowest bit as 1 to designate that the object is a Fixnum.

```
0101 => 1011 # The object_id for 5 is 11.
0011 => 0111 # The object_id for 3 is 7.
0000 => 0001 # The object_id for 0 is 1.
```