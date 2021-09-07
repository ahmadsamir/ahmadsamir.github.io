---
layout: post
title: When you really appreciate clang-format
---

In the KDE repos, a lot of repositories have been formatted using clang-format (almost all of the KDE Frameworks, and IIRC a lot of parts in Plasma, and some apps, and Okular and KActivities (the latter two have had clang-format much longer before the rest of KDE caught up)).

There was this Linux Kernel talk given by Greg-Kroah Hartman where he talked about the importance of formatting patches submitted to the Kernel, they have tools/scripts to format patches according to the coding style used in the Kernel, in that talk he said that the human brain recognises patterns, and because of that it is much easier to read code that is formatted in a regular pattern that you're used to; which in, my experience so far, is pretty much true.

And you can appreciate the code being uniformly formatted in any KDE Frameworks repo; but you have to understand that, to the best of my knowledge, KDE as a community has always had a coding style (especially in the core libraries, formally known as kdelibs, which has been split into separate repos, i.e. went from one monolithic gigantic repo to the "smaller" ones which are the Frameworks nowadays), that was adhered to as much as possible, and any developer in a KDE code review will point out code style issue, so the effect of clang-format there was making something that looked good, look better.

However to really appreciate what clang-format does, try running it on an old codebase, of a project that had one main developer, so it was his coding style/taste. You run clang-format on something like that (with the set of formatting rules from extra-cmake-module/kde-modules/clang-format.cmake), and suddenly the code is transformed, gone are all the unfamiliar indentation, all the pointy hard tabs, all the braces around if/while/for blocks that aren't on the same line; it's like you put the codebase in the washer, set it to the 10 minutes quick wash program, and then got it out, pristine, smelling of soap. And yes, the human brain recognises patterns, reading that code now is somewhat easier.

Thanks go to the developers who work on clang-format (and clang in general, apparently they understand that for a tool like that to survive and prosper it must be open-source, with as many developers working on it, paid and volunteers.... remind you of anything?)
