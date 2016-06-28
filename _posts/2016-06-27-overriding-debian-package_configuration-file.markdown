---
layout: post
title:  "Overriding debian package configuration file"
date:   2016-06-27
tags: dpkg-package-management 
---
<h3>Overview</h3>
If you install a service via a debian package it will probably be started as part of the install.  This can be a problem if the default configuration shipped with the package is unsatisfactory and you need the package to start up correctly on the first try.  
<h4>for example</h4>
I am installing the made-up-monitor service that makes bad assumptions in it's default configuration file /opt/made-up-monitor/config and due to this configuration errors are generated when the service first starts up.

I could just stop the service, fix the config and then start it back up, or I could ensure that the service starts up with the correct config from the get go. 

<h3>Solution</h3>
Prior to install, specify a diversion.
{% highlight bash %}
$ sudo dpkg-divert --package made-up-monitor --divert /opt/made-up-monitor/config.orig --rename /opt/made-up-monitor/config
{% endhighlight %}<br>
Then prior to installation, create the correct configuration file at /opt/made-up-monitor/config
<br>
Now you can install the package and it will use the correct configuration file.
