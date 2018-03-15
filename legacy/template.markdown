---
layout: post
title:  __TITLE__
date:   __DATE__
tags: [__TAG__]
---
Curly braces can be omitted when interpolated strings contain a reference to an instance, class, or global variable.

{% highlight ruby %}
@instance_variable = "Yo"
puts "#@instance_variable dawg"
"Yo dawg"
=> nil

@@class_variable = "Nam"
puts "Nuoc #@@class_variable"
"Nuoc Nam"
=> nil

$global_variable = "Atlas"
puts "#$global_variable will carry you noobs!"
"Atlas will carry you noobs!"
=> nil
{% endhighlight %}
