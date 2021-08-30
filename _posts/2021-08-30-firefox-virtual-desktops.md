---
layout: post
title: Firefox and virtual desktops
---

At some point Firefox started remebering which window was open on which desktop, which meant that if you're running KDE or GNOME ...etc and open several Firefox windows on different virtual desktops, when you restart Firefox, each window will be restored to the desktop it was open on. Apparently [that started with Firefox 77](https://bugzilla.mozilla.org/show_bug.cgi?id=890125).

Which is nice... or not, depending on your use-case; so, the good news is that this is actually configurable via a pref ```widget.disable-workspace-management``` (you can open the config page in Firefox by going to ```about:config```), that [pref was introduced in Firefox 81](https://bugzilla.mozilla.org/show_bug.cgi?id=1628742).

It took 1-2 restarts to see the changes, but it works.
