---
layout: single_post
title: "SMP is as easy as 1-2-3"
date: 2018-07-20
tags: dev saturn osdev
excerpt: "And other hilarious jokes you can tell yourselves"
issue: 10
---

<div><em style="text-align: center;display: block;">(And other hilarious jokes you can tell yourselves)</em></div>

Symmetric multiprocessing (<a href="https://en.wikipedia.org/wiki/Symmetric_multiprocessing">SMP</a>) is one of the big features I'm currently exploring for <a href="https://saturn-os.org">Saturn</a>. I implemented multi-tasking very early on as that was a key part in the microkernel design, however up until now everything ran on a single core. Its the classic concurrency vs parallelism comparison, just because things are running concurrently doesn't mean they're running simulatenously. Adding SMP support requires careful upfront design (whoops) or lots of refactoring down the road. Before making any further progress with Saturn I wanted to make sure it had a solid core foundation, so I started to refactor now while that was still feasible.

# In the beginning there was the bootstrap processor

I was really curious how multicore actually worked and how you set it up. I mean we all know about multithreading and multiple processes and distributing work across cores, but how does the OS actually orchestrate that? What happens when the computer first boots up? 

At first glance it seems pretty straightforward. At startup the firmware runs some tests and does some behind the scene work, and eventually passes control over to the bootloader which then loads the OS.  When you have a multicore CPU, the firmware picks one of the working cores as the "main" CPU, called the "bootstrap processor" (BSP), and considers the remaining cores (or hardware threads in the case of SMT) as "application processors" (AP). It starts up each AP in <a href="https://en.wikipedia.org/wiki/Real_mode">real mode</a>, clears the interrupt flag and then halts each AP. Meanwhile the BSP carries on towards entering your kernel.

# Interprocessor Interrupts

At this point all of our APs are halted (not running any code), and wont respond to normal interrupts/exceptions. Luckily there is a special type of interprocessor interrupt or IPI that is used to facilitate communication between CPUs. By sending a certain sequence of IPIs we can wake up our APs and get them to do useful work. To do this we need to make use of a <a href="https://en.wikipedia.org/wiki/Trampoline_(computing)">trampoline</a>.

Remember that I said APs start in real mode? 16-bits of non-protected, non-paging, segmented fun. Our BSP at this point is either in 32-bit protected or 64-bit long mode with paging enabled. We can't just have the BSP allocate a bunch of data structures and hand them off to the AP, because it can't address them because we can only access the first 1MB of RAM. A trampoline allows us to get around this.

A trampoline is a small bit of code + data that configures an AP to be useful. It basically does a readers digest version of what the kernel had to do - setup a GDT, enable paging etc - without really caring about the details. Ie it just fills out the bare minimum needed to get to protected mode, because once there we'll replace that stuff or use what the kernel already setup.

The BSP allocates a small chunk of memory at a low address which stores important addresses for the AP to use, among them a stack pointer, a function address to call once in protected mode, and a flag it continually will check for the AP to modify to indicate progress. The BSP and AP communicate with eachother by modifying the flag and waiting for it to change to a specific value before continuing on. Once this dance is finished the AP halts and waits for the BSP to finish initializing all the remaining APs and then send it an interrupt.

# Locad nda lok (or: Lock and load)

Now that we have multiple processors that want to process things we need to synchronize anything that could be accessed from multiple tasks. Due to the micro part of the microkernel, there aren't many data structures that could be modified by multiple processes. Said structures were global variables of convenience, and now I had the opportunity to redesign them. 

Due to the nature of these kernel variables and how they were accessed, I decided to implement <a href="https://en.wikipedia.org/wiki/Spinlock">spinlocks</a>. A spinlock does as advertised: "can we aquire this lock? No? Okay, can we aquire this lock now? No? Okay,...".  Essentially it just continually checks a conditon in a loop, best used if you expect to wait only a few hundred cycles. Mutexes and more elaborate mechanisms have their place, the middle of an interrupt service routine ain't one of them.

The spinlocks are used in a few key areas to synchronize access to a task's mailbox, a scheduler's run queue, or in task creation. What happens if you don't use locks? The header for this section is a lighthearted example, more realistically the entire system crashes and you're sitting in GDB with no clue what happened.

# Schedulers, Directors, and Bears, oh my

Originally Saturn had a single scheduler which controlled what task was currently running. Adding more CPUs to the picture meant supporting multiple schedulers, each with their own separate run queues and block queues. The first design iteration had a complicated process when a scheduler noticed a blocked task could be run. It would examine all of the other schedulers to see if any could accept this task and then inject the task onto that scheduler's run queue. Likewise if the scheduler ran out of tasks it would try to steal from another.

This worked, in a kinda sorta every second tuesday of each month way. I decided on a more clean approach by adding a second level scheduler: the director. A director is the main/meta scheduler that schedules the schedulers. Instead of schedulers each having their own blocked queues that had to be managed invididually, the director would be the single point of control for blocked tasks. Schedulers still have their own sleep queues for tasks that sleep for a very short amount of time.

This simplified things greatly while being a lot safer. Now there was a single point of entry for scheduling tasks: give it to the director, and it will find the best scheduler for you.

# Aftermath

This post marks the 800th commit to Saturn, making it one of the longest running projects I've started. Currently I'm taking on the 64-bit rewrite and will be adding extensive testing along the way. You can see the <a href="https://github.com/patrick-lafferty/saturn/">source code</a>, or to learn more visit <a href="https://saturn-os.org">saturn-os.org</a>.

If you have any questions or comments, corrections or suggestions, criticisms et cetera, I’d love to get in touch by email or in the comments below. I’m always open to learning new things and correcting bad things.

Part two of my series *The Joy of Operating Systems*, where I write about my experience writing my own operating system *Saturn*.

* Part 1: <a href="https://patricklafferty.ca/blog/2018/04/03/the-joy-of-operating-systems/">The Joy of Operating Systems</a>