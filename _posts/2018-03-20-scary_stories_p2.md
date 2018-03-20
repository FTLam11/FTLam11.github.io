---
layout: post
title:  scary stories p2
date:   2018-03-20
tags: [stories]
---
Mannnnnnnnnn, I just gotta run into some problems first thing in the
morning. I'm deploying the next version of a React app on the client's
server. Then I see this T H I C C stack trace:

```
csc_web        | TypeError: Converting circular structure to JSON
csc_web        |     at JSON.stringify (<anonymous>)
csc_web        |     at module.exports
(/home/app/webapp/node_modules/htmlescape/htmlescape.js:24:15)
csc_web        |     at NextScript.render
(/home/app/webapp/node_modules/next/dist/server/document.js:306:79)
csc_web        |     at xa
(/home/app/webapp/node_modules/react-dom/cjs/react-dom-server.node.production.min.js:31:322)
csc_web        |     at a.render
(/home/app/webapp/node_modules/react-dom/cjs/react-dom-server.node.production.min.js:35:18)
csc_web        |     at a.read
(/home/app/webapp/node_modules/react-dom/cjs/react-dom-server.node.production.min.js:34:250)
csc_web        |     at renderToStaticMarkup
(/home/app/webapp/node_modules/react-dom/cjs/react-dom-server.node.production.min.js:43:75)
csc_web        |     at _callee3$
(/home/app/webapp/node_modules/next/dist/server/render.js:227:100)
csc_web        |     at tryCatch
(/home/app/webapp/node_modules/regenerator-runtime/runtime.js:65:40)
csc_web        |     at Generator.invoke [as _invoke]
(/home/app/webapp/node_modules/regenerator-runtime/runtime.js:299:22)
```

Was planning on using `git bisect` but I had a gut feeling which commit
was going to be the culprit. I got lucky checking out the correct commit
and read through the build logs. Realizing that the build is now
exporting static HTML, I see a bunch of API responses getting logged in
the working commit. There are no API responses getting logged in the bad
commit.

Huh? I'm sure the API server is up, I can connect to it from my own
computer outside of the client's network. Could it possibly be that the
client's own server CAN'T CONNECT TO ITS OWN API SERVER IT IS HOSTING?

Du-du-du-du-du its the audio daily double. I ping the API URL on the
client's teamviewer connection and of course, something is wrong with
their network setup. Good gravy, can I catch a break?
