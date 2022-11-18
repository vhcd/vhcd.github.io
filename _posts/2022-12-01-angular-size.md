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

The overall idea is to first, figure out what parts of the actor are currently visible. Then, we need to approximate those parts as a shape on which we can compute an angular size.

## Visibility check

In game engines, if you want to check if something is visible, you "[trace](https://docs.unrealengine.com/4.27/en-US/InteractiveExperiences/Tracing/Overview/)" it. However, traces work as a single line, or as a fixed-size shape. So if we were to trace a pedestrian, it would only check its root position and returns whether that was visible or not.

However, most pedestrians have a [skeleton](https://docs.unrealengine.com/5.0/en-US/skeletons-in-unreal-engine/), which is used to animate them. And as with real-life skeleton, virtual skeletons also have bones. What if we traced bones?

That's the solution we chose: for each type of actor we wish to get data from, we define the set of bones (or [sockets](https://docs.unrealengine.com/4.27/en-US/WorkingWithContent/Types/StaticMeshes/HowTo/Sockets/)) that we'll trace.

This has a lot of benefits
* Bones carry semantic information (e.g. head, foot), so we know which parts were and weren't visible.
* For a single actor, we can trace multiple meshes. For instance, for a biker, we can trace the bike and biker bones.
* We can select the level of accuracy we want by selecting or excluding bones from the traces. For example, for most pedestrians we don't really care about each finger visibility. But in some specific use cases, we could...

The one major downside, however, is performance cost. Even though tracing is highly optimized in Unreal Engine, we can't just trace 20 bones on 40 actors at each frame. Our main solution for that is to only trigger this process when we need it: from our [scenario](/scenarios/), we roughly know when an actor will be relevant to ego. Another unexplored solution could be to distribute computation over our [nDisplay](/ndisplay/) cluster. Instead of having a single computer tracing 5 actors, why not have each computer of the cluster trace one? We're far from being CPU-bound on our cluster, so that actually could work.

Another downside is inaccuracy: traces use the "physics" version of the world, which can differ from the "visible" version. Collisions (which traces are a part of) use a simpler world, with geometry that's much more efficient to work with than rendered objects. On top of that, collision geometry is unaware of rendering tricks that can alter the object shape, such as [World Position Offset](https://docs.unrealengine.com/4.27/en-US/Resources/ContentExamples/MaterialNodes/1_10/). Those tricks are commonly used for moving foliage (e.g. simulating wind on tree branches), so we have to be very aware of this limitation.

## From bones... to angular size?

Ok, so line traces can tell us which bones are and aren't visible. But how do we get an angular size from that information?

To answer that, we first have to go back to a question mentioned above: what's the angular size of a non-spherical object?

















