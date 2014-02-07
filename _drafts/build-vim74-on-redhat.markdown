---
layout: stickie
title: Building Vim 7.4 on redhat
meta: vim autocomplete omnicomplete intellisense neocomplete 7.4 vim74 lua ruby
---

## For a basic vim setup with fuzzy search, autocomplete and various other vim  plugins ...

curl -O ftp://ftp.vim.org/pub/vim/unix/vim-7.4.tar.bz2
cd vim74

sudo yum install ncurses ncurses-devel
sudo yum install lua lua-devel
sudo yum install ruby ruby-develop
./configure --enable-rubyinterp --enable-luainterp --with-features=huge
sudo make
sudo make install
