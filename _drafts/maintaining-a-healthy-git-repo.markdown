---
layout: stickie
title: Maintaining a healthy git repo
meta: git, compression, repo, code
categories: git
tags: git
---
When you have many developers committing many branches to a large git repo things can get messy fast, depending on the strategy you use for merging and branching and the amount of work you do with the repo.

Through the course of a repositories life, you can generate a lot of objects through the creation of different branches, files, and commits. This can slow down the common tasks that you perform with git and git related tools.

There are some useful strategies for keeping your local repo fast & tidy.

### Tidying up the local repo.
*Stack Overflow has a few things to say ...*

[http://stackoverflow.com/questions/6127328/how-can-i-delete-all-git-branches-which-are-already-merged](http://stackoverflow.com/questions/6127328/how-can-i-delete-all-git-branches-which-are-already-merged)

The following command will delete all local branches that are merged.
{% highlight bash %}
  git branch --merged | grep -v "\*" | xargs -n 1 git branch -d
{% endhighlight %}

*And there are some github projects for speeding up*
[https://github.com/arc90/git-sweep](https://github.com/arc90/git-sweep)

### Optimizing a slow repo.
[http://stackoverflow.com/questions/3313908/git-is-really-slow-for-100-000-objects-any-fixes](http://stackoverflow.com/questions/3313908/git-is-really-slow-for-100-000-objects-any-fixes)
{% highlight bash %}
  git repack -ad
{% endhighlight %}

{% highlight bash %}
  git gc --prune=now
{% endhighlight %}

* how do you validate connectivity of objects?
{% highlight bash %}
  git fsck
{% endhighlight %}

* how do you see how many objects are in your git repo?
{% highlight bash %}
  git count-objects
{% endhighlight %}
> 5132 objects, 22004 kilobytes

* diagnose and clean up dangling blobs and objects?

git count-objects
> 5132 objects, 22004 kilobytes

{% highlight bash %}
git fsck
> Checking object directories: 100% (256/256), done.
> Checking objects: 100% (254767/254767), done.
> dangling blob eca68806715cd74cff244d70584680fb29e375b4
> dangling blob 6da7c0ffce826481369641d6f2953d2659e219d6
> dangling blob 94dab0eca6a8d4b11e2b455f6a2568588f2fa6db
> dangling blob 0ff7a01c7a322327689afd64643596bd80a9e0f7
...
{% endhighlight %}

{% highlight bash %}
git gc --aggressive
> Counting objects: 254139, done.
> Delta compression using up to 8 threads.
> Compressing objects: 100% (244681/244681), done.
> Writing objects: 100% (254139/254139), done.
> Total 254139 (delta 183349), reused 67388 (delta 0)
> Removing duplicate objects: 100% (256/256), done.
> Checking connectivity: 254139, done.
{% endhighlight %}

{% highlight bash %}
git fsck
> Checking object directories: 100% (256/256), done.
> Checking objects: 100% (254139/254139), done.
{% endhighlight %}

{% highlight bash %}
git count-objects
> 0 objects, 0 kilobytes
{% endhighlight %}

* Delta compression
