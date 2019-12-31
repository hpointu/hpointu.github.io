---
layout: post
title:  "Creating some maps"
date:   2019-12-31 15:25:00 +0000
categories: [blog, dev]
---

I'm still not sure how I'll generate my islands. I have a few completely
different ideas on how I could go about this.

I think I'll start as a simple 2d map. I'll cut off some pieces of a plain
square to make it look like an island, and I'll generate a random heightmap with
large brush "strokes". Virtually speaking of course. Something like in Gimp
when you make a brush stroke, it looks like a gaussian distribution around
where you clicked.

So my first task will be to write some code to simulate these brush strokes.
Then I'll generate some random paths with random strokes sharpness along the
way... We'll see how it goes.

