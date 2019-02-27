---
layout: post
title:  Printing to stdout
date:   2019-02-27
tags: [ruby]
---
I was sadly mistakened about `p` being an alias for `print` while trying
to use the *output_to_stdout* matcher for a spec.

{% highlight ruby %}
class RubyClass end;

describe RubyClass do
  it 'keeps a reference to self' do
    block = lambda {
      class RubyClass
        p self
      end
    }

    expect { block.call }.to output('RubyClass').to_stdout
  end
end
{% endhighlight %}

Using `p` and running the spec results in `"RubyClass\n"`. I know `p`
uses `inspect` instead of `to_s`, which is super helpful when working
with assorted data structures in console. I just never noticed `p`
always adding a newline.

In contrast, `print` uses `to_s` and does not add a newline, so really
completely different behavior from `p`.

In short, `print` is `puts` without the newline. They both return `nil`
since they call `to_s`. `p` adds a newline, but calls `inspect` instead.
