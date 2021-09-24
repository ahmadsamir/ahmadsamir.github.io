---
layout: post
title: GCC, Clang[d], LSP client, Kate and variadic macro warnings, a short story
---
Kate has had an LSP plugin for sometime now, which uses Clangd. It's a great plugin that brings many code navigation/validation features, akin to those available in Qt Creator and KDevelop.

So naturally since I got it to work, I've been using it. At some point I found out about the **Diagnostics** tab in the **LSP Client** tool view in Kate, which displays useful information; however I also saw that it was plagued by a spam of the following warnings:

<pre>
[clang] Must specify at least one argument for '...' parameter of variadic macro
[qloggingcategory.h:121] Macro 'qCDebug' defined here
</pre>

which is really annoying to say the least, as it adds needless noise.

I just ignored it and moved on; then, by accident, while searching for something in the Extra CMake Module KDE repo I found [this](https://invent.kde.org/frameworks/extra-cmake-modules/-/blob/master/kde-modules/KDECompilerSettings.cmake#L546):
<pre>
if(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    # -Wgnu-zero-variadic-macro-arguments (part of -pedantic) is triggered by every qCDebug() call and therefore results
    # in a lot of noise. This warning is only notifying us that clang is emulating the GCC behaviour
    # instead of the exact standard wording so we can safely ignore it
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-gnu-zero-variadic-macro-arguments")
endif()
</pre>

this explains why those warnings are shown. Adding <code>-Wno-gnu-zero-variadic-macro-arguments</code> to my build flags, I use GCC by default, indeed made those warnings stop. But then GCC started complaining about an unrecognised build flag, which is correct, given that that flag is for Clang.

I started searching for a way to pass that compilation flag to Clangd without involving GCC, and I found [this](https://github.com/clangd/clangd/issues/569#issuecomment-715444829), which led me to [this](https://clangd.llvm.org/config.html).

So, the solution is to create a **.clangd** file in your repo's top level directory (I created it in the parent dir to my KDE Frameworks git checkouts, this way it affects all of them as Clangd searches for that file in all the parent directories of the current source file), and put this in it:
<pre>
CompileFlags:
    Add: [-Wno-gnu-zero-variadic-macro-arguments]
</pre>

The End.

Feel free to tell me about any corrections in my posts, you can send me an email, or better still, use a GitHub issue.
