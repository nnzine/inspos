---
title: Granular
author: Brock Wilcox <awwaiid@thelackthereof.org>
---

***META: I am writing some thoughts here but am not sure how this will take shape. I'm thinking it'll end up being either an overview of Granular stuff or, more likely, a deep dive into `glut` or `granchild`. We will see! Since the overall zine project isn't defined, the structure of this isn't defined either. But I've only barely begun and I'll tell you one thing for sure: I'm going to delete most of this!***

I don't know where I first saw the word "granular", maybe when reading through the list of every single script published for norns. But it sent me on a side mission from whatever it was I was going to do. Let's do this.

Maybe it was from when I read the description of `granchild`

> granchild is a granular grandchild. this script was born out of and inspired by @cfd90's clever twine and @justmat's inspiring mangl, both themselves based on @artfwo's amazing glut script, which itself was inspired by @kasperskov's grainfields. i consider this script to be a grandchild of glut, and child of the other two - using some ideas from twine (randomization in parameters) and some code from mangl (lfos, greyhole integration) and the engine of glut, to make a granular sequencer to fit my personal musical journey. ([link](https://norns.community/en/authors/infinitedigits/granchild))

So ... that's a lot of words. And a lot of them that I never heard of. I went on a journey. I always start with wikipedia.

> It is based on the same principle as sampling. However, the samples are split into small pieces of around 1 to 100 ms in duration. These small pieces are called grains. Multiple grains may be layered on top of each other, and may play at different speeds, phases, volume, and frequency, among other parameters. ([link](https://en.wikipedia.org/wiki/Granular_synthesis))

I looked at the list of engines on norns.community and see that several reference `glut`, so I decided to start there.

Glut. There is a youtube video demo that is super cool and I'm hooked. Glut is both a lua script that works with the grid AND a SuperCollider engine. I open up the code to both, scrolling through to get a sense of the length and overall structure. I'm interested in pulling the engine into a super-engine that I play with, so I'm particularly looking at what sort of global variables it is using.

Sidetrack: I see the SC declare `var <buffers;` which has a funny-looking `<` that I'm not familiar with. Google "less than supercollider variable declaration" and learn from the SC docs that "A getter message for an instance variable is created by typing a less than sign < before the variable name." OK so this auto-makes the variable readable externally with a method. Don't think I care right now but great!

I have no idea how this thing works yet, but I see (a) that it is quite a bit shorter than I thought and (b) that it uses SC `GrainBuf` which I think I saw once... so maybe that is doing some heavy lifting. I suddenly realize that scripts should also be experienced, so I fire up my actual norns and play around a bit.

Now I'm reading through the lua, which is quite a bit longer than the SC. A quick scan but then I start from the bottom and go upward, starting in `redraw` but then noticing that the real work is in `init`. Note that `enc` and `key` are both empty, so this script really doesn't take any norns input.

Here in `init` I see things I expect. First a setup of grid button callbacks, then some stuff about polls and and recorders that I don't understand, a grid refresh loop, and finally a whole bunch of `params` setup, which is the main "UI" for this script.

... [TO BE CONTINUED] ...
