---
layout: post
title:  How I hacked a bandcamp user account (JK)
date:   2019-03-10
tags: [javascript]
---

Just sharing a fun write up about figuring out how to apply for a
Senior Fraud/Risk Engineer posting (I did not apply FYI). If I have an
opportunity to write a job posting myself, I will definitely try to make
it just as fun as this one.

1. Checking cookies

> To apply, gather the crumbs (starting with your cookies).

I found:

fraud_job_url: Over+here%3A+bandcamp.com%2Fectotherm%2Fsnakeoil_requests%3Fsnakeoil_param%3Dfrog

Decode the URL:

```ruby
URI.decode('Over+here%3A+bandcamp.com%2Fectotherm%2Fsnakeoil_requests%3Fsnakeoil_param%3Dfrog')
=>
"Over+here:+bandcamp.com/ectotherm/snakeoil_requests?snakeoil_param=frog"
```

Cool, let's check it out.

> Applying is not a quick win,
> To do so you must first log in.
> Take over the account
> Whose details lie about -
> найти необычный IP-адрес to begin.

There was a request log table with three columns: ip address, a bandcamp
URL, and user agent. I got lucky with this part, the requests with a
SnakeOil agent looked fishy. Running a `whois`, the ip address turned
out to be Russian, coincidental joke maybe?

https://en.wikipedia.org/wiki/Snake_oil_(cryptography)

The suspect bandcamp user: https://bandcamp.com/gilamonster

Cool, there's a function Launder, some character shifting, and a
character lookup dump. Reading comprehension being my weakness, I got
hung up looking for what to call `it()` on haha.

```ruby
lookup = %w(f y d s v k e i n p a t l r j u z o g w c q m h x b)
legend = ('a'..'z').to_a.reverse.zip(lookup).to_h
'dirty'.chars.map { |char| legend[char] }
=> ["m", "o", "n", "e", "y"]
```

Calling `Launder('money')` prompted a window alert with another URL to
follow showing an HTTP POST request with a user agent detail and body
params missing.

Cool, pieced together the missing information, fired the complete
request, and got this response:

> gilamonster uses the dumbest password you can think of - log in with
> it at [REDACTED]

Zang logged in, got redirected to the job posting page but with a
contact email to send info to.

Thus completes this little adventure. Most enjoyable job application
I've done ever.
