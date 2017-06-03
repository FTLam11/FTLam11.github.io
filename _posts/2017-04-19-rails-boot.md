---
layout: post
title:  "Exploring the Rails Magic"
date:   2017-04-19
tags: [ruby]
---
Time to slowly decrypt dat Rails magic, beginning with startup and configuration settings.

## The primary suspects

Three files are responsible for setting up the entire Rails stack. Each preceding file is required by the one below it.

* `config/boot.rb` - This file sets up Bundler and load paths
* `config/application.rb` - This file loads Rails gems, gems for the specified environment, and configures the Rails app
* `config/environment.rb` - This file runs all initializers

## Initializers

The majority of the configuration settings are found in `config/initializers`, where nine default initializers (for Rails 5.1) reside. New initializer files can be created in this directory and will automatically be loaded at startup. I'd rationalize that it's much easier to reason about configuring the application by breaking down different responsibilities into separate files. Some of the more frequently visited initializers are probably `filter_parameter_logging.rb` to prevent sensitive request parameters like passwords/payment info from being logged, `wrap_parameters.rb`to more easily work with JavaScript MVC frameworks, `jbuilder.rb` to customize your API JSON responses.