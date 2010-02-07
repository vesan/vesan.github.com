---
layout: post
title: "Rails tip: Partials know if you are rendering a collection or an object"
tags: [code, tips, ruby, rails, partials]
---

# [{{ page.title }}]({{ page.url }})

<div class="post_information">
  {{ page.date | date_to_string | upcase }}
</div>

When you render partial `thing`, variable `thing_counter` gets set. When you render it with an object `thing_counter` is set to zero (0).

{% highlight ruby %}
render :partial => 'thing', :object => @thing
{% endhighlight %}

When you render partial with a collection, `thing_counter` is set to one (1) and goes up with each iteration.

{% highlight ruby %}
render :partial => 'thing', :collection => @list_of_things
{% endhighlight %}

You can use this to do more or less things depending on whether partial is rendered for an object or a collection. If `thing_counter.zero?` is `true` it's rendering a object.
