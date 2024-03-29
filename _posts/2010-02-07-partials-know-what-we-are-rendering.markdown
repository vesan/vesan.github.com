---
layout: post
title: "Rails tip: Partials know if you are rendering a collection or an object"
tags: [code, tips, ruby, rails, partials]
---

When you render partial `thing`, variable `thing_counter` gets set. When you render it with an object `thing_counter` is set to zero (0).

```ruby
render :partial => 'thing', :object => @thing
```

When you render partial with a collection, `thing_counter` is set to one (1) and goes up with each iteration.

```ruby
render :partial => 'thing', :collection => @list_of_things
```

You can use this to do more or less things depending on whether partial is rendered for an object or a collection. If `thing_counter.zero?` is `true` it's rendering a object.
