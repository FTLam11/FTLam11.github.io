---
layout: post
title:  scary stories p3
date:   2018-03-23
tags: [stories]
---
Lord have mercy on mahhhhhhhh sooooooooooouuullllllllll.

I was wrong for trying to monkey patch htmlescape. I was even more wrong
for trying to do the same with next js.

Curses docker. How did `TypeError: Converting circular structure to
JSON` cause such a long series of unfortunate events?

Because running `docker build` reuses previous cached images to speed up
the build process for a new container, `next export` kept failing with
the circular structore JSON error. I'm still not sure why but adding the
`--no-cache` flag solved the problem. Feels bad man.
