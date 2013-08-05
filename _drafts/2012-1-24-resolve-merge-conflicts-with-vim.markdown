---
layout: stickie
title: Using vim to resolve git merge conflicts
meta: git, vim, merge, conflict
---

{{title}}
THis is inspired by the vimcasts video on the topic

VIM GIT resolve merge

TARGET - active (head) branch
MERGE - the branch that will be merged in

Gdiff
Running Gdiff on conflicted file performs a vimdiff

Opens 3 Windows ( frmo left to right ...)
Target copy
Working Copy
Merge Branch Copy

:ls

fugitive naming convention for mering conflicted files
(buffspec)
working copy:
demo.js
demo

target:
fugitive://path/to/project/.git//2/demo.js
//2

merge:
fugitive://path/to/project/.git//3/demo.js
//3

diffget

( with the unimpaired plugin )

\]c - next conflict

\[c - previous conflict

diffput

:diffput demo | diffupdate

:only - closes all the other windows except for the working copy

do
dp - put change into working copy

Gwrite will commit and delete the diff views

