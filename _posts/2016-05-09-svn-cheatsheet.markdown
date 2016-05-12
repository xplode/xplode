---
layout: post
title:  "SVN cheatsheet"
date:   2016-05-09
tags: svn cheatsheet 
---
<small>
<strong>*cheatsheet*</strong>
Stuff I do occasionally and can never seem to remember exactly.
</small>

{% highlight bash %}
# After editing a .working file to resolve conflicts, resolve the conflict.
svn resolve --accept working <filename> 

# Resolve a tree conflict where <path> is the conflicted file path that you
# want to accept.
svn resolve --accept working -R <path>
{% endhighlight %}
