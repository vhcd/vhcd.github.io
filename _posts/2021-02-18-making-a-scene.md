---
published: false
title: Making a scene
---
When you start implementing your experiment, the first thing you'll work on is the scene. You may want to reuse an existing one from a previous project, maybe modify it, or start from scratch to build something tailored to your needs.

![scene.png]({{site.baseurl}}/images/scene.png)

In this article, we'll explain how we built the scene from the above screenshot, starting from nothing.

# Road network

As we [previously mentionned](/opendrive), the road network is a very important piece of our simulation, for which we need both the 3D representation and its logicial description (i.e. OpenDRIVE). As such, we've mentioned that we use [RoadRunner](https://www.mathworks.com/products/roadrunner.html), which is our favorite tool for that specific work. If you've played any [city-building game](https://en.wikipedia.org/wiki/City-building_game) (e.g. [SimCity](https://en.wikipedia.org/wiki/SimCity), [Cities: Skyline](https://en.wikipedia.org/wiki/Cities:_Skylines), you know that creating roads and city can be quite a fun activity if you're given the right tools. It really feels like RoadRunner understands that, and drawing any road network with this software is effortless and can even feel rewarding once you've accomplished your goal in such a short amount of time.

RoadRunner also offers tools to create the scene beyond the road network. This includes vegetation, buildings, etc. However, we're not big fan of those features, as Unreal Engine has both more assets and tools to build your world from the ground up.

![scene_rr.png]({{site.baseurl}}/images/scene_rr.png)

So what you see in the image above is the end product as far as RoadRunner is concerned. Roads, surfaces between them, and that's pretty much it. From that, the import/export to Unreal Engine is also very simple, as RoadRunner offers native support to this engine (as well as Unity, and a wide range of other tools).

# Above the ground

As mentioned, all our "above ground" scene creation is done within Unreal Engine, as it offers tools that are easy to use and ensure optimal performance, since we want our scenes to render at (at least) 60Hz.

Beyond tooling, Unreal Engine's [Marketplace] offers a huge collection of ready-to-use assets, at very attractive prices. Epic Games is even giving away free assets as part of their [Free For The Month] and [Permanently Free Collection](https://www.unrealengine.com/marketplace/en-US/assets?tag=4906). All that allowed us to grow our asset library at very low cost.

It's also worth mentioning that all of [CARLA](http://carla.org/)'s assets are free to use under a permissive licence (CC BY-SA, IIRC), you can find them [here](https://github.com/carla-simulator/carla/blob/master/Util/ContentVersions.txt). They include everything you need to build driving simulation environments, so that's a great way to get started.

## Vegetation

Whatever type of scene you're doing, you'll probably want some sort of vegetation within it. From forests and fields along highway, to plants and trees along sidewalks, vegetation is something that makes your scene feel more realistic. 

![foliage.png]({{site.baseurl}}/images/foliage.png)

Unreal Engine has a great [Foliage Tool](https://docs.unrealengine.com/en-US/BuildingWorlds/Foliage/index.html), which makes it very easy to place large amounts of various kind of vegetation, all the while ensuring optimal performance, so that your framerate won't drop even if you have a wheat field with 3 million blades.

The [Marketplace] has tons of foliage assets, some of them free! The [Project Nature](https://www.unrealengine.com/marketplace/en-US/profile/Project+Nature) is one example of a mutitude of high-quality and free vegetation. Between those and all the assets that went [Free For The Month] and containted vegetation, I don't think we'll ever need to buy a vegetation asset pack.

## Buildings

## Props

[NVSplineMesh]

## Landscapes?

[Marketplace]: https://www.unrealengine.com/marketplace/en-US/store
[Free For The Month]: https://www.unrealengine.com/marketplace/en-US/assets?count=20&sortBy=effectiveDate&sortDir=DESC&start=0&tag=4910
