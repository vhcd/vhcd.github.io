---
published: false
title: Making a scene
---
When you start implementing your experiment, the first thing you'll work on is the scene. You may want to reuse an existing one from a previous project, maybe modify it, or start from scratch to build something tailored to your needs.

![scene.png]({{site.baseurl}}/images/scene.png)

In this article, we'll explain how we built the scene from the above screenshot, starting from nothing.

# Road network

As we [previously mentionned](/opendrive), the road network is a very important piece of our simulation, for which we need both the 3D representation and its logicial description (i.e. OpenDRIVE). As such, we've mentioned that we use [RoadRunner](https://www.mathworks.com/products/roadrunner.html), which is our favorite tool for that specific work. If you've played any [city-building game](https://en.wikipedia.org/wiki/City-building_game) (e.g. [SimCity](https://en.wikipedia.org/wiki/SimCity), [Cities: Skyline](https://en.wikipedia.org/wiki/Cities:_Skylines), you know that creating roads and city can be quite a fun activity if you're given the right tools. It really feels like RoadRunner understands that, and drawing any road network with this software is effortless and can even feel rewarding once you've accomplished your goal in such a short amount of time.

RoadRunner also offers tools to create the scene beyond the road network. This includes foliage, buildings, etc. However, we're not big fan of those features, as Unreal Engine has both more assets and tools to build your world from the ground up.

![scene_rr.png]({{site.baseurl}}/images/scene_rr.png)

So what you see in the image above is the end product as far as RoadRunner is concerned. Roads, surfaces between them, and that's pretty much it. From that, the import/export to Unreal Engine is also very simple, as RoadRunner offers native support to this engine (as well as Unity, and a wide range of other tools).

# Foliage

# Buildings

# Props

# Landscapes?
