---
layout: post
title:  "Quickly create a textfile in bash"
date:   2016-06-02
tags: bash
---

Write out text to a file in bash.
{% highlight bash %}
#!/bin/bash
cat <<EOF > /full/path/myfile
Lorem ipsum dolor sit amet, consectetur... 
EOF
{% endhighlight %}


