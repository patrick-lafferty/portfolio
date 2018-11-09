---
layout: single_post
title: "Saturn: One Year Later"
date: 2018-11-08
tags: dev saturn osdev
excerpt: "Reflections on a year of developing my own operating system"
issue: 12
---

<div><em style="text-align: center;display: block;">"He mentioned something interesting. Patrick is pursuing new research. He claims that what they're doing in Canada is the missing piece: <br>A system to surpass MS-DOS"</em></div>

<img src="/images/blogposts/saturn_logo_cropped.png">

Its been one year since I started work on my operating system [Saturn](https://github.com/patrick-lafferty/saturn/). This is a reflection on work achieved so far, stats, and a look towards the future.

# By the numbers

Since Oct 2017, there have been 870 commits, averaging two commits per day over the past 382 days. According to GitHub I've made 67,391 additions and 22,244 deletions, across 335 files. The codebase is 92% C++, 2.9% Assembly, 0.7% [c]makefiles/shell scripts, and the rest GitHub wrongly attributes to other languages. There have been 33 issues created with 27 being closed and 6 still outstanding. One developer, and a partridge in a pear tree.

# What was achieved

My greatest achievement with Saturn was the start of the UI framework with Apollo. Having a [convenient language](https://github.com/patrick-lafferty/saturn/blob/master/src/applications/transcript/layout.h) for declaring UI layouts proved to be a great asset in rapid prototyping. If nothing else I think Apollo is the point where Saturn left the OSDEV introductory cradle and started becoming serious, creating its own identity. [Vostok](https://github.com/patrick-lafferty/saturn/blob/master/src/services/virtualFileSystem/vostok.h) was another interesting achievement, exposing complex nested objects in the virtual file system. It allowed programs to interact with eachother through the filesystem, invoking eachother's functions.

Saturn supports multi-tasking with both ring0 and ring3 (user) tasks, separate address spaces per task. It provides inter-process communication via asynchronous message passing. The scheduler supports multiple priority groups, so latency-critical tasks like IRQ handlers can pre-empt any running task, input and UI services can respond to user input quickly. 

# What's happening now

Versions 0.1 to 0.3 were an exploratory dive into the world of operating systems. I essentially did a depth-first design, at each step implementing a minimal feature set in order to proceed to the next higher level, to see how far I could go and understand roughly how long it would take to reach my goals. At each step of the journey I made notes about future considerations and things I'd like to add/explore. So at each fork in the road even though I picked the shortest path, there's notes taped to the signposts indicating what should be done when I return.

Having successfully reached a far enough goal, I made the decision to go back to the beginning, and revisit each note. Where quick fixes, hacks and TODOs sufficed for a speed-run to the end zone, now there are proper complete implementations. Saturn is undergoing a major overhaul, not just to x86-64 but to a cleaner design. From the beginning I focused on correctness and sustainability, which led me to create a testing framework called [Preflight](https://github.com/patrick-lafferty/saturn/blob/master/src/kernel/arch/x86_64/misc/testing.h) ([examples](https://github.com/patrick-lafferty/saturn/blob/master/test/kernel/arch/x86_64/misc/avl.cpp)). Each feature that gets rewritten (or ported over with minimal changes) is getting its own suite of tests.

The kernel now has a two stage loader. The loader separates startup concerns from the kernel, creating a clean separation. The loader ensures that the kernel is either delivered a valid working environment, or the boot sequence is stopped. Loadable ELF programs are supported from the very beginning (because the kernel is an ELF program, as are services and drivers it requires). The infrastructure of IDT/GDT/ISRs and physical/virtual memory have been updated for the 64-bit environment.

# What's the future hold

The virtual filesystem will be upgraded to support task namespaces. Using Vostok will be easier with interface definitions that encapsulate filesystem calls. The majority of my effort will be with Apollo and the graphical interface. I aim to develop a rich, dynamic environment like Pharo or Oberon where the lines between shell, UI and programs are blended, and the system is openly modifiable and examinable. 


Check out [Saturn](https://saturn-os.org)'s website which includes an online documentation browser. Visit Saturn's [repository](https://github.com/patrick-lafferty/saturn) to see all of the code. 