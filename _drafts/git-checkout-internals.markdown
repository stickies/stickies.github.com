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

### The baseline tree

### The working directory ( aka: working tree )

### The index ( aka: staging area )
