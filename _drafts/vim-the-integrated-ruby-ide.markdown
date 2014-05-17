---
layout: stickie
title: Vim the integrated Ruby IDE
meta: vim, macosx, ruby, plugins
---

## Vim does it better.

If you are a ruby hacker looking for a really nice IDE for ruby development, put your wallet away and look no further. This guide will run you through setting up a powerful integrated environment for Ruby/Rails and Javascript development.

## Set up your .vim
Vim stores all its configuration in a .vim folder that resides in your home directory on unix.

We want to make a repeatable config that you can store and reuse on different machines.

Please note most of this section is not required in vim and is just my personal preference for
orgainising my configuration and plugins.

First go to the default vim config folder on your computer.
`cd ~/.vim`

`git init`

We're going to refer to this as your vim-repo.

`touch plugin.module`
`touch settings.module`
`touch mappings.module`
`touch syntax.module`
`touch vimrc`

We'll break down the configuration conceptually into different sections.

## Set up vundle
Plugins are a great way of adding functionality to Vim, however they can be a little tricky to manage. There have been several plugins written to improve this process. Popular ones being pathogen and vundle.
vundle works like bundler, which is great for rubyists, so that's what I use.

You'll need to install vundle and include the plugin code in your vim-repo.

Follow the install instructions on the [vundle](https://github.com/gmarik/Vundle.vim) github page.

NOTE: There is also NeoBundle which is a port of Vundle with some additional functionality.

## Set up rvm, install vim-rvm
* Navigate between editor and shell using only vim windowing commands.
* Use ctags in the background with vimproc to keep up to the minute references to your code.
* rb-fsevent ctags and vim, plugin.
* ctags with git hooks
* set up neocomplete
* set up vimshell
* set up file navigation with ctrlp and nerdtree
* set up colorschemes with vim-colorschemes

* use easy-motion in vimshell
* use ctags with jumplist and gemsets

This guide assumes you are using vim 7.4 with lua and ruby enabled, if not, use homebrew to install the latest vim ( 7.4 as of this writing ) with lua and ruby support.

{% highlight bash %}
  brew install vim
  vim --version | grep 'VIM - Vi'
{% endhighlight %}

Firstly, if you install the following plugins, your life *will* be better.

vundle
ctrlp
nerdtree
easymotion
Shougo/vimproc
Shougo/neocomplete
Shougo/vimshell
tpope/vim-rvm
tpope/vim-rails
