---
layout: post
title:  "nodejs github api"
date:   2016-06-10
tags: github nodejs javascript
---
<h2>Calling the github api in nodejs</h2>
This example retrieves all issues in the playground repo using a personal access token.  Personal access token which are generated via user Settings > Personal Access Tokens
{% highlight javascript %}
let https = require('https');

var options = {
                host: 'api.github.com',
                path: '/repos/xplode/playground/issues',
                headers: { 'Authorization': 'Token ########################################',
                           'User-Agent': 'xplode'
                         }
              };

var req = https.request(options, (res) => {
  console.log(`STATUS: ${res.statusCode}`);
  console.log(`HEADERS: ${JSON.stringify(res.headers)}`);
  res.setEncoding('utf8');
  var body = '';
  res.on('data', (chunk) => {
    body += chunk;
    console.log(`BODY: ${chunk}`);
  });
  res.on('end', () => {
    console.log(body);
    console.log('No more data in response.');
    var issues = JSON.parse(body);
    console.log(issues);
    // Do something with the issues now that I have them.
  });
});

req.on('error', (e) => {
  console.log(`problem with request: ${e.message}`);
});

req.end();
{% endhighlight %}
