---
layout: post
title:  "curl github api"
date:   2016-06-10
tags: curl github 
---
<h2>Calling the github api issue using curl</h2>
close issue 8376 in the playground repo
{% highlight bash %}
curl -X PATCH -d '{"state": "closed"}' -u xplode:########################################  https://api.github.com/repos/xplode/playground/issues/8376
{% endhighlight %}
<br>
The examples use a github personal access token and Basic Auth.
Personal access tokens are generated via the user Settings > Personal Access Tokens
<br>
<h4>The general form for PATCH, POST, PUT requests</h4>
{% highlight bash %}
curl -X <http verb> -d <body in json format> -u <username>:<personal access token> https://api.github.com/repos/<username>/<repo>/issues/<issue>
{% endhighlight %}
<br>
<h4>The general form for GET requests</h4>
{% highlight bash %}
# without query parameters
curl -u <username>:<personal access token> https://api.github.com/repos/<username>/<repo>/issues/<issue>

# with query parameters.
# note the quotes surround the url, you need to do this when using multiple query parameters
# otherwise the ampersand in the query string will background the job.  Maybe best to
# use quotes if you plan on using any query params at at just to avoid and head scratchers.
curl -u <username>:<personal access token> "https://api.github.com/repos/<username>/<repo>/issues/<issue>?<param_1>=<value_1>&<param_2>=<value_2>"
{% endhighlight %}
<br>
<h4>Examples</h4>
<h5>List issues in the playground repo</h5>
This will only list open issues owned by me.
{% highlight bash %}
curl -u xplode:######################################## https://api.github.com/repos/xplode/playground/issues
{% endhighlight %}
<br>
Return all issues that:
<ul>
<li>have been created since June 12th, 6:42am MDT ( 12:42 UTC time )
<li>are owned by anyone as long as I have access to them
<li>have the blue label
<li>are closed
<li>sort by the created time in descending order
</ul> 
{% highlight bash %}
curl -u xplode:######################################## "https://api.github.com/repos/xplode/playground/issues?since=2016-06-12T12:42:00Z&filter=all&labels=blue&state=closed&sort=created&direction=desc"
{% endhighlight %}
<br>
<h5>Close 1000 issues in the playground repo in sequence starting at issue 100</h5>
{% highlight bash %}
for issue in $(seq 1000); do
  curl -X PATCH -d '{"state": "closed"}' -u xplode:######################################## https://api.github.com/repos/xplode/playground/issues/$(($issue + 99))
  done
{% endhighlight %}
