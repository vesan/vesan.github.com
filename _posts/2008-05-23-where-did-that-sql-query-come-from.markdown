---
layout: post
title: Where did that SQL-query come from?
tags: [code, plugins, rails, rake, performance]
---

I’ve been using Ruby on Rails plugin [QueryTrace](http://terralien.com/projects/querytrace/) to see where the database action is really happening in my Rails apps. It adds short backtrace to your database queries in your log files. It’s great for pinpointing performance problems.

It causes the app to slow down a bit, so I have used Alex Chaffee’s [query_trace rake tasks](http://pivots.pivotallabs.com/users/alex/blog/articles/300-rake-query-trace) to enable and disable it when needed. But now it seems that QueryTrace is moved to github and the old subversion repo is not working any more.

So I made working version of the rake tasks. Functionality is the same and because github makes tarballs of the projects for download git is not needed.

Here is the code:

```ruby
require 'open-uri'

namespace :query_trace do
  desc "Enables the query_trace plugin. Must restart server to take effect."
  task :on => :environment do
    unless File.exist?("#{RAILS_ROOT}/vendor/query_trace.tar.gz")
      Dir.chdir("#{RAILS_ROOT}/vendor") do
        url = "http://github.com/ntalbott/query_trace/tarball/master"
        puts "Loading query_trace tarball from #{url}..."
        file = open("query_trace.tar.gz","wb") do |f|
          f.write(open(url).read)
        end
      end
    end
    Dir.chdir("#{RAILS_ROOT}/vendor/plugins") do
      system "tar zxf ../query_trace.tar.gz"
    end
    puts "QueryTrace plugin enabled. Must restart server to take effect."
  end


  desc "Disables the query_trace plugin. Must restart server to take effect."
  task :off => :environment do
    Dir.chdir("#{RAILS_ROOT}/vendor/plugins") do
      folder = Dir.glob("ntalbott-query_trace-*")
      if folder
        folder.each {|f| FileUtils.rm_rf(f)}
      end
    end
    puts "QueryTrace plugin disabled. Must restart server to take effect."
  end
end
```
