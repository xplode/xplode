---
layout: post
title:  "Doing stuff in Vim"
date:   2016-05-27
tags: vim
---

<h3>Executing bash commands</h3>
This is short and easy, but so useful it deserves it's own section.
{% highlight vim %}
:!<command>
{% endhighlight %}


<h3>Splits<h3>
<h4>Creating splits</h4>
Create vertical and horizontal splits with
{% highlight vim %}
:sp
:vsp
{% endhighlight %}

Create a new file opened in a vertical split with
{% highlight vim %}
:vnew filename
{% endhighlight %}

<h4>Navigating splits</h4>
{% highlight bash %}
left  -> <ctrl>h
right -> <ctrl>l
down  -> <ctrl>j
up    -> <ctrl>k
{% endhighlight %}

<hr>
<h3>Directory navigation</h3>
<h4>Edit an existing file</h4>
Open nerdtree from within vim
{% highlight bash %}
<ctrl>n<esc>
{% endhighlight %}

Open nerdtree file in a vertical split
{% highlight bash %}
# highlight file
s
{% endhighlight %}

{% highlight vim %}
Print the current working directory
:pwd
{% endhighlight %}

Change the current working directory to the directory of the currently open file
{% highlight vim %}
:cd %:p:h
{% endhighlight %}

<h3>Configuration</h3>
Reload .vimrc file
{% highlight vim %}
:so %
{% endhighlight %}

<h3>Finding files</h3>
<h4>CommandT</h4>
{% highlight bash %}
# loader is backslash \, then start typing and the fuzzy finder will do the
# rest.
<loader>t
# ctrl v opens the highlighted files in a vertical split
<c-v>
{% endhighlight %}

<h3>File manipulation</h3>
<h4>Edit an existing file</h4>
{% highlight vim %}
:edit <filename>
{% endhighlight %}

Save the current file under a different name (old file is not deleted)
{% highlight vim %}
:saveas <filename>
{% endhighlight %}


<h4>Create a new file</h4>
{% highlight vim %}
:new <filename>
{% endhighlight %}
<h4>Rename, delete or move the current file</h4>
<p>I just use Time Pope's Vim Eunich to do this stuff.  The commands he's provided are straight forward and easy to remember:</p>
<ul>
<li>:Remove: Delete a buffer and the file on disk simultaneously.
<li>:Move: Rename a buffer and the file on disk simultaneously.
<li>:Rename: Like :Move, but relative to the current file's containing directory.
<li>:Chmod: Change the permissions of the current file.
<li>:Mkdir: Create a directory, defaulting to the parent of the current file.
</ul>
<p>For a complete list of vim eunich commands see <a href="https://github.com/tpope/vim-eunuch">https://github.com/tpope/vim-eunuch</a>.</p>


<hr>
<h3>Git</h3>
<p>I prefer to execute git commands from the command line, but sometimes it's nice
to do gittin from vim.  Either you can do:
{% highlight vim %}
:!git <command>
{% endhighlight %}

Or give Tim Pope's Vim Fugitive a try.  He's aliases :!git to :Git so this is 
equalivent: 
{% highlight vim %}
:Git <command>
{% endhighlight %}
And if you dig into his plugin there is much more available.
<a href="https://github.com/tpope/vim-fugitive"></a>.
I installed it a few days ago, I'll try to keep track of the commands that I've
actually used below.
<ul>
<li>:Gmove: Deletes and renames the current buffer. 
</ul>

<hr>
<h3>Syntax</h3>
<h4>Enable/Disable Syntax highlighting</h4>
{% highlight bash %}
:syntax off

:syntax on
{% endhighlight %}
