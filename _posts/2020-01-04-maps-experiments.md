---
layout: post
title:  "A few non-sastisfying maps"
date:   2020-01-04 13:25:00 +0000
categories: [blog, dev]
comments: true
---

I spent a bit of time playing with Voronoi diagrams to try and generate some
good looking meshes and cells for my maps. It turns out the method works well
but is very complex for a not particularly big difference with more standard
grids.

Either the different points in my mesh are sparse or completely random, in which
case it's really hard to deform (height-wise) or they follow some kind of
structure to make them easier to deform. So I've tried placing them not too
randomly.

![voronoi cells](/assets/maps/voronoi.png)

I'm not satisfied at all with the result. The benefits of my approach are almost
inexistent compared to a grid. One way to solve this is to find a better
distribution for my vertices, which looks less "gridy", and then derive
altitudes from the actual Voronoi graph, base on the feature of the generated
cells, rather than just noise that's just completely independent of the grid
features.

But meanwhile I decided to explore generating small islands from Perlin noise.
Same here, I'm really not happy with the result. But at least I got to
experiment a bit.

![voronoi cells](/assets/maps/perlin.png)

I don't know what I'm going to do. All these experiments are a lot of work and
I'm not sure it's worth spending so much time on this. For now at least.

I think I'll go from something simple, and implement basic features for the
game, regardless of the map generation. Basic maps, for now, will do the job.
