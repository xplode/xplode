---
layout: post
title:  'Bloody singleton pattern'
date:   2015-01-09 12:18:00
thumbnail: gtg_sun-over-shoulder.png 
categories: design-patterns 
tags: top_level_post
---

<h4>The singleton pattern is an ANTI-pattern.</h4>
I've known this for some time and have seen fellow programmers use it as a crutch for years, but I only recently witnessed a great example in the wild.

I was working on integrating an application with a very large corporation's cloud service (we'll call them SupaMegaCorp <strong>SMC</strong>).  In doing so I used a gem that SMC provided to deploy and manage virtual machines in the SMC cloud.

This gem used a singleton object to manage configuration data, including login info for a single account.

My application was a type of cross cloud application manager and needed to manage virtual machines in multiple accounts simultaneously.  It couldn't do that with a single global account configuration.

Sure the application could switch account info on the fly, but that would serialize time expensive cloud operations.  Spinning up servers across different accounts is a task meant to be parallelized.

I ended up monkey patching SMC's code locally and submitted a patch to the developer maintaining the code.  Below is an example of the SMC problem and my fix for it.
<br><br>
<strong>The SMC api code looked something like this:</strong>
{% highlight ruby %}
class Configuration
  include Singleton
  attr_accessor :api_username, :api_password
end

class ApiAccessor
  def initialize()
    api_username = Configuration.instance.api_username
    api_password = Configuration.instance.api_password
    @api_handle = get_handle(api_username, api_password)
  end

  def create_virtual_machine
    ... whatever happens where the @api_handle is used to create a virtual machine
  end
end
{% endhighlight %}
<br>
<strong>Here is how this api is used.</strong>
{% highlight ruby %}
Configuration.instance.api_username = 'my_username'
Configuration.instance.api_password = 'my_password'

cloud = ApiAccessor.new
cloud.create_virtual_machine
{% endhighlight %}
<br>
<strong>My fix is below,</strong> it allows for a default global configuration while given the caller freedom to pass in a specific account configuration.
{% highlight ruby %}
class Configuration
  @@the_instance = nil

  attr_accessor :api_username, :api_password

  def self.instance
    @@the_instance = self.new unless @@the_instance
    return @@the_instance
  end

  def self.set_instance(instance)
    @@the_instance = instance
  end
end


class ApiAccessor
  def initialize(config = nil)
    @config = config || Configuration.instance 
    api_username = config.api_username
    api_password = config.api_password
    @api_handle = get_handle(api_username, api_password)
  end
end
{% endhighlight %}
