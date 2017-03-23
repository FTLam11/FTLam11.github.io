---
layout: post
title:  "Ruby Blocks"
date:   2016-12-28
tags: [ruby]
---
I read about different ways to use blocks in Ruby aside from passing them to enumerators for iteration. Here, a block is used to set a bunch of different attributes for a hero. 

{% highlight ruby %}
class Hero
  attr_accessor :title, :abilities, :stats

  def initialize(name)
    @name = name
    yielf self if block_given?
  end
end

riki = Hero.new("Riki") do |hero|
  hero.title = "Stealth Assassin"
  hero.abilities = ["Cloud", "Blink Strike", "Backstab", "Tricks of the Trade"]
  hero.stats = [17, 34, 14]
end
{% endhighlight %}