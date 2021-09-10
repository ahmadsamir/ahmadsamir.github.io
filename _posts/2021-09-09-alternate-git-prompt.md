---
layout: post
title: Get more out of the window title with Konsole
---

If you use git on a regular basis, you should look into using git-prompt; there is a file called git-prompt.sh that is shipped with git, the location in your setup varies depending on the Linux distribution you're using, for example in OpenSuse it's /etc/bash_completion.d/git-prompt.sh. The [file](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh) is of course available in the upstream git repo.

Following the instructions from the top of that file should give you a very useful addition to the prompt of your shell (the file has instructions for BASH and ZSH). However that is not what this blog post is about; this post is about making the Konsole window title more useful, and by that I mean use the window title to show the current dir path and the info from git-prompt.

Now, the details, (these instructions are for BASH, but I expect it'll work with other shells with a bit of tweaking?):

- Set Konsole to show the title from the shell session on the title bar: <em>Settings -> Configure Konsole -> General -> Show window title on the title bar</em>

- Open <em>~/.bashrc</em> in your favourite editor and add this:
<pre>
# Adds a '*' and/or a '+' character to the window title to show
# the status of the repo, see git-prompt.sh for the details
export GIT_PS1_SHOWDIRTYSTATE=1
</pre>

- Next add this small helper function to set the window title (for what this function does see <a id="function-0" href="#function-1">[1]</a>):
<pre>
__prompt_set_window_title() {
    printf '\e]2;%s %s\a' "$(__git_ps1 " [ %s ]")" "${PWD/#$HOME/\~}";
}
</pre>

- Next add the <b>__prompt_set_window_title()</b> function to the PROMPT_COMMAND (the prompt command in BASH is executed after each command in the shell), by putting this on a new line in <em>.bashrc</em>:
<pre>
PROMPT_COMMAND='history -a; __prompt_set_window_title'
</pre>

For what <code>history -a</code> does see <a id="note-2-0" href="#note-2-1">[2]</a>.

Now when you <b>cd</b> to any dir that has a git repo, the name of the branch will be displayed between the square brackets on the title bar, along with a '*' and/or '+' characters to show the status of the repo.

<hr style="margin-top: 100px"/>
<a id="function-1" href="#function-0">[1]</a> Breaking down <b>__prompt_set_window_title()</b>:

* <code>prinft '%s %s' "first arg" "second arg"</code> printf will replace the first "%s" with "first arg" and the second "%s" with the second arg ...etc

* <code>\e]2;</code> starts the escape sequence to set the window title and <code>\a</code> marks the end of it

* <code>$(__git_ps1 " [ %s ]")</code> for this details about this part, read the docs at the beginning of the git-prompt.sh file; this will put the current branch name between square brackets. Note that if you're not in a dir with a git repo, this will show nothing, i.e. an empty string.

* <code>${PWD/#$HOME/\~}</code> this will put the path of the current dir (i.e. the string stored in the PWD env var) next to the git branch name (and replace */home/username* with *~*), you can remove it if you don't want that behaviour (you'll also want to remove the second "%s" in the *printf* command).

<a id="note-2-1" href="#note-2-0">[2]</a> <code>history -a</code> adds a very useful feature, which I picked up from Mandriva more than a decade ago (thanks, Colin Guthrie :)), it appends the shell history to the history file after you run each command, which means that if you open a new terminal emulator window, you'll find that the shell history has the last command you ran from any other shell session; without 'history -a', the shell history is only appended in one go to ~/.bash_history (or whatever it's called in your distro) after you close the session). You can remove the <code>history -a;</code> part if you don't want that behaviour.
