---
layout: post
title:  "The state of evil"
date:   2015-01-03
thumbnail: gtg_the-crew.png 
image: gtg_sun-over-shoulder.png 
categories: testing
tags: top_level_post
---

<h4>Recently I wrote the following abhorrent test code:</h4>

{% highlight ruby  %}
RSpec.shared_context "setup and teardown" do
 
  before(:each) do
    if example == example.example_group.descendant_filtered_examples.first
      # do setup stuff
    end
  end
 
  after(:each) do
    if example == example.example_group.descendant_filtered_examples.last
      # do teardown stuff
    end
  end
end
{% endhighlight %}

<br>
<strong>Why is this code so awful?</strong>  <u>State across tests is evil</u>

Tests ('examples' in RSpec) should be independent of each other.  The code snippet above creates state across an example group.  This code executes as part of the first and last examples in the group with the pointed intention to create state across all examples in the group.

Rspec provides hooks for executing code before and after <code>:each</code> or <code>:all</code> examples in an example group.

The <code>:each</code> hooks execute after entering the example group, prior to each example.  RSpec basically appends the code in the hook to the code in your example.

The <code>:all</code> hooks execute outside of the example group and do not have access to the state of the example group.  Use of the <code>:all</code> hooks is a red flag.  Examples should be independent of each other, performing actions across entire contexts creates dependency across unit tests.  This is usually bad and wrong... badong. 

<br>
<strong>So why would I write this code?</strong>  <u>Sometimes evil is necessary</u>

I was using rspec along with capybara and selenium to perform system tests against a running application.  These tests are meant to replace a human interacting with a live system, nothing is mocked out and just getting to certain function requires a ton of state.

Some test groups depend on system state that is time consuming to setup.  Other test groups require that this state not exist.  Different permutations of the same expensive state are applicable to other test groups, thus having a shared context that creates this state then tears it down made sense.  In order to have access to context specific state (so we can tune the setup for different test groups) I needed access to the state of the context (so I couldn't use the <code>:all</code> hooks).

In the end I am not satisfied with this solution, if you have a solid system test architecture that is able to test state heavy systems in a lightweight compartmentalized way without mocking, I would happy to hear from you.
