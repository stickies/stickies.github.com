---
layout: stickie
title: Using homebrew
meta: Vim-7.3
---

{{title}}

A lot of the time I see people reach for the binary distribution of a package when the homebrew version doesn't meet the requirement. Most of the time you don't need to do this.

#### GIT GIT GIT!

Homebrew is built using git, and git is actually part of the homebrew workflow, if brew isn't advertizing the version of a package that you want, you might want to try using the git integration!

So for example, say you want an earlier version of wkhtmltopdf ....

    git checkout 21fd4c916a2cb1d902dafb114842adc29028a2ba /usr/local/Library/Formula/wkhtmltopdf.rb

Will give you version 0.9.9.

If you want to track down an actual version you can do so by grepping the git log:

    cd /usr/local
    git log | grep wkhtmltopdf -B 4

This will give you the commit ids.


