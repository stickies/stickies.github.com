---
layout: stickie
title: Vim the integrated Ruby IDE
meta: vim, macosx, ruby, plugins
---

## Vim does it better.

If you are a ruby hacker looking for a really nice IDE for ruby development, put your wallet away and look no further. This guide will run you through setting up a powerful integrated environment for Ruby/Rails and Javascript development.

* Set up vundle
* Set up rvm, install vim-rvm
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
