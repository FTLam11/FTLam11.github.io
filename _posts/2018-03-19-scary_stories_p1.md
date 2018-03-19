---
layout: post
title:  Scary Stories p1
date:   2018-03-19
tags: [stories]
---
Sometimes I run into some really silly shit at work, time to share those
in the form of "scary stories".

One of the projects I worked on required helping the client setup a
server for production use. Some observations/sources of pain/scary shit I noted:

1. Awful and misleading domain name
2. No SSH access means I have to contact someone everytime I want to
   deploy a new version. If no one responds, I can't deploy. So much for
   setting up Capistrano.
3. No automation for docker builds. The front-end container was also
   using Next.js for server side rendering, and building the source
   files took five minutes every. single. goddamn. time.
4. `npm install` failed intermittently. Perhaps due to some network
   error? I never figured it out, rebuilding the container would
   eventually succeed.
5. Static assets was setup to be served by nginx, but there was one file
   that could not be found. After about a week of painstakingly
   investigating each layer of the application, it was discovered the
   client's firewall was blocking all media files. My boss told me to
   change the file extension, and wham, success.
6. Relating to the last point, I setup the entire application locally
   and on our own staging server to verify the static asset issue.
   Clearly, there were no problems locally or on the staging server.
   Sigh.
7. Whatever fisher price server our front-end container was running had
   terrible performance. Using the benchmarking tool `wrk`, the number
   of requests/second was something like 40/minute in production mode?
   What!?
8. Ended up exporting the entire front-end app into a bunch of static HTML
   files. The static files were served by nginx. This improved performance by like over 1000
   times lol.
9. I hate rock hunting.
