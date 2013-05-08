---
layout: post
title: Syncing OmniFocus with Trello
tags: [trello, omnifocus, workflow, open-source]
---

We have started using Trello for organizing project at [Kisko Labs](http://kiskolabs.com) and to follow my [Pivotal Tracker to OmniFocus](http://vesavanska.com/2012/pivotal-tracker-integration-for-omnifocus/) adapter I created a similar for Trello.

You can specify which lists of cards are considered done so that moving a card to a "Done" list will mark them done also in OmniFocus when syncing.

To start using it, simply install the gem and start sync:

{% highlight sh %}
$ gem install omnifocus-trello
$ of sync
{% endhighlight %}

On the first run the sync will create `~/.omnifocus-trello.yml` file. Follow the instructions in the file to get a Trello access token. After completing the instructions and filling the needed information, just re-run `of sync` and it will do the first sync. After that you can run `of sync` any time and it will update the OmniFocus data.

The code is available at [GitHub](https://github.com/vesan/omnifocus-trello).
