---
layout: post
title:  "Doing stuff in Git"
date:   2016-05-27
tags: git 
---

<h4>Show all local and remote branches</h4>
{% highlight bash %}
git branch -v -a
{% endhighlight %}

<br>
<h4>Create a branch and setting the upstream origin</h4>
{% highlight bash %}
git checkout -b <branch>
git push -u origin <branch>
{% endhighlight %}

<br>
<h4>Attach a detached head to a remote origin.</h4>
{% highlight bash %}
git checkout refs/remotes/origin/<branch>
git checkout -b <branch>
git push --set-upstream origin <branch> 
{% endhighlight %}


<h4>Force push after rebase of your branch.</h4>
{% highlight bash %}
git push origin your-branch -f
{% endhighlight %}

