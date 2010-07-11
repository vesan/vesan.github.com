---
layout: post
title: "Vim plugin documentation problem with Git"
tags: [vim, git, tips, plugins]
---

I use [Pathogen](http://github.com/tpope/vim-pathogen) and Git submodules to handle my Vim plugins with this [setup](http://www.allenwei.cn/tips-using-git-submodule-keep-your-plugin-up-to-date/). It works great but I get problems with the Git submodules after I create documentation for the plugins by running the Vim command `call pathogen#helptags()`.

When I run `git status` it says that the condition of the submodule plugin is dirty (most recently this happened with Tabular and textile.vim). This is caused by the `doc/tags` file created inside the plugin. I have solved this by editing the `.git/info/exclude` file inside the plugin's directory and adding the line `doc/tags` to it.

Most plugins have their `doc/tags` file already created and committed to Git so this problem doesn't happen that often, but if somebody has better solution for this, please leave a comment.

