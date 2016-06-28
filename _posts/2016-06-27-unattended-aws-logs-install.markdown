---
layout: post
title:  "Unattended aws cloudwatch logger install"
date:   2016-05-26
tags: aws cloudwatch 
---
<h3>Create cloudwatch credentials</h3>
Open the Identity and Access Management (IAM) console at https://console.aws.amazon.com/iam/.

Create a user and give it the Role Policy.
On the Permissions tab, under Inline Policies, click Create Role Policy.
On the Set Permissions page, click Custom Policy, and then click Select.

For more information about creating custom policies, see http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-policies-for-amazon-ec2.html IAM Policies for Amazon EC2 in the Amazon EC2 User Guide for Linux Instances.

On the Review Policy page, in the Policy Name field, type a name for the policy.

In the Policy Document field, paste in the following policy:

{% highlight bash %}
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "logs:DescribeLogStreams"
    ],
      "Resource": [
        "arn:aws:logs:*:*:*"
    ]
  }
 ]
}
{% endhighlight %}<br>
Click Apply Policy.


<h3>Create the aws credentials file</h3>
This should live under the user that will be installing cloudwatch.  The file should
look like:
{% highlight bash %}
$ cat /home/xplode/.aws/credentials
[default]
aws_access_key_id = ####################
aws_secret_access_key = ########################################
{% endhighlight %}<br>

<h3>Create the awslogs config file</h3>
The following example conf file lives in /opt/awslogs/awslogs.conf.  It will log
the output of the widget.log starting from the end of that log file.  The system's
hostname is used to identify the log stream in cloudwatch.

{% highlight bash %}
$ cat  /opt/awslogs/awslogs.conf
[general]
# Path to the CloudWatch Logs agent's state file. The agent uses this file to maintain# client side state across its executions.
state_file = /var/awslogs/state/agent-state

[/var/log/widget/widget.log]
datetime_format = %Y-%m-%d %H:%M:%S
file = /var/log/widget/widget.log
buffer_duration = 5000
log_stream_name = {hostname}
initial_position = end_of_file
log_group_name = /var/log/samhain/samhain.log
{% endhighlight %}<br>


<h3>Install python</h3>
{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get install python-setuptools  python-pip
{% endhighlight %}<br>

<h3>Install awslogs</h3>
{% highlight bash %}
curl https://s3.amazonaws.com/aws-cloudwatch/downloads/latest/awslogs-agent-setup.py -O
python ./awslogs-agent-setup.py --region us-east-1 --non-interactive --configfile=/opt/awslogs/awslogs.conf
{% endhighlight %}<br>

<h3>Stopping and starting awslogs</h3>
{% highlight bash %}
$ sudo service awslogs stop
$ sudo service awslogs start 
{% endhighlight %}<br>

<h3>Tips</h3>
The aws logger uses a state file to keep track of where it is between starts.  If you
want to erase the state to force the logger to start fresh from either the beginning
or end of the file (depending on your configuration), simply stop the logger, delete the state file and start the logger.

{% highlight bash %}
$ sudo service awslogs stop
$ sudo rm /var/awslogs/state/agent-state
$ sudo service awslogs start 
{% endhighlight %}<br>

<h3>References</h3>
http://www.brandonchecketts.com/archives/unattended-install-of-cloudwatch-logs-agent
