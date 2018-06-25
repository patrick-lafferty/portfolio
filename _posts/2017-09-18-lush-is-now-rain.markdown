---
layout: single_post
title: "Lush is now Rain"
date: 2017-09-18
tags: dev lush rain
excerpt: "Lush was an interactive Racket shell I started working on Summer 2017 to learn Racket. I renamed it to Rain."
issue: 7
---

Lush was an interactive Racket shell I started working on this summer to learn Racket. As it turns out there
is already a shell called Lush, the Lisp Universal Shell. So I decided on renaming the project to Rain: 
the <span style="text-decoration: underline">Ra</span>cket <span style="text-decoration: underline">In</span>teractive Shell.

Along with the renaming I decided to make the latest feature public. The first version of code completion just picked one completion for you and filled in the remaining characters in a light grey text next to the characters you were typing. I started working on an ncurses-like interface, and the first feature is a dropdown listbox that shows all the possible completions similar to completion widgets in other IDEs.

<div class="album">
<img src="/images/blogposts/fizzbuzz-with-dropdown.png">
</div>

To learn more, check out Rain's [documentation](https://patrick-lafferty.github.io/rain/index.html). You can view its [source](https://github.com/patrick-lafferty/rain) and also get the latest release there.