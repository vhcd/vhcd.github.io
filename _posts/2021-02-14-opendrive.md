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


One answer lies in the reason why we use driving simulators: to run experiments. As such, we're interested in some data that will be generated when subjects are driving.

# How do we create it?

# How do we read it?

[0]: https://fr.mathworks.com/help/roadrunner/ug/Exporting-to-OpenDRIVE.html
[1]: https://www.highwaycodeuk.co.uk/control-of-the-vehicle.html

[OpenDRIVE]: https://www.asam.net/standards/detail/opendrive/
[Google Maps]: https://maps.google.com/
[OpenStreetMap]: https://www.openstreetmap.org/
[PEGASUS]: https://www.pegasusprojekt.de/en/