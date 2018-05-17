---
layout: post
title:  Messin with Method Missin
date:   2018-05-17
tags: [ruby]
---
Ruh roh, check this snippet inspired by Metaprogramming Ruby:

{% highlight ruby %}

class DungeonDice
  def method_missing(method, *args)
    puts 'roll dat die yo'

    5.times do
      number = rand(6) + 1
      puts "spin...spin...spin...#{number}"
    end

    puts "landed on...#{number}"
  end
end

dice_roll = DungeonDice.new
puts dice_roll.yugioh
{% endhighlight %}

Nice juicy `stack level too deep (SystemStackError)`. What's happenin?

Since `number` is defined within the block, it is out of scope once the
last `puts` statement tries to reference it. Ruby interprets `number` to
then be a method call on `self`, and since `DungeonDice` does not have a
method called `number`, it calls `method_missing`! Dice ROLL!
