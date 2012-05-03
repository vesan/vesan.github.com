---
layout: post
title: Pivotal Tracker integration for OmniFocus
tags: [pivotal tracker, omnifocus, workflow, open-source]
---

I use OmniFocus for my task management and I've been using [omnifocus](https://rubygems.org/gems/omnifocus) and [omnifocus-github](https://rubygems.org/gems/omnifocus-github) gems to sync issues assigned to me into OmniFocus.

I also wanted this ability for my Pivotal Tracker projects. So I wrote the needed integration code and released [omnifocus-pivotaltracker](https://rubygems.org/gems/omnifocus-pivotaltracker). It syncs non-finished stories, tasks and chores that are assigned to you from all you Pivotal Tracker projects.

To start using it, simply install the gem and start sync:

{% highlight sh %}
$ gem install omnifocus-pivotaltracker
$ of sync
{% endhighlight %}

On the first run the sync will create `~/.omnifocus-pivotaltracker.yml` file. Edit it and fill your Pivotal Tracker name and token. After filling those, just re-run `of sync` and it will do the first sync. After that you can run `of sync` any time and it will update the OmniFocus data.
