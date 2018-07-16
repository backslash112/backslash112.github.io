---
layout: post
title:  "How to create a sample web server with Node.js quickly"
date:   2016-07-05 14:32:48
categories: server
---

You can use Connect and ServeStatic with Node.js for this:

1. Install connect and serve-static with NPM: <br />
~~~ shell
npm install connect serve-static
~~~

2. Create server.js file with this content:<br />
~~~ javascript
var connect = require('connect');
var serveStatic = require('serve-static');
connect().use(serveStatic(__dirname)).listen(8080, function(){
    console.log('Server running on 8080...');
});
~~~

 3. Run with Node.js:
 ~~~ shell
node server.js
~~~


[react-native-getting-started]: http://facebook.github.io/react-native/docs/getting-started.html#content
[jekyll]:      http://jekyllrb.com
