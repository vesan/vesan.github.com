---
layout: post
title: Kippt gem 3.0 released
tags: [programming, kippt, releases, open-source]
---

![Kippt logo](/images/2012/kippt-logo-86.png)

I've been working on the Kippt gem this December and this release bundles all the work together.

Biggest change that calls for the major version number bump is changing all the APIs to return proxies so that the user of the library gets full control on what is loaded from the server. Backwards compability has been kept when possible but here it couldn't be kept.

Feature additions are mostly providing methods for actions that the library didn't support before like favoriting clips, checking if authenticated user follows a list, more filtering options when loading clips and so on. Non-feature related changes are removing total count from fetched collections because it has been depreacted by Kippt, deprecating support for Ruby 1.8.7 because [it is not supported any more](https://www.ruby-lang.org/en/news/2013/06/30/we-retire-1-8-7/) and improving test coverage of the library.

Read about all the changes from the [CHANGELOG](https://github.com/vesan/kippt/blob/master/CHANGELOG.md) and install from [RubyGems](http://rubygems.org/gems/kippt).
