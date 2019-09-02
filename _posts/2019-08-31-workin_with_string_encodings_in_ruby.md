---
layout: post
title:  Wrassling with string encodings in Ruby
date:   2019-08-31
tags: [ruby]
---

I ran into some head scratchers trying to exchange encrypted messages
between servers. It was a good time to get a small taste of dealing with
different string encodings.

After trying to brute force convert between ASCII-8BIT and UTF-8
(turrible idea BTW) I picked up some trock tronkory involving binary
strings, Base64, and how Ruby handles string encodings.

I was trying to read in a file containing binary string data as
environment variables, but the default string encoding used to read the
file is UTF-8. I naively thought I could save binary strings that would
be interpreted as UTF-8 at runtime and then manually convert back to
binary. I simply did not understand string encodings... and sought to
learn more about them [here](http://kunststube.net/encoding/).

Ruby has as `Base64` [module](https://devdocs.io/ruby~2.5/base64) to
encode/decode binary data to its own string representation. Enough with
the binary data shenanigans, I needed to:

1. Read binary string keys (ASCII-8BIT)
2. Encrypt JSON data using a key (ASCII-8BIT)
3. Base64 encode it (still ASCII-8BIT)
4. Convert encoding to UTF-8

Just for laughs and giggles I tried my hand at using [function
composition](https://www.ghostcassette.com/function-composition-in-ruby/)
to write a module to handle this workflow:

{% highlight ruby %}
module LarvataCable
  module AuthWrapper
    extend self

    FN = {
      encrypt: ->(data) { LarvataCable.auth_box.encrypt(data) },
      base64_encode: ->(data) { Base64.encode64(data) },
      utf8_encode: ->(data) { data.encode('UTF-8', 'ASCII-8BIT') },
      ascii_encode: ->(data) { data.encode('ASCII-8BIT', 'UTF-8') },
      base64_decode: ->(data) { Base64.decode64(data) },
      decrypt: ->(data) { LarvataCable.auth_box.decrypt(data) },
    }.freeze

    GENERATE_TOKEN = FN[:encrypt] >> FN[:base64_encode] >> FN[:utf8_encode]
    PARSE_TOKEN = FN[:ascii_encode] >> FN[:base64_decode] >> FN[:decrypt]

    private_constant :FN

    def generate_token(data)
      { token: GENERATE_TOKEN.(data.to_json) }
    end

    def parse_token(params)
      JSON.parse(PARSE_TOKEN.(params[:token]))
    end
  end
end
{% endhighlight %}

To read in the keys, I'm using the magic comment feature to instruct
Ruby that the file containing the keys uses binary encoding. Currently I
have this file as a Rails initializer and gitignored, but I'll
re-attempt to read it in from an .env file. Just gotta remember the
string encoding shenanigans jaja.

{% highlight ruby %}
# encoding: binary
{% endhighlight %}
