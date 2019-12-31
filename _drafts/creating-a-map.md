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
large brush "strokes". Virtually speaking of course. Something like in Gimp when
you make a brush stroke, it looks like a gaussian distribution around where you
clicked.

So my task will be to write some code to simulate these brush strokes.  Then
I'll generate some random paths with random strokes sharpness along the way...
We'll see how it goes.

But I think I'll have to play around with manually created heightmaps first, so
I can get a feel of what feels natural and what does not. So I can randomize it
the "right" way...

Here are some manually drawn heightmap (with GIMP), used as a base of my terrain
mesh.

![first_try](/assets/2019-12-31_island_1.gif)

![second_try](/assets/2019-12-31_island_2.gif)

This looks quite tricky to randomize, I had to draw several of them to get a
convincing island. But anyway that technique is simple enough to be worth
developping.

Meanwhile, a 3d model I'v drawn a few days ago to explore a bit about the universe
and the theme.

![zeppelin](/assets/2019-12-31_zeppelin.png)

And HAPPY NEW YEAR !
