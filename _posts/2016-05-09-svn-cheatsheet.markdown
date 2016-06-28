---
layout: post
title:  "SVN cheatsheet"
date:   2016-05-09
tags: svn cheatsheet 
---
<h4>resolving conflicts</h4>
{% highlight bash %}
# After editing a .working file to resolve conflicts, resolve the conflict.
svn resolve --accept working <filename> 

# Resolve a tree conflict where <path> is the conflicted file path that you
# want to accept.
svn resolve --accept working -R <path>
{% endhighlight %}<br>

<h4>reverting unwanted committed changes</h4>
{% highlight bash %}
# This is how I revert to a specific revision for a branch I'm working in.

# First I check the history see the revisions.
$ svn log $REPO/branches/2027 --stop-on-copy
------------------------------------------------------------------------
r12885 | xplode | 2016-05-31 10:08:57 -0600 (Tue, 31 May 2016) | 1 line

ref #2027 Making a bad change that I'll want to revert.
------------------------------------------------------------------------
r12882 | xplode | 2016-05-27 15:44:31 -0600 (Fri, 27 May 2016) | 1 line

ref #2027 Added something awesome.
------------------------------------------------------------------------
r12877 | xplode | 2016-05-27 10:00:18 -0600 (Fri, 27 May 2016) | 1 line

ref #2027 creating
------------------------------------------------------------------------

# I want to revert to revision 12882, my last awesome change before the bad commit.
svn merge -r HEAD:12882 $REPO/branches/2027
# This is a reintegrate merge, see the references below for an explanation of the
# reintegrate merge.


# Don't forget to commit the change.
svn ci -m 'ref #2027 reverting to revision 12882'

{% endhighlight %}
http://svnbook.red-bean.com/en/1.7/svn.ref.svn.c.merge.html
