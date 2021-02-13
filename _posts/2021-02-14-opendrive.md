---
published: false
title: OpenDRIVE
---
# What it is

> [OpenDRIVE] defines a file format for the precise description of road networks

If you're familiar with traditional mapping tools ([Google Maps], [OpenStreetMap]), you might notice that you don't get much description beyond "there's a road here". But in driving simulators, we need much, much more information than that. That's where OpenDRIVE helps us.

Initially developped through the [PEGASUS] project, OpenDRIVE was then transfered to [ASAM](http://asam.net/), where its development continues.

[![OpenDRIVE]({{site.baseurl}}/images/exporting_to_opendrive_02.jpg)][0]

An OpenDRIVE file follows the [XML](https://en.wikipedia.org/wiki/XML) standard and contains all relevant information describing a road network. This includes road geometries, marking, lanes, borders, etc. Any road network in the world can be described in the OpenDRIVE format.

# Why do we need it?

So an OpenDRIVE file is an "HD map". But why do we need that?

[![gap.jpg]({{site.baseurl}}/images/gap.jpg)][1]

One answer lies in the reason why we use driving simulators: to run experiments. As such, we're interested in data that will be generated when subjects are driving. Some of that data include

* Distance to another vehicle
* Time to collision
* Lane border distance

All of those values have one thing in common: they rely on the road geometry. If the road is perfectly uniform and straight, computing those is simple. However, roads are never uniform and straight, so we need to have access to the actual geometry of them to compute those values.

Another reason is scenario creation. When you're building your experiment, you create some situations which involve other actors, such as cars or pedestrians. If your scenario involves following a lead car, you want to be able to simply set the lead car to "keep its lane", without having to manually describe the curve it has to follow. Having access to the complete road description empowers the scenario designer to easily control any actor within the scene.

With such description at hand, we've been able to implement some major features of our platform, that we'll go into later in this blog. Here are a few:

* A *Virtual Driver*, which can drive any car on any road network, while being controlled with high level goals, such has "keep your lane" or "turn left at the next intersection"
* Pedestrians that automatically walk around their block to add some life to our scenes
* Automatic generation and positionning of traffic lights at intersections

# How do we create it?

Now that we have a clear view of what we need OpenDRIVE for, how do we get our hands on such files?

[![rr.jpg]({{site.baseurl}}/images/rr.jpg)][2]

Our favorite tool is [RoadRunner](https://www.mathworks.com/products/roadrunner.html)It's a scene creation tool, which exports both the 3D part of the scene and its OpenDRIVE description. In the tool, you can either create a custom road network that's tailored for your needs, or you can use the various [GIS](https://mathworks.com/help/roadrunner/ug/gis-data-resources-for-roadrunner.html) features within the tool to import real-world data. An extension is also available, called [RoadRunner Scene Builder](https://fr.mathworks.com/products/roadrunner-scene-builder.html), which can automatically create scenes from [HERE](https://www.here.com/platform/automotive-services/hd-maps) HD Maps. This certainly is the future for scene creation, but this service is quite expensive right now.

It's also worth mentionning [TrianGraphics](https://triangraphics.de/), which has more or less the same feature set as RoadRunner, but seems more difficult to work with.

[![atlatec.png]({{site.baseurl}}/images/atlatec.png)][3]

Another really different approach is to scan real road sections, and generate OpenDRIVE for it. At least two companies specializes in that ([Atlatec][3], [3D Mapping](https://www.3d-mapping.de/)), but it's not cheap, and you only get the OpenDRIVE, so no 3D scene to use in your simulator.

# How do we read it?

There's one thing left about OpenDRIVE: how do we actually read the file from our simulator? How do we [parse](https://en.wikipedia.org/wiki/Parsing) it and extract data from it?

There are some libraries available online, that can manipulate OpenDRIVE files to some degrees. But in my experience, [esmini](https://github.com/esmini/esmini) as by far and large the best library in its [RoadManager](https://github.com/esmini/esmini/tree/master/EnvironmentSimulator/Modules/RoadManager). It's an active open-source project lead by Volvo, and the OpenDRIVE library has all the features you want, and I know firsthand that implementing some of those is definitely not an easy feat to accomplish. The maintainer is very receptive and supportive, and it's always a pleasure contributing to this project.

[0]: https://fr.mathworks.com/help/roadrunner/ug/Exporting-to-OpenDRIVE.html
[1]: https://www.highwaycodeuk.co.uk/control-of-the-vehicle.html
[2]: https://www.mathworks.com/videos/getting-started-with-roadrunner-junction-creation-in-roadrunner-1586438843851.html
[3]: https://www.atlatec.de/

[OpenDRIVE]: https://www.asam.net/standards/detail/opendrive/
[Google Maps]: https://maps.google.com/
[OpenStreetMap]: https://www.openstreetmap.org/
[PEGASUS]: https://www.pegasusprojekt.de/en/