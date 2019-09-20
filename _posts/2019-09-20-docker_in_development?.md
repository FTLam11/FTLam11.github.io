---
layout: post
title:  Docker in development?
date:   2019-09-20
tags: [docker]
---
I guess it was about time to try my hands at dockerizing a chat module
I've been hacking at the past few weeks. I never invested the time to
learn the basics and background behind Docker, despite often working
with/against it on plenty of occasions. There's just so many friggin
ways of dockerizing any given project; we often use the Phusion
Passenger base image for Ruby. It seems to be a good fit given the
sheer number of projects we have, and aids in standardizing the
deployment workflow while minimizing overhead. Our office does not use
Heroku BTW.

After spending a few days reading through [Docker for Rails
Developers](https://pragprog.com/book/ridocker/docker-for-rails-developers),
Docker documentation, and various blog posts I still feel somewhat
apprehensive about going full Docker in development. I felt much pain
as I suffered through the early stages of building images using a
Dockerfile, and then organizing all the services I needed via Docker
Compose. Hell, I haven't even gotten to production yet with the container
orchestration jazz.

I still consider myself a Docker noob, so maybe this rant means nothing
to seasoned Docker veterans. The learning experience thus far is akin to
eating a gigantic humble pie. It's been a while since I've felt so
hopeless lol. Definitely brings back terrifying memories of the
beginning of this long software developer journey.

**RANT MODE ACTIVATED**

* Even with declaring a separate gem cache volume and docker layer caching,
I hate waiting for images to build.

* If I didn't have a mechanical keyboard, I'd really hate to prepend all
commands with `docker-compose run ${SERVICE}`. I'd alias the shit out of
Docker commands otherwise.

* Is the extra level of abstraction worth it in development? Most
library documentation is not written initially with Docker in mind. My
chat module is made up of a handful of server resources/workers. It was
relatively quick to scan the documentation and start all the different
processes up manually. It took no more than a couple hours to tweak
config and wrap everything in a Procfile. It took almost two full days
of suffering to hammer out a couple crude `Dockerfile`s and a `docker-compose.yml`.
Setting up the hostnames and ports to facilitate network traffic was
really fucking lame. Config error driven development blows.

* Writing README information for both traditional and Dockerized
development kinda bites. This doesn't even account for a production
environment `docker-compose.yml`.

**END RANT**

Well, if I didn't have to do the actual development containerization, I
think I'd be pretty damn satisfied using Docker in development though
LOL. Disregarding the learning curve (what tool/library doesn't require
a learning curve?), it is pretty cool to not have any of the
dependencies actually installed locally. This is especially helpful if
dealing with an unfamiliar language/framework.

So yeah, I still feel kinda meh? Maybe I'm still salty since there is no
one else to reap the benefits of this containerization effort? I'm
flying solo on this chat module, so the only real feedback so far is my
own. Guess we'll see down the road if anyone else has any positive
feedback.
