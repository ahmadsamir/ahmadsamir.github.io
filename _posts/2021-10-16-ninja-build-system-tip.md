---
layout: post
title: ninja Build system, and renamed files
---
I've been hearing about `ninja`, I had looked at it some time in the past, and did some local basic benchmarking (using `time`), and didn't find a huge difference in build times, both from scratch and incrementally. I tried ninja again recently and found one feature that sells it pretty well to me, it can show the build progress on one line in the terminal.

So I switched to `ninja` to see how that goes, seems to work OK, but a new tool, new quirks, right? :).

I happened to be using git interactive rebase, and so was jumping between commits, some files were renamed (with `git mv`), that's when ninja failed to build incrementally, clearing the build dir and starting anew, worked fine. But incremental builds are very useful, otherwise the waiting times between the usual "modify code, compile, test, repeat" cycles would be longer, and those waiting times aren't my favourite pastime. The workaround turned out to be simple, `touch CMakeLists.txt` file in the top source dir, then run ninja again and it should pick up the changes.

FWIW, that `touch CMakeLists.txt` method works too if say, you built KIO against some other Framework's libfoo.so.n, then libfoo.so.n was updated to .n+1, which would make the incremental build fail (with `make` at least, haven't seen that with `ninja` (yet?)), touch the file, and it picks up the changes (probably since it will check the build dependencies again).

Have fun hacking at code.
