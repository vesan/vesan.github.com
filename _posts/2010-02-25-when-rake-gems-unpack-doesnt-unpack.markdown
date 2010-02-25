---
layout: post
title: "When rake gem:unpack doesn't unpack"
tags: [code, tips, ruby, rails, partials]
---

If you run into a situation that `rake gems:unpack` silently skips some gems, check if those gems provide rake tasks. When Rakefile is loaded, vendor/gems is not yet added to load path so Rails doesn't try to unpack those gems. For example when you run generator for  [hoptoad_notifier](http://github.com/thoughtbot/hoptoad_notifier "thoughtbot's hoptoad_notifier at master - GitHub") it adds `tasks/hoptoad_notifier_tasks.rake` to your Rails project. If you try to unpack it after that, it doesn´t work.

You can either run `rake gems:unpack` before running the generator and adding the rake tasks or comment out the rake tasks temporarily and uncomment them after unpacking the gem. In hoptoad_notifier´s case you would comment out `require 'hoptoad_notifier/tasks'` in the `tasks/hoptoad_notifier_tasks.rake` file.
