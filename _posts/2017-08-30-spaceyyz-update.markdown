---
layout: default
title: "SpaceYYZ Update"
date: 2017-08-30
tags: dev spaceyyz
---

[SpaceYYZ](https://patrick-lafferty.github.io/spaceyyz/) is a small demo app I wrote a year ago to learn AngularJS. Recently I decided to revisit it and apply what I've learned since then to improve the project.

## Starting out - Proper Foundation

Before modifying the actual app, I needed to setup a stable foundation to build upon. This involved two key areas: a proper build system/task runner, and a standardized file layout. Originally I just used the scripts section in package.json to run the builds, tests and misc tools. This however proved to be fragile, both because said scripts can only be one liners chained with && or ;, and because occasionally builds would silently fail and the previous output would remain, leading to an inconsistent mental state of the app. 

I looked at various task runners and weighed the pros and cons, before settling on plain old makefiles. Why makefiles? I know make and its already installed so theres one less thing to configure. To enforce consistent and up to date builds I made sure the make targets removed all existing output files before running webpack. With that in place, I made sure to add targets for linting and testing so I could make large changes to the app and be confident that I wasn't breaking anything. Then I focused on cleaning up the directory layout so that files could be found in their expected places.

## Modernizing

Once the foundation was in place, I started work on converting the app to use ES6. This allowed me to make use of great new features such as modules, block scoped variables, classes, arrow functions, and new library features like Array.find. The end result was a leaner, more declarative codebase. A similar effort was spent bringing the HTML/CSS side up to date to make use of modern layout using Grids and Flexboxes, semantic elements (article, section, nav, main), as well as using [Less](http://lesscss.org/) and [BEM](http://getbem.com/). Finally minor defects like inline styles were replaced with classes.

One big issue SpaceYYZ had was that it wasn't responsive -  It was designed solely for desktop use. The design in general was unappealing. 

<div class="album">
<blockquote class="imgur-embed-pub" lang="en" data-id="a/K9kAO"><a href="//imgur.com/K9kAO">SpaceYYZ 1.0</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>
</div>

I wanted to make the app responsive so it would work well on any device. I decided to overhaul the entire app's UI to make it responsive while having a minimal, elegant sort of futuristic look. In doing so I cut out as many unnecessary or incomplete UI elements as possible to make the layout as minimal and clean as possible.

<div class="album">
<blockquote class="imgur-embed-pub" lang="en" data-id="a/MsQaU"><a href="//imgur.com/MsQaU">SpaceYYZ 2.0</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>
</div>

## On becoming Responsive

[Grid](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout) and [Flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Using_CSS_flexible_boxes) made it very easy to have a responsive layout. With media queries you can easily adapt your grids to have more or less columns, add or remove gaps in between columns, and switch flexboxs to arrange items vertically instead of horizontally. All this with just CSS, without needing to change the structure of the HTML files. I like Grid so much I made a [lab](https://patrick-lafferty.github.io/labs/#Grid) dedicated to it. From there it was a matter of setting proper font sizes, margins, paddings etc as appropriate for the media breakpoints.