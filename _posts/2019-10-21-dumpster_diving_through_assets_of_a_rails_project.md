---
layout: post
title:  Dumpster diving through assets of a Rails project
date:   2019-10-21
tags: [ruby, rails, webpacker]
---
It's time to hold dat big phat "L". I volunteered to investigate why one
of our Rails projects was taking forever (45+ minutes with occasional
timeouts in our CI/CD pipeline). I had my suspicions of which build job
was taking the longest and confirmed primary suspect numero uno: the
infamous asset pipeline. I'm not blaming the tool, because I also knew
no one on the team (including myself) had a solid understanding of our
assets/front-end dependendies for the project.

I'm gonna document as many anti-patterns as I find and what corrective
actions I plan on taking to "optimize" management of all the JavaScript,
CSS, fonts, images, etc... Lord have mercy on our souls. Roll up them
sleeves, it's time to go deep in dumpster!

# Ambiguous layouts

The first offender I found were two nearly identical layout files:
`application.html.erb` and `admin.html.erb`. Given this project is a
custom construction project management application, it is mainly meant
for internal use.

In addition the layout `head` contains CDN links for fonts, stylesheets, and
several JavaScript libraries. These should be included in the asset
pipeline. In the case where the application with is hosted locally in a place
without an internet connection, it's a real bad look.

# Vendor JavaScript littered across multiple directories

God fucking damn it:

* `app/assets/javascripts` is not the place to put vendor JavaScript
files
* The vast majority of assets I found in `vendor` can now be installed
from npm. Instead of manually copying specific versions of files into
`vendor`, we can just use `package.json` to cleanly manage these
dependencies. Sadly, there are some files that I can't easily match up
through searches.

# Gems solely used to bundle JavaScript and related assets

I guess I can rationalize and understand the original motivation for these
gems. Before the Rails modernized its front-end assets management, lots
of people relied on gems that convenient bundled popular JavaScript
libraries. Updating versions would be abstracted through updating gem
versions, allowing the majority of Rails developers to interact solely
through the bundler API. It was also super easy to just throw a couple
`require` statements to load each library developers needed. Sprockets
did the heavy lifting of finding each dependency, with a little help
from configuring asset paths.

But with adoption of webpack, npm, yarn, and other tools from the
front-end ecosystem, it seems time to pass the handling of preparing the
front-end assets to the new overlords. I'd estimate
`webpacker`-compatible projects would gain pretty substantial
improvements during precompilation.

Gemfiles would also become less bloated, allowing applications to boot
faster! Can't hate on that! The application I'm working on upgrading has
~15-20 of these gems and has a pretty awful load time (without spring
and bootsnap).

# javascript_include_tag littered across layouts

The purpose of Rails layouts is to provide a base template for views to
use. Why not include all these rogue `javascript_include_tag`s inside
the `application.js` manifest? I'm furious. ANGER LEVEL > 9000!!!! The
individual JavaScript files aren't even business logic files, they are
just libraries! MRGRGRGRGR...

# Global scope pollution

I came across a shit load of custom "utility" JavaScript files that do not
export anything. Because the sprockets `require` directive merely
appends the contents of a target file, it is super easy to call random
functions and reference variables wherever anyone wants. Essentially
everything shares the scope, and things aren't isolated. It's convenient
and we can just write braindead jQuery shit without ever thinking about
what actual dependencies are needed/where? Lord have mercy, get me out
of this fucking game.

# Mystery non-kosher vendor themes

I don't know who decided to buy (or steal) the analog admin theme...
hell, this shit ain't even worth stealing. What a clusterfuck of CSS
files, images, fonts, and JavaScript bundles. Don't even think twice
when considering usage of undocumented mystery grab bag themes. This
shit is worse than cancer. STAY OFF THIS SHIT.

# Non-rails developers wutface?

Yeah, this has just turned into a rant. At times our Rails projects
receives help from dedicated front-end developers, and I'd wager that
they experience mind-numbing levels of shock after seeing how much
spaghetti JavaScript and having to dig out library versions from gem
documentation/source code instead of checking `package.json`.

# Closing

Let us embrace our modern front-end overlords:

> May we have discipline when choosing dependencies.

> May we actually understand our dependencies.

> May we have the basic mental capability to use these dependencies
> correctly.

Worst legacy project update experience ever. Kum-bay-fuckin-ya. LOL
