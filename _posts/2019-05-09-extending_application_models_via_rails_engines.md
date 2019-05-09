---
layout: post
title:  Extending application models via Rails engines
date:   2019-05-09
tags: [ruby]
---

On the DL, been diggin in the crates for different ways to slice n dice
cleaner software architecture. I've spit a [simple gem to scrape bandcamp info](https://github.com/FTLam11/fronkin_bandcamp_scraper)
before, but you know what time is it. Enter the 36 chambers of Rails
engines...

> It is the unemotional, reserved, calm, detached warrior who wins, not
> the hothead seeking vengeance and not the ambitious seeker of fortune.

JK JK, Dame is cooooooold bloodddded tho.

No clownin', with an EZ-PZ-lemon-SQUEZ application you can probably
get away with just going ham at writing code and not spend much time up
front planning your approach. But for any moderately complex
application, it does help to chill a bit and reflect on past endeavors
and lessons learned.

I've been at the same software agency the past two years, and my team
has finally had the time/resources to invest in modularizing common
features present in a lot of different applications we've worked on. At
the start of this new year, I was very vocal and detailed about how I
felt our team should grow this year and improve the standards and
processes we abide by.

The Rails engine guide had a section for [overriding models and
controllers](https://guides.rubyonrails.org/engines.html#overriding-models-and-controllers).
However, I wanted to actually extend the host application's model from
the engine itself, not the other way around. The issue I ran into was
how to load files from the engine in the context of the host
application.

> Because these decorators are not referenced by your Rails application
> itself, Rails' autoloading system will not kick in and load your
> decorators. This means that you need to require them yourself.

This was a somewhat painful but enlightening journey into how Rails
loads the entire application, how the process differs by application
environment.

The snippet from the article loads decorators defined in the host
application to enhance engine models/controllers and uses
`ActiveSupport::Dependencies::Loadable.require_dependency` (thanks Pry!)
to load them. Those decorators could then extend the application
models/controllers via `class_eval`, modules, etc...

{% highlight ruby %}
config.to_prepare do
  Dir.glob(Rails.root + "app/decorators/**/*_decorator*.rb").each do |c|
    require_dependency(c)
  end
end
{% endhighlight %}

Sadly, I wouldn't be able to get the path to my engine's decorators.
I'd have to wrap the extra behavior into a module and decide how to
include it.

One possible approach was to continue leveraging
the mattr_accessors that targeted the host models and use `Kernel.send`
to include the module inside the `to_prepare` hook. Not sure how I feel
about this, it is very transparent from the host application
perspective, but I'd need to define mattr_accessor placeholders on the
engine side as well as setting up initializers on the host application
side to get the right includes.

[Another approach](https://stackoverflow.com/questions/36717540/extending-applications-model-in-rails-engine)
was to use the `Module.included` hook and explicitly declare these in the necessary host application models/controllers.
The first answer has a great explanation for this approach.

TLDR:

> “Victorious warriors win first and then go to war, while defeated
> warriors go to war first and then seek to win”
