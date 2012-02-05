---
layout: stickie
title: Vim multi file search replace
meta: 
---

{{title}}
This is an example of how you can use vim to do multi file search and replace

This command is an example of how you group spaces in vim and use them in a multi line search replace

args app/views/cms_pages/**/*

argdo %s/\(\s*\)\(\.top_margin\)\n\(\s*\)\(\.span[8|9|16]\)/\1\212\r\3\4/e | update

%s/\(\s\s\s\s\.row\.top_margin\)\n/\112\r/e | update
argdo %s/\(\s\s\s\s\.row\.top_margin\)\n/\112\r/e | update
%s/\(\s\)
%s/\(\s*\)\(\.top_margin\)\n\(\s*\)\(\.span[8|9|16]*\)/\3
%s/\(\s*\)\(\.top_margin\)\n\(\s*\)\(\.span[8|9|16]*\)/\1\212\r\3
%s/\(\s*\)\(\.top_margin\)\n\(\s*\)\(\.span[8|9|16]*\)/\1\212\r\3/
argdo %s/\(\s*\)\(\.top_margin\)\n\(\s*\)\(\.span[8|9|16]*\)/\1\212\r\3\4/e | update
%s/\(\s*\)\(\.top_margin\)\n\(\s*\)\(\.span[8|9|16]+\)/\1\212\r\3\4/
%s/\(\s*\)\(\.top_margin\)\n\(\s*\)\(\.span8|9|16\)/\1\212\r\3\4/
%s/\(\s*\)\(\.top_margin\)\n\(\s*\)\(\.span[8|9|16]*\)/\1\212\r\3\4/


/\(%button.btn.brand\)\n\(\s\)*%a\(.*\)\n\(\s\)*\(.*\)
/.action-link\r\2%a.btn\3 \5

/\(%button.btn.brand\)\n\(\s*\)%a\(.*\)\n\(\s\)*\(.*\)/\.action-link\r\2%a.btn\3 \5/

371,378s/\(s*\)%\S\(\S\)\n/\2/
371,378s/\(s*\)%\S\s\(\S\)\n/\2/
371,378s/\(s*\)%\S\s\(\S*\)/\2/
s/^\s*//
s/^\s*\(\w
s/^\s*\(\w*\)\r/\1/
s/^\s*\(\w*\)\n/\1/
43,46s/^\s*\(\w*\)\n/\1/
s/^\s*\(\w*\)/\1/
s/^\s*\(\w*\)/ \1/
/%a/+,/%a/-s/%u/%BLAH/
/%a{:href => ".*"}/+,/%a{:href => ".*"}/-s/%u/%BLAH/
/%a{:href => ".*"}/+,/%a{:href => ".*"}/-s/%u/\%BLAH/
/%a{:href => ".*"}/+,/%a{:href => ".*"}/-s:%u/%BLAH:
s:(\s*)//
s:(:s*)::
s:(\s*)::
s:\(\s*\)::
w
110,118s:^\(\s*\)[^%]\(\w*\):\2:
