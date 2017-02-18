---
layout: default
title: "Dev Environment"
date: 2016-11-3
tags: dev 
---

This post describes my current web dev environment for a friend that was curious about it. I had these requirements when coming up with it:

* it had to be fast and light on memory *and* disk space - my box has a 1.33Ghz quad-core Atom, 2GB of ram and 32GB of space of which Windows and friends take up a big chunk
* it had to be fully featured and $EDITOR must be or have Vim bindings

I based the environment on running Arch Linux inside VirtualBox. I prefer Arch because I love pacman, out of all the distro package managers its the only one I feel that just.always.works, I've never got it into a critically broken state (hello aptitude, yum), and allows me to stay on the bleeding edge with new updates. Arch is also minimalistic, you pay for what you get allowing me to keep the virtual image small. A nice bonus about running a VM is that you can save the exact state of the machine before you shutdown your real one. So at night you just stop the VM and in the morning when you start up you are right back where you left off, no need to startup your editor and open the files you left off at. 

### Editor

For my editor I use Vim currently with powerline and vim-surround. I can't <strong>not</strong> use Vim anymore, I keep learning new things to improve my experience. For example: sometime after starting some HTML5 practice, I learned about the tag blocks `at` and `it`. Adding `at` to a command selects the entire element from the beginning tag to the ending, while `it` selects everything except the starting/closing tags. So with the following html:

{% highlight html %}
<div>
    <p>...</p>
    <form>...</form>
    <section>...<section>
</div>
{% endhighlight %}

and the cursor is on the beginning div line,

* `>at` and `<at` increases/decreases the indent respectively of the entire div
* plus the standard `y`ank, `p`ut, `d`elete and `c`hange operators (and all the rest) give you a lot of control, say you wanted to change everything in the div: just type cit in normal mode and the inner contents are deleted and you are put in insert mode inside the div

With vim-surround you can do things like surrounding/changing/removing things around things. So if we wanted the `<p>` and `<form>` to be inside a div we could do `ysj<div>`, or if we wanted to switch the `<p>` to a `<div>` we could do `cst<div>` with the cursor inside the `<p>`. For the yank/put/delete/change ops I originally wrote them as (y)/(p) etc but then decided to switch to use highlighting so with a simple ``cs) ` `` they changed to `` `y` `` /`` `p` ``.

### Shell

I've been using bash for years, so I picked zsh to learn something new. Zsh has some cool features, like:

* extended globbing (`grep "foo" **/*.js` will recursively search all .js files in subdirs)
* [zmv](http://zshwiki.org/home/builtin/functions/zmv), zcp are replacements for mv/cp that you can define patterns to rename many files at once
* autocompletion (of arguments (git st`<TAB>` will show you all args that start with st), paths (ls /h/p/d`<TAB>` could expand to /home/patrick/downloads), commands)

Searching "why zsh" will bring up a number of good sites with examples. Along with zsh I use [powerline](http://powerline.readthedocs.io/en/master/index.html). It basically makes the prompt look cool and configures it to show useful information, along with a second prompt to the right of the screen. Finishing up the trifecta comes [Tmux](https://tmux.github.io), a terminal multiplexer. Basically it allows you to split up the screen to have multiple shells running in splitscreen mode, among other features. In the screenshot below I split the screen vertically once then split the right side horizontally. The large left pane hosts Vim, the top right is running a jekyll server that rebuilds this blogpost as I change it, the bottom right is for misc things, `wc -l *` in this example.

![Dev environment screenshot](/images/blogposts/dev_environment.png)

## Runners up

Choices that didn't quite make the final cut but are still very nice are listed below.

### Visual Studio Code + MSYS2

[VS Code](https://code.visualstudio.com) is *really cool*. It runs on Windows/Linux/Mac. Its [open source](https://github.com/Microsoft/vscode) using the MIT license. Intellisense and debugging and extensions everywhere! Stop reading this and go check it out!... *But*, when using the vim extension (which we know is essential) on my mighty tablet, I could type out a line of code, take a sip of coffee, look out the window, look back and the line might just finish showing up. Maybe someday.

### Windows Subsystem for Linux

Special mention goes to [WSL](https://msdn.microsoft.com/commandline/wsl/about) or informally Bash on Ubuntu on Windows. With it you have the entire Ubuntu user space natively accessible on Windows. Once WSL is setup you can go to bash and apt-get something, it will get the native binary from Ubuntu's servers. So you could in theory run an old 16-bit Win/Dos program inside wine inside bash on windows - Windows on Linux on Windows... *we need to go deeper*. Caveat: it only works on 64-bit editions of Windows. Who cares, its $CURRENT_YEAR! Well this cheapo tablet I work on is limited to 32-bit Windows, even though it has a 64-bit cpu. Which is why I opted for the above VirtualBox config. WSL has some teething issues (still limited to 16 colours, and support for inotify hasn't been released yet but is coming soon) but its something I am very much looking forwards to using when I can.


