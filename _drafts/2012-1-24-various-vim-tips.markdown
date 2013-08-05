---
layout: stickie
title: Various vim tips
meta: git, vim, merge, conflict
---

{{title}}
## VIM VIM VIM VIM VIM VIM VIM VIM VIM VIM VIM VIM VIM VIM

### Fave commands
Give match data to grep:

    g* ( with cursor over match candidate )
    :grep Ctrl-R / ## to paste the buffer

Quickly navigate the quicklist:
iIf you've been using Vim grep.

    :cn # next match
    :cp # previous match

    # Then, so you don't have to keep stretching for the command
    @: # repeat the last
    @@ # keep on repeating

Save all buffers:

    wa

Delete to end of line

    D

Output shell to buffer:

    :%!git branch -a

Switch split window

    Ctrl - W - ARROW KEY

Folding

    zi - enable/disable all folds
    za
    zc
    zo
    zA
    ZC
    zO

Gundo

    fn F5
    g-
    g+
    :earlier {N}s

Fugitive

    Gstatus
    Ctrl n
    Ctrl p

...to traverse lines

Use the - ( minus key ) to stage and unstage
Use with Gdiff and Folding

    Shift c - open the commit window

with Gdiff you can stage changes to the index ( left buffer ) with :Gwrite or you can pull changes from the index with :Gread when you are in the working copy buffer

    :diffget

    :diffput

If you're in the index version with the Gdiff command and there are changes between the two files, use diffput  to reflect the changes into the working copy

## Using Glog to view the history of a file in git.

Fugitive lets you view the git log with the handy %Glog% command. This command outputs to the quickfix buffer. You can traverse the list by using either

    :copen #open the quickfix pane
    :cn # open the item in the quickfix list
    :cp  open the previous item in the  quickfix list
    :cnf
    :cpf

    Glog

    Glog --

If you want to traverse the history of the current file, you can use:

    Glog -- %

This will generate a list of all the commit objects in which the current file is referenced. You can then traverse through these to see the history or changes by using

    :cn
    :cp

See line by line when and by whom a file was last changed

    Gblame

Working with the quickfix list
I recommend installing unimpaired.vim, another plugin by Tim Pope. This provides lots of useful pairs of mappings, including a handful that make working with the quickfix list much easier:

    [q
    :cprev
    jump to previous quickfix item
    ]q
    :cnext
    jump to next quickfix item
    [Q
    :cfirst
    jump to first quickfix item
    ]Q
    :clast
    jump to last quickfix item

### Resources

http://vimcasts.org/episodes/fugitive-vim-exploring-the-history-of-a-git-repository/

Gitv instructions
https://github.com/gregsexton/gitv/blob/master/doc/gitv.txt


Find method in instructions
Shift K


Buffers
bn - next loaded buffer
bp - previous loaded buffer
:ls  - buffer list
1 Ctrl ^ - number and ctrl x loads that buffer into the current window


~TAGS
vim has integration with ctags for rapidly finding methods

:ta has_many
:ta BelongsTo
:sta my_thing --- to open in a split
You can even use a regex to find matching

:ta /^validates_\*

Then navigate through them

:ts - show the list
:tn, :tp - next and previous
:tf - first tag of the list
:tl - last tag of the list

To jump through to the definition when the cursor is over the fname type:
g Ctrl]

