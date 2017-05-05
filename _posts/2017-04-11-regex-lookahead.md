---
layout: post
title:  "Lookahead regex"
date:   2017-04-11
tags: [javascript, ruby]
---
Had a situation where I wanted to split a guitar tuning string into an array of individual notes.

{% highlight javascript %}

var algernon = "DAEAC#E";

var algernonNotes = algernon.split(/([A-G][#b]?)/); // [ '', 'D', '', 'A', '', 'E', '', 'A', '', 'C#', '', 'E', '' ]

{% endhighlight %}

Well, cb diam la. I don't want the empty strings. I found two ways of addressing this conundrum:

{% highlight javascript %}

var algernon = "DAEAC#E";

var algernonNotes = algernon.split(/(?=[A-G][#b]?)/); // [ 'D', 'A', 'E', 'A', 'C#', 'E' ]
var algernonNotes = algernon.split(/([A-G][#b]?)/).filter(Boolean); // [ 'D', 'A', 'E', 'A', 'C#', 'E' ]

{% endhighlight %}

So the first approach uses a Lookahead regex, while the second approach calls `filter()` to remove the empty strings since they evaluate to `false`. Gonna have to look into this lookahead stuff. And the Ruby equivalent:

{% highlight ruby %}

var algernon = "DAEAC#E";

var algernonNotes = algernon.split(/(?=[A-G][#b]?)/); // [ 'D', 'A', 'E', 'A', 'C#', 'E' ]
var algernonNotes = algernon.split(/([A-G][#b]?)/).reject(&:empty?); // [ 'D', 'A', 'E', 'A', 'C#', 'E' ]

{% endhighlight %}