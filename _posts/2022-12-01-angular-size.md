---
published: false
title: Angular size of partially occluded actors
---
A researcher colleague recently asked me: "Could we get the angular size of an actor in realtime, even if they're partially occluded?". As with many things in Unreal Engine, the answer is "yes we can!", so here's a breakdown of our answer to this question.

# The problem

## Angular size...

Angular size, visual angle, angular aperture, etc. are, in our case, all synonms of what we want to measure. To quote from [Wikipedia](https://en.wikipedia.org/wiki/Angular_diameter):

> The angular diameter [...] is an angular distance describing how large a sphere or circle appears from a given point of view

In our field of study, it can be a useful indicator of the visibility of actors in the scene from the driver's point of view. I won't go too deep in the actual scientific use of this measure, as this blog discusses more the *how* and not the *why* (researchers cover that part in their publications).

## ... of actors...

As the definition above points out, angular size works if the target is a sphere (or projects as circle). But in our case, we're not really interested in the angular size of a distant planet, but actually on scene objets.

And those can be of any shape: pedestrians, street signs, roadside props, bikes, and so on. So how does the definition of angular size apply to a not-so-spherical pedestrian? Should we take the height? The width? The average? And pedestrians can move their arms or legs, carry objects, which can all impact the angular size.

## ... partially occluded

Not only do actors have widely varying shapes, but they can quite often be partially occluded! A pedestrian can be partly behind a trash can, or a street sign can be somewhat masked by a tree.

This "occluded" requirement actually makes any solution much more complex to design: the size of an actor could be approximated statically, but the occlusion is inherently dynamic, and therefore needs to be solved at runtime.

# Our solution

When I sat down to design a solution, two different approaches came to mind. I'll go over the one we actually implemented, which is CPU-driven. And I'll mention at the end the one we didn't, which is GPU-driven.

