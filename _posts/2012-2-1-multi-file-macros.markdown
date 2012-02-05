---
layout: stickie
title: Various vim tips
meta: git, vim, merge, conflict
---

{{title}}
VIM VIM VIM VIM VIM VIM VIM VIM VIM VIM VIM VIM VIM VIM 

:argdo normal @m

        .sub-banner{:style => "background: url('/assets/investors/without_text/better-for-investors2.png') no-repeat center top;"}

not used          // %s/.sub-banner{.*\(investors\|businesses\)\/\(\w.*\).png/
not used          // :%s/.sub-banner{.*\/\(\w.*\).png.*/\1/
not used          // :%s/.sub-banner{.*url(\('.*\/\(\w.*\).png'\).*/.sub-banner.\2/
// :%s/.sub-banner{.*url(\('.*\/\(\w.*\).png'\).*/.sub-banner.\2\r.\2 { background: url(\1) no-repeat center top; }/


Editing vim macros ( recordings )

You can open a buffer and just paste the contents of the regsiter that you assigned the macro to, then edit it and yank it back into the register
