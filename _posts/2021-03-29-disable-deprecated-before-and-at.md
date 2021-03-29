---
layout: post
title: -DKF_DISABLE_DEPRECATED_BEFORE_AND_AT=0x054F00 ?
---
-DKF_DISABLE_DEPRECATED_BEFORE_AND_AT=0x054F00

<pre class="code">
        0x054F00
0x05    0x4F   0x00
</pre>


In terminal:
<pre class="code">
$ printf '%d %d %d\n' 0x05 0x4F 0x00
5 79 0
</pre>

Using <code>std::cout</code>:
<pre class="code">
std::cout << std::dec << 0x05 << " "
                      << 0x4F << " "
                      << 0x00 << std::endl;

Outputs:
5 79 0
</pre>

And:
<pre class="code">
std::cout << std::hex << QT_VERSION_CHECK(5, 79, 0) <<  std::endl;

Outputs:
54f00
</pre>

<pre class="code">
                   54f00
Hexacdecimal: 54    4f   00
Decimal:      5     79   0
</pre>

I hope you understand now.
<p>&nbsp;</p>
<hr />
<b>Side-note:</b> I would like to thank Ilya Bizyaev, who took the time to file an issue which resulted in fixing the config files of my blog, so that links created to my blog on https://planet.kde.org actually work.

This is very good because it actually has two results, a) I fixed the config and b) it gave me proof that someone actually read a post from this blog.

My main goal when I created this blog was to share info, so even if no one reads a new post right after I create one, it could help someone doing an online search some years from now (including myself, since I also think of this blog as my little personal wiki, something like this great all-things-about-BASH wiki https://mywiki.wooledge.org , but much smaller (and not as useful, yet.... ?) :)
