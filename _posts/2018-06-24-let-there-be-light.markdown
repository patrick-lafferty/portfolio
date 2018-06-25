---
layout: single_post
title: "Let there be light"
date: 2018-06-24
tags: dev 
excerpt: "Five lessons learned from redesigning my blog"
issue: 9
---

Recently I decided to revisit my personal site and overhaul its design.
These are the lessons I learned, the logic of the
old and new designs, Paint scribbles on screenshots, and general advice.

# Lesson One: Let there be light

Every editor I use is configured for a dark colour scheme. As I spend most of
my time in that environment, I grew accustom to it and preferred dark schemes,
which my personal design reflected. As it turns out, many people prefer
dark-on-light colour schemes to light-on-dark when reading general text.
It can be less straining and easier to read for a large segment of the population.
With that in mind I set out to do a complete makeover.

The "Before" picture:

<div class="album">
<img src="/images/blogposts/dark_to_darker.PNG">
</div>

Note how it goes from a dark blue background to an even darker black
text background. The "After" picture:

<div class="album">
<img src="/images/blogposts/bright_to_brighter.PNG">
</div>

Note how it goes from a light blue background to an almost white
text background. Text is almost always black on a bright white,
when the contrast is lower with black-on-bright blue I make sure
the font size is large to make up the difference.

Looking at the numbers[1], the old dark design had a contrast ratio of
5.48:1, while the new one varies from 12.97:1 to 17.51:1. The mentioned
black-on-blue parts have a contrast ratio of 5.97:1, so there is a marked
improvement all around.

1: https://webaim.org/resources/contrastchecker/

# Lesson Two: Pick a proper colour palette

I don't know much about colour theory except that I like blue, and some
now irrelevant details about radiosity. I know enough to know that you
can't just pick colours willy-nilly, or use a large number of them without
rhyme or reason. Websites like Paletton[2] help you create a colour palette using
a variety of different schemes like monochromatic, triads and tetrads.
For non-artists like me, spend a lot of time deciding on what best fits 
your design, then take the handful of colours it gives you
and use only those, except for perhaps small accents.

2: paletton.com

# Lesson Three: Design for your audience first, yourself second

Look at this.

<div class="album">
<img src="/images/blogposts/old_header.PNG">
</div>

I designed for me, for what I thought looked cool. But the result was atrocious:
300px separates each menu item, so your eyes have to move far from one to the
next. The header is 400px tall and the only reason for that is to fit in more 
blue. It's a waste of space.

Think about who you are trying to reach out to. If we're being honest, for me that primarily
would be recruiters right now. Or, if not recruiters per se, those otherwise trained in the job offerin' arts before straitened circumstances forced them into a life of aimless wanderin'. Anywho, recruiters (like all people) are busy people. Get. To. The. Point.

<div class="album">
<img src="/images/blogposts/new_header.PNG">
</div>

No nonsense, have a typical navbar typically where one expects it. Also, make
the landing page for your blogs show a short list of excerpts instead of putting all
of them in one long page, so people can quickly see if the post interests them or not. 
The difference is staggering:

<div class="album">
<img src="/images/blogposts/length_compare.png">
</div>

It took so long to scroll the old site that I half expected the ladder song from MGS3
to start playing. I also broke up long-form text with a distinct colourful double-border
header to aid scanning. Finally, get to the point. If you know your audience is looking
for key details, don't hide them inside paragraphs. Pull them out and emphasize them.
Spend only 5 seconds reading and compare:

<div class="album">
<img src="/images/blogposts/spaceyyz_old.PNG">
</div>

With:

<div class="album">
<img src="/images/blogposts/spaceyyz_new.PNG">
</div>

In 5 seconds, could you pick up anything interesting in the old paragraph, when
it was among half a dozen others right above and below it? If you want someone to notice something, help them notice it. Make the name more prominent, and put the key information directly underneath on the left side of the screen where their eyes will naturally be. Separate the hot and cold content with colour.
Then if they're interested, they can read the bigger description.

# Lesson Four: Give space to breathe

Empty space is one of the most important parts of the overall appearance. Generally if you
have the space to spare, give yourself generous padding and margins. Keep content away
from its borders, make it comfortable: think subways at 2pm vs rush hour. This lets
the colours come out and do their thing. Pay attention to your line height so that
text is easier to read.

# Lesson Five: Guide the eye with shape and colour

I settled early on a simple geometric style with a two-tone colour contrast. This
allowed me to play around with shape-outside and clip-path (straying a bit from lesson three),
but it also influenced the user experience. Look at each section: sections with a paragraph
have a quadrilateral with a large bold header leading the eyes towards the text (see last 
screenshot above). Sections with lists (experience, projects) have a shape guiding the user downwards. 

<div class="album">
<img src="/images/blogposts/experience.PNG">
<img src="/images/blogposts/projects.PNG">
</div>

The most important part, the call to action, has a literal arrow directing the user to act.
It fits the overall angular design, but it has a function too.

<div class="album">
<img src="/images/blogposts/call_to_action.PNG">
</div>

# Fin

That's the end of the lessons. If you have any questions or comments, corrections or suggestions, criticisms et cetera, I'd love to get in touch by email. I'm always
open to learning new things and correcting bad things.