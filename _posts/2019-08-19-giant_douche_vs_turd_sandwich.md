---
layout: post
title:  Giant douche vs turd sandwich
date:   2019-08-19
tags: [ruby, security]
---
In part one [Security got me
down](https://ftlam11.github.io/2019/08/15/security_got_me_down.html), I
introduced JWT, signing, and integrity checking through a basic
client/server authentication scheme. I wanted to learn more about JWT
and its use cases before committing to it for a project at work.

I'd like to briefly expand on JWT signing, then talk about JWT encryption/refresh
tokens before diving into comparing JWTs and sessions.

### JWT Signing continued

In part one, I gave an example of signing a JWT using a secret (HMAC).
In this case, there is **one** key used to generate the signature.
Anyone that has this key can sign valid tokens, therefore it is
imperative the key be kept private. Using a secret to sign JWTs keeps
things simple and can work well for "traditional monolith" applications.
It won't matter when scaling up with more instances of the application
since the key will be the same (assuming the key is configured properly
as an environment variable). No matter which application server receives
the client request, the server will be able to verify the JWT integrity.
I'll revisit this point when comparing JWTs and sessions.

Another way of signing the JWT is with a public/private key pair. This
method uses asymmetric encryption (don't confuse this with JWT
encryption!).

### Symmetric and Asymmetric encryption

In symmetric encryption a **single key** is used to encrypt and decrypt
the data. That sounds similar to what is happening when signing JWTs
using a secret, but it is not the same thing!

Decrypting is the act of converting an encrypted message back to its raw
form. There is no way to decrypt an HMAC, it is not bidirectional.

Recall that when the server attempts to verify the integrity of the JWT
token passed through the **Authorization** header, the server does not
actually decrypt the JWT. It Base64Url decodes the token and then uses
the secret along with the header and payload to regenerate another
signature to compare with the JWT signature. Matching signatures mean
the integrity check passed, JWT content was not fudged.

Asymmetric encryption is a bit more involved. From an encryption
standpoint, check [this
article](https://www.freecodecamp.org/news/https-explained-with-carrier-pigeons-7029d2193351/).
The article explains it using pigeons and relates it to HTTPS. For
encryption, the public key is used to write the message, while the
private key is used to read it. For signing, the private key is used to
write the message's signature, while the public key is used to verify
the signature.

What if a web application was modularized into several smaller server
applications though? If a secret was used to sign tokens, each server
could potentially sign tokens. It probably isn't desirable for each
smaller server application to be able to sign tokens. For this type of
architecture public/private key signing can be a good option. An
independant server application can be responsible for signing all tokens
using its private key. Other server applications can use the
corresponding public key to verify the signature is that of the signing
server.

OK, I realized I don't really wanna talk about JWT encryption. Here is
the [RFC](https://tools.ietf.org/html/rfc7516), good luck XD.

### Hmm how are JWTs revoked?

JWT invalidation is kinda fucked up. There is a standard `exp` claim
that specifies when the JWT expires. That's cool, but what if there is a
need to revocate a single JWT on demand? Some shithead stole my JWT! I
don't think waiting for it to expire is good. What if there is no `exp`
claim (this probably not a best practice)? Also, JWT tokens should be
invalid upon password changes? What are some strats for invalidating JWTs?

1. Clown strats: Change the secret or private/public key pair used to
   sign the JWTs. This will not only invalidate that one compromised
   JWT, but all other JWTs that were signed using the same secret/key
   pair. This results in all users being signed out. This is probably a
   last resort move.
2. Default the JWT expiration claim to a very short time interval, and
   use a long-lived refresh token to facilitate obtaining another JWT.
   I don't think you'd want the refresh token to be another JWT,
   assuming it is signed using the same secret of the JWT. The original
   problem of invalidating JWT is now compounded by trying to invalidate
   the refresh token. If the refresh token is just a generic token
   stored in the database and associated with a user, the server could
   query it and compare it against the client provided refresh token. To
   invalidate the JWT, change the refresh token stored in the database
   so the comparison fails. There still would be a small window for the
   JWT to be compromised though.
3. Maintain a blacklist of revoked JWTs. The tradeoff is another lookup has to
   be made to see if a JWT is blacklisted. The lookup could be ignored
   if the `exp` claim is already overdue. The blacklist should be
   centralized, wouldn't want conflicting revocation perspectives.
4. Abandon mission: Store the JWT on the user record and query the
   database for each request. Now the JWT is just a generic token lol.

Essentially the issue is how to invalidate ONE JWT without affecting all
other JWTs that have been signed with the same secret/key pair?

## JWTs vs sessions

And now the upshot.

1. [Stop using JWT for
   sessions](http://cryto.net/~joepie91/blog/2016/06/13/stop-using-jwt-for-sessions/)
2. [Why your solution won't
   work](http://cryto.net/~joepie91/blog/2016/06/19/stop-using-jwt-for-sessions-part-2-why-your-solution-doesnt-work/)

Ok, there are some really great points in both these posts. In
the context of default session behavior, Rails uses
[CookieStore](https://devdocs.io/rails~5.2/actiondispatch/session/cookiestore).
All session data is stored client side within the cookie. Cookies are
also encrypted using `secret_key_base` so they cannot be read without
it. So the comparison for Rails sessions should be made to **stateless**
JWT tokens. Let's cosplay the only pertinent session data contained on
both the cookie and JWT are a user ID.

### Stateless JWT vs `CookieStore`

What factors must be considered in using each option? Should you
mix-n-match them?

1. Web application architecture
2. Mobile application support
3. What the application services do

With a run-of-the-mill monolith Rails application, `CookieStore` is
probably the best option. The API is likely mounted on the same domain,
or maybe a subdomain so cookies can be shared and used. Just gotta pay
attention to configuring the `Set-Cookie`
[header](https://stackoverflow.com/questions/18492576/share-cookie-between-subdomain-and-domain/23086139#23086139).
One potential area of concern is if there must be support for mobile
applications. Definitely not my forte, how is the concept of a web
browser emulated by a native mobile application? Not gonna open that can
of worms (yet).

What if the application services involve something more than simply
serving JSON, e.g., authorizing downloads, dealing with websocket
connections, transcoding multimedia, etc... These features are likely
implemented on separate owned servers or through third-party services.
Cookies can't be set for domains that aren't the origin domain.
Cosplaying as one of those services, even if I could receive the cookie,
what the phuck can I do with it? It's encrypted using the issuing
server's secret. Even if I was able to decrypt it, there could be a
bunch of information that can't be properly interpreted without the
necessary context. So how do requests to the above services get authorized?

I need to do some more pondering.

## Moar Sauces

1. [Persisting cookies in IOS
   application](https://stackoverflow.com/questions/4597763/persisting-cookies-in-an-ios-application)
2. [Chris Oliver on cookies vs
   JWT](https://gorails.com/forum/cookies-vs-token-for-authentication#forum_post_3348)
3. [Rails session cookies for API
   authentication](https://pragmaticstudio.com/tutorials/rails-session-cookies-for-api-authentication)
4. [Ultimate guide to HTTP
   cookies](https://blog.webf.zone/ultimate-guide-to-http-cookies-2aa3e083dbae)
5. [Invalidating JSON Web
Tokens](https://stackoverflow.com/questions/21978658/invalidating-json-web-tokens?rq=1)
6. [Understanding RSA signing for
   JWT](https://stackoverflow.com/questions/38588319/understanding-rsa-signing-for-jwt)
