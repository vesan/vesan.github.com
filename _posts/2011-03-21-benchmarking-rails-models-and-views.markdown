---
layout: post
title: Benchmarking Rails models and views
tags: [ruby, rails, tips, benchmarking]
---

Ruby on Rails provides many helper methods and commands to help you benchmark and profile your application code. But tools are only useful if you use them. The following methods seem to escape my mind and I end up creating quick hacks for benchmarking instead of using the built-in ones. That is clearly not optimal and I wanted to tell you about these handy benchmarking helpers. 

## Models

In models you can use `benchmark` method available as model class method. It takes a block and after running it outputs the elapsed time to the log file. Here's an example:

{% highlight ruby %}
def generate_large_things(users)
  users.each do |user|
    Thing.benchmark("Creating a thing") do
      thing = Thing.generate!(user)
    end
    
    Thing.benchmark("Processing a thing") do
      thing.process!
    end
  end
end
{% endhighlight %}

After running the code you will see something like this in the log file:

    Creating a thing (0.47019)
    Processing a thing (2.17420)
    [other output]
    Creating a thing (0.42210)
    Processing a thing (4.23380)
    [other output]
    Creating a thing (0.39311)
    Processing a thing (2.02156)

The time measurements in seconds are in parentheses. In this case we can see that the processing took more time.

## Views

For benchmarking view code Rails provides `benchmark` method. It works the same way as the model equivalent taking a block and outputting results to the log file. Simple example in Haml:

{% highlight haml %}
- benchmark "Generating chart"
  = generate_chart(data)
{% endhighlight %}
