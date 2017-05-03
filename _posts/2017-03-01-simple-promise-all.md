---
layout: post
title:  "Simple promise all"
date:   2017-03-01
tags: [javascript]
---
A naive version of `Promise.all()` using built-in HTTP node module. Once all asynchronous requests have returned, it spits everything out using the `print()` callback.

{% highlight javascript %}
var http = require('http');
var responses = [];
var responseCount = 0;

var print = function(collection) {
  for (var i = 0; i < collection.length; i++) {
    console.log(collection[i]);
  };
};

var get = function(url, queue) {
  http.get(url, function(res) {
    var payload = '';
    res.setEncoding('utf8');
    res.on('data', function(chunk) {
      payload += data;
    });
    res.on('error', function(err) {
      return console.log(err);
    });
    res.on('end', function() {
      responses[queue] = payload;
      responseCount++;
      if (responseCount === 3) print(responses);
    });
  });
};
{% endhighlight %}