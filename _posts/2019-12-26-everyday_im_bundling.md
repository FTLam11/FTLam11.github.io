---
layout: post
title:  Everyday I'm bundling
date:   2019-12-26
tags: [ruby, docker, unix, devops]
---

I decided to try a slightly different approach with bundler for
production, trying my hand at packing the projects gems to
`vendor/cache`. I used [this
article](http://ryan.mcgeary.org/2011/02/09/vendor-everything-still-applies/)
as a reference point. It's a bit old, but still applicable.

Using `bundle package` to package gems into `vendor/cache` trades
repository file space for quicker bundling. I did run into a couple
issues with trying to handle platform-specific gems like
`google-protobuf`. I found other folks were have similar
[issues](https://github.com/bundler/bundler/issues/7076).

I generally develop without using Docker, so this was an opportunity to
learn about this conundrum. Docker for macOS uses the `x86_64-linux`
platform, so when Docker failed to bundle successfully, I saw that
my gem sources was missing the `google-protobuf` gem. Looking further
into it, I had to enable a couple options for `bundle config`:

```
BUNDLE_CACHE_ALL: "true"
BUNDLE_CACHE_ALL_PLATFORMS: "true"
BUNDLE_SPECIFIC_PLATFORM: "true"
```

These flags ensure that `vendor/cache` contained **all** the necessary
gems. I had to add the `x86_64-linux` platform `Gemfile.lock` to ensure
`bundle package` would pick it up.  Bundler's `bundle config` [docs](https://bundler.io/v2.0/man/bundle-config.1.html)
lists all available flags and provides description. Running `bundle
install` with the deployment flag installs gems to `vendor/bundle`
instead of the default system location (though I don't know how much
this matters with Docker, since each container is self-contained HAHA).
The local flag is interesting since bundler will install gems directly
using `vendor/cache` as the gem source, avoiding gem downloads from
RubyGems, GitHub, etc...

I ended up with the following production Dockerfile. The last step is
overridden with kubernetes. Builds are super fast, I've never been this
excited about our CI/CD pipeline! The image takes ~4 minutes to build,
and that's with the Docker cache being busted early on. Most builds should
be able to use layer cache up until the `COPY . /usr/src/app` step.
Zaaaaaaaaaaaaangggggggg.

```
FROM ruby:2.6.4

LABEL maintainer='frank@larvata.tw'

RUN apt-get update -yqq && apt-get install -yqq --no-install-recommends \
  tzdata \
  libsodium23

COPY Gemfile* /usr/src/app/
COPY vendor/cache /usr/src/app/vendor/cache
WORKDIR /usr/src/app
RUN bundle install --deployment --local --without development test
COPY . /usr/src/app/
EXPOSE 50051

CMD ["bin/rails", "s"]
```
