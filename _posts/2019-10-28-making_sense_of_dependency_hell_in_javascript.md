---
layout: post
title:  Making sense of dependency hell in JavaScript
date:   2019-10-28
tags: [javascript]
---
This is kinda a continuation of my previous post about diving through a
legacy Rails project and attempting to modernize how the front-end
assets are managed. When I began my first job as a developer, all
JavaScript was written using spaghetti jQuery and it mostly got the job
done.

As I began working on custom ERPs, the front-end complexity became
painful. I did my best and leveraged OOJS, handlebars.js, and more
spaghetti jQuery to write a really shitty data store and avoid full page
reloads.

I never spared a thought to what dependencies were required for any
given page. I naively called functions from any given library, because
everything was expected to be available globally. Sprockets didn't
really help in enforcing modular JavaScript, since its directives simply
appended file contents. To be frank, many pages were just monolith
JavaScript modules with no clear depiction of dependencies. Adding new
libraries was EZ-PZ-LEMON-SQUEE-EZ, just throw it in `application.js`,
it was never a concern to consider which pages a library was actually
required on. Throw a `require` or `require_tree` in and call it a day.

Fast forward a bit, I started reading up on modern JavaScript,
struggling with Webpack, and learning Vue. I came across a great article
about how modules evolved throughout the history of JavaScript, it's
super good, cheggit [here](https://tylermcginnis.com/javascript-modules-iifes-commonjs-esmodules/).
I got a much better understanding of where the legacy project stood in
terms of modularity, as well insight into `require/import` and
`module.exports/export` given via CommonJS and ES6 modules. Key points:

* CommonJS is a great step towards modularizing JavaScript, no longer
are we simply "modularizing" by splitting code into separate files nor
are we abusing globals
* There are two glaring cons with CommonJS: web browsers do not
understand CommonJS modules, and module loading is done synchronously
* Module bundlers are able to address the issue of web browsers not
being compatible with CommonJS modules, and [AMD](https://requirejs.org/docs/whyamd.html)
seems to be an option in solving the synchronous loading problem
* ES6 modules are supported by modern web browsers through setting the
`type` attribute of script tags to `module`
* ES6 modules can forego the module bundling by simply including the
"main" script on the page since imported dependencies are explicit

Modules seem to be an overlooked topic, science be praised. My sanity
was almost gone while upgrading the dreaded legacy project, but I feel a
little bit better and more optimistic now.
