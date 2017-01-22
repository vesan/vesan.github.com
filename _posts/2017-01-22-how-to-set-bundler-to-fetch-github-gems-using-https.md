---
layout: post
title: How to set Bundler to fetch Github gems using HTTPS
tags: [ruby, bundler, heroku, travis]
---

Fetching Github gems using HTTPS instead of git protocol is going to be the new default when Bundler 2.0 is released. Here are the instructions on how to do it locally and on [Heroku](https://www.heroku.com/) and [Travis CI](https://travis-ci.com/) before Bundler 2.0 is released.

Configure Bundler to use HTTPS for GitHub sources:

```ruby
bundle config --global github.https true
```

And on Heroku (note the double underscore):

```ruby
heroku config:set BUNDLE_GITHUB__HTTPS=true
```

And on Travis (put it to your `.travis.yml` file):

```ruby
before_install:
  - "bundle config --global github.https true"
```
