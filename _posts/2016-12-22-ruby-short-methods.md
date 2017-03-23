---
layout: post
title:  "Ruby Short Methods"
date:   2016-12-22
tags: [ruby]
---
As always, everything in moderation, including moderation. Rubyists generally prefer methods that:

1. Do one thing
2. Call lower-level methods to handle the nitty-gritty implementation details
3. Have descriptive names

Using an exercise from [exercism.io](http://exercism.io/exercises/ruby/queen-attack/readme), here is some code demonstrating these guidelines. 

{% highlight ruby %}
class Queens
  attr_reader :position

  def initialize(position)
    raise ArgumentError unless valid_position?(position)
    @position = position
  end

  def attack?
    same_rank? || same_file? || same_diagonals?
  end

  private

  def valid_position?(position)
    valid_rank?(position) && valid_file?(position)
  end

  def valid_rank?(position)
    position.values.all? { |position| on_board?(position) }
  end

  def valid_file?(position)
    position.values.transpose.all? { |position| on_board?(position) }
  end

  def on_board?(position)
    position.first > -1 && position.first < 8
  end

  def same_rank?
    position[:white].first == position[:black].first
  end

  def same_file?
    position[:white].last == position[:black].last
  end

  def same_diagonals?
    rank_delta == file_delta
  end

  def rank_delta
    (position[:white].first - position[:black].first).abs
  end

  def file_delta
    (position[:white].last - position[:black].last).abs
  end
end
{% endhighlight %}