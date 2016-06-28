---
layout: post
title:  "Checking for an installed package in bash script"
date:   2016-05-31
tags: bash packages 
---

Script to do stuff depending on a package being installed on the system.
{% highlight bash %}
#!/bin/bash

if dpkg -l | grep $PACKAGE_NAME | grep '^.i'; then
  # do stuff
fi
{% endhighlight %}
