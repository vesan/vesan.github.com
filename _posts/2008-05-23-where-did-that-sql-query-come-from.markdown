---
layout: post
title: Where did that SQL-query come from?
tags: [code, plugins, rails, rake, performance]
---

# [{{ page.title }}]({{ page.url }})

<div class="post_information">
  {{ page.date | date_to_string | upcase }}
</div>

I’ve been using Ruby on Rails plugin [QueryTrace](http://terralien.com/projects/querytrace/) to see where the database action is really happening in my Rails apps. It adds short backtrace to your database queries in your log files. It’s great for pinpointing performance problems.

It causes the app to slow down a bit, so I have used Alex Chaffee’s [query_trace rake tasks](http://pivots.pivotallabs.com/users/alex/blog/articles/300-rake-query-trace "alex blabs - rake query_trace") to enable and disable it when needed. But now it seems that QueryTrace is moved to github and the old subversion repo is not working any more.
