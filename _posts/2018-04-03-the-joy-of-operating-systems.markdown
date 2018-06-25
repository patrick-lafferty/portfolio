---
layout: single_post
title: "The Joy of Operating Systems"
date: 2018-04-03
tags: dev saturn
excerpt: "It all started with an interesting thought experiment. Starting with just Clang, with no standard C or C++ library, and an empty text file - and no Windows or Linux, what is the minimum amount of code one needs to write to display text on the screen?"
issue: 8
---

# The Thought

It all started with an interesting thought experiment. Starting with just Clang, with no standard C or C++ library, and an empty text file - and no Windows or Linux, what is the minimum amount of code one needs to write to display text on the screen? Assuming text mode with a colour monitor, you just write values to video memory (0xB8000), and voila. Alright that’s easy enough, how about user input? Well that introduces interrupt handling, but if you just want a bootable program that echoes the keys the user presses and that’s it, that’s essentially a few dozen extra lines of code.

# The Rabbit Hole

What about nice TTF fonts? Now things get fun. Let’s say you want to use FreeType to handle all your TrueType font rendering needs. Nice, simple, portable library that only depends on LibC - the C Standard Library, which is non-portable and depends heavily on the host operating system. Which means you need to write it now. So, you try to compile a small demo to see what it is FreeType really depends on. Here's how the process goes:

Okay, we need fread() (and fseek and friends), to read the font.ttf file

<ul>
    <li>well, then we need to add some filesystem support</li>
    <li>but, the filesystem needs a driver that handles reading blocks of data from fd/hdd/sdd</li>
    <li>and we're going to need a way of measuring time since some low-level ATA commands have specific time requirements</li>
    <li>and we also need dynamic memory aka the heap</li>
    <li>so, we need some way to query the actual physical memory available</li>
    <li>and setup a virtual memory manager to handle paging</li>
    <li>then write our heap, and operator new and malloc, and might as well add aligned versions since we'll need them later</li>
</ul>

What about colour? Styles? String handling? It quickly snowballs into a complex project. You reach for familiar functions in the standard library to help, and then realize you need to write them first. Its. All. You.

# The Red Pill

Our hero journeys on to see how deep the rabbit hole goes. In your quest you seek out resources; first among them is the OSDev Wiki. You start absorbing reading material at a blistering rate – several volumes of Intel Software Developer Manuals, ATA spec sheets, Ext2 design docs, the Multiboot specification, PCI, VGA, VBE. Tens of thousands of pages of dry documentation, strict specification, and plenty of historical anecdotes about why things are the way they are.

You start writing the barebones of your kernel. But once you get past the minimum point everyone else gets to, you step out of the forest into a wide-open field, and you realize just how much of an artform there is to the project. I don’t mean the visual design. There are so, so many design decisions you must make when coming up with your OS’s architecture, that the potential state-space is immense. If you don’t decide to follow in Unix’s (or other mainstream OS) footsteps, you really get a chance to explore your creative side. Technological creativity, let the source code be your canvas. By embracing it, your OS will take on and express its own personality and provide a window to show others how you think.

# The Project

My operating system is called <a href="https://github.com/patrick-lafferty/saturn">Saturn</a>. Its source code is under the BSD-3 clause license. The name comes from the Saturn V rocket, and a lot of other names are similarly space themed. I’ve been working on it for 5 months now, with currently 701 commits, 274 files, and roughly 22,000 lines of code in C++ and x86 Assembly. Currently working on version 0.3.0, I’m focusing on my own GUI framework, but I’ll write more about that when the milestone’s reached.

Saturn essentially brings together every important project I’ve ever written. Over the years I’ve worked on a diverse array of hobby projects, and when I encounter a big design choice with Saturn, I can take advantage of my past experiences. I needed to write a shell in C++ - well luckily, I made a shell before using Racket – <a href="http://github.com/patrick-lafferty/rain">Rain</a>. I needed to design a GUI framework – well fortunately, I have a few years’ experience with WPF from my <a href="https://github.com/patrick-lafferty/AssetManager">Asset Manager</a>. I need to design a graphics stack – well auspiciously, I wrote a 3D renderer for my game engine <a href="https://patrick-lafferty.github.io/projects/projectstacks/">Project Stacks</a> – as well as a small GUI framework for the engine, which helps with the previous point. I needed to design a messaging system – well swimmingly, I designed another game engine based around Actors and asynchronous message passing called <a href="https://patrick-lafferty.github.io/projects/chengine/">Chengine</a>. Lexers, parsers, type checkers, messaging systems: check, check, check, check.

<div class="album">
<img src="/images/blogposts/Transcript.PNG">
</div>

People question why I don’t typically finish hobby projects, and they have a good reason to. Its an important skill to see a project all the way to release and ship it, and its obvious why this would be important for prospective employers. But I contend that the breadth of topics, the diverse background of problems and solutions I’ve encountered holds value. When I encountered all those design breakpoints mentioned above, instead of having to pause for days or weeks to research, prototype, explore the status quo, I can just hit the ground running and be productive.

# The Joy

The joy of making your own operating system is that you own the entire stack. All of it. Memory, CPU scheduler, drivers, system calls, GUIs. From top to bottom, you are in control of *everything*. When you write an app for your OS and debug it, you can step through every single line of code between the function that draws a string to the screen...

<ul>
    <li>to the text layout that parses ANSI escape colour codes</li>

    <li>to the text renderer that freads a glyph from the font</li>

    <li>to the ext2 filesystem that issues a read request for the font's inode</li>

    <li>to the ata driver that processes that request</li>

    <li>to the context switching assembly code</li>
</ul>

And realize that register EBX has the value 9 when it should clearly be a 32-bit memory address like 0xA0123450. Then you can walk allllll the way back up to see exactly where things went wrong. Ohh, at step 3 a pointer was passed but physical memory wasn't committed to the virtual address so reading that memory actually gave you the flags (the 9) for that virtual page, instead of its contents.

No longer do you run into situations where it’s out of your hands - wait for a OS service patch, wait for a new release of some third-party library that fixes the bug. The only limit is you.

Being in complete control of the entire software stack is a liberating feeling.

