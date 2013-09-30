---
layout: stickie
title: Git Checkout Internals
meta: git
---

I want to take some time to study and understand [the libgit2 checkout internals document](https://github.com/libgit2/libgit2/blob/development/docs/checkout-internals.md) and some of the terminology it is using as I think this can help me understand the fundamentals of git internals better.

`Checkout has to handle a lot of different cases. It examines the differences between the target tree, the baseline tree and the working directory, plus the contents of the index, and groups files into five categories:`

Firstly lets dissect the four 'states' that this sentence mentions.

* Target tree
* The baseline tree
* The working directory
* The 'index' ( and contents of )

### The target tree
  "Git is a content-addressable filesystem. Great. What does that mean? It means that at the core of Git is a simple key-value data store." - Schacone
  [A good explanation of what a tree is can be found here](http://git-scm.com/book/en/Git-Internals-Git-Objects)

Essentially, a tree in git is equivalent to a directory structure, with each node in the tree pointing either to another (sub) tree or a blob ( actual content ) object.

So, the target tree is as you may expect: where the resulting code and files will be after the checkout.

### The baseline tree

### The working directory ( aka: working tree )

### The index ( aka: staging area )
