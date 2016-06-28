---
layout: post
title:  "Quick and dirty server management"
date:   2016-05-07
tags: bash ssh mysql
---
Execute some code against a set of servers whose ip addresses are stored
 in a mysql database.
{% highlight bash %}
# First, know the parameters that allow you to retrieve mysql values from the
# command line without any extra formatting.
# --silent
# --skip-column-names
# --execute
$ mysql --silent --skip-column-names --execute 'select address from Servers';
1.1.1.1
2.2.2.2

# shorthand
mysql -s -N -e 'select address from Servers';

# Next know how to iterate over the retrieved values
for address in $(mysql -s -N -e 'select address from Servers'); do echo $address; done
1.1.1.1
2.2.2.2

# Last, know how to use ssh to execute a command on a remote server.
ssh 1.1.1.1 "command goes here"
{% endhighlight %}
<br>
Let's put it all together in to do something useful.
{% highlight bash %}
for address in $(mysql -s -N -e 'select address from Servers'); do
  # Change the repository for my personal repo from whatever it is to development.
  ssh $address "sudo sed -i -e 's/^deb.*xplode.*$/deb https:\/\/repo.xplode.io\/debian\/ xplode development/' /etc/apt/sources.list"
done
{% endhighlight %}
<br>
Since I have to change repos regularily, I put the code in a script that takes a repo arg.
{% highlight bash %}
#!/bin/bash

if [[ -z $1 ]]; then
  echo "Usage:  switch [component]"
  exit 1
fi

for address in $(mysql -s -N -e 'select address from Servers'); do
  ssh $address "sudo sed -i -e 's/^deb.*xplode.*$/deb https:\/\/repo.xplode.io\/debian\/ development '$1'/' /etc/apt/sources.list"
 done
{% endhighlight %}
