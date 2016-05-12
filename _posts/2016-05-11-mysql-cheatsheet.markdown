---
layout: post
title:  "MySQL cheatsheet"
date:   2016-05-11
tags: mysql cheatsheet 
---
<small>
<strong>*cheatsheet*</strong>
Stuff I do occasionally and can never seem to remember exactly.
</small>

{% highlight bash %}
# Backup a database on a live system.
mysqldump -u<user> -p<pass> --single-transaction --routines --triggers --all-databases > backup_db.sql

# Restore database backup.
mysql -u root -p < backup_db.sql

# Get version
mysql --version

# Load data from a file for insert: LOAD_FILE
# * ensure that mysql can read the file and that it is on the mysyql server's system.
sudo chown mysql:mysql myfile.tar.gz
mysql -e 'UPDATE File SET content=LOAD_FILE("/tmp/myfile.tar.gz") WHERE id=0'
{% endhighlight %}
