---
layout: post
title: Where did that SQL-query come from?
tags: [code, plugins, rails, rake, performance]
---

# [{{ page.title }}]({{ page.url }})

<div class="post_information">
  {{ page.date | date_to_string | upcase }}
</div>

I’ve been using Ruby on Rails plugin [QueryTrace](http://terralien.com/projects/querytrace/ "Terralien - QueryTrace") to see where the database action is really happening in my Rails apps. It adds short backtrace to your database queries in your log files. It’s great for pinpointing performance problems.
