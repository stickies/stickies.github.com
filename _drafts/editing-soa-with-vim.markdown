---
layout: stickie
title: Editing SOA Projects with vim.
meta: vim soa
---
Service Oriented Architecture ( SOA ) is a way of decoupling your system archictecture. It can present a series of technical challenges to the developer or technical team overall.

As a developer working with SOA, I often find myself working in several different project folders in several different languages at the same time, this can present a series of technical challenges in itself. Continually context switching and changing screens ( or having many tabs open for each project ) can become a balancing act.

### With Vim
Below I have outlined a series of steps that you can take to set up your for this style of development.

First, symlink all your seperate projects into one project folder. For example:

Given I have several project folders, each project is managed as a seperate git repository:

    ~/Projects/backend
      |- .git/index
      |- backend.rb

    ~/Projects/web_service
      |- .git/index
      |- fast_web_service.cpp

    ~/Projects/frontend
      |- .git/index
      |- frontend.js

    ~/Projects/assets
      |- .git/index
      |- assets.css

Create a new workspace directory ...

{% highlight bash %}
mkdir  ~/Projects/stack
{% endhighlight %}

And symlink all the projects into ~/Projects/stack

{% highlight bash %}
ln -s ~/Projects/backend ~/Projects/stack/
ln -s ~/Projects/web_service ~/Projects/stack/
ln -s ~/Projects/frontend ~/Projects/stack/
ln -s ~/Projects/assets ~/Projects/stack/
{% endhighlight %}

Now, when you open the *stack* folder, you can then search using full path globbing.

{% highlight vim %}
:grep 'Company' **/*
{% endhighlight %}

This will search across all your folders for things.

Then use ctags to index everything into a tags file.

{% highlight bash %}
ctags -R --exclude=.git --exclude=log * `echo $GEM_HOME`/gems/*
{% endhighlight %}


Now, if you don't already have it, install the Ctrl-p plugin for vim.

Once this is done, just add this line to your vim config ( ~/.vimrc ):

{% highlight vim %}
let g:ctrlp_follow_symlinks = 1
{% endhighlight %}

Or ..

{% highlight vim %}
let g:ctrlp_follow_symlinks = 2
{% endhighlight %}
