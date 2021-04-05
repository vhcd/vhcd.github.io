---
published: false
title: Data Collection
---
The main point of a driving simulation experiment is to collect data relevant to our study. This includes both simulation related data (e.g. speed), but also physiological data, such as eye-tracking or EEG; all of which need to be synchronized. Today we'll discuss how we solve this in our V-HCD platform.

# A bit of (our) history

Up until a few years ago, the amount of data sources in our experiments was pretty limited; we'd rarely go beyond simulation + eye-tracking. Even just two data source raise the issue of synchronization between them, but this would be answered by one-to-one connection between the sources, or manual post-acquisition resynchronization.

But recently, we've been using more and more data sources for multiple reasons: broadening of our research areas, new emerging physiological data acquisition tools, or the rise of cheap [IoT](https://en.wikipedia.org/wiki/Internet_of_things) devices. We've done driving simulation connected with tablets, [Launchpads](https://novationmusic.com/en/launch/launchpad-x) eye-tracking, [EEG](https://en.wikipedia.org/wiki/Electroencephalography), [ECG](https://en.wikipedia.org/wiki/Electrocardiography), respiration measurement, [EDA](https://en.wikipedia.org/wiki/Electrodermal_activity), [fNIRS](https://en.wikipedia.org/wiki/Functional_near-infrared_spectroscopy) and even within an [MRI](https://en.wikipedia.org/wiki/Magnetic_resonance_imaging)!

Obviously, we didn't use all of them at once, but still, even just a few of those within an experiment requires a more robust way to synchronize them; our previous workflow doesn't scale up.

## RTMaps

Our first initial solution was to use [RTMaps](https://intempora.com/products/rtmaps/).

> RTMaps™ stands for Real-Time Multisensor Applications, it is a highly-optimized component-based development and execution software tool. Thanks to RTMaps™ you can design, develop, test, benchmark and validate multisensor applications for Advanced Driver Assistance Systems (ADAS) and Highly Automated Driving (HAD) software functions but also advanced features in other domains such as autonomous and mobile robotics, energy, system monitoring, complex instrumentation and human factors.

Mosty developed for [ADAS](https://en.wikipedia.org/wiki/Advanced_driver-assistance_systems) development, this tool can read and synchronize many data-sources, which then can be manipulated to provide output, or simply recorded. It natively supports most car-related data sources, and over time gains additional support for other sources closely related to driving studies. And you can quite easily add new data sources, so no worries there.

[![rtmaps.png]({{site.baseurl}}/images/rtmaps.png)][0]

Even though the tool is great, it's not perfect for driving simulation experiment data collection. First, it's quite expensive; actually it's by far the most expensive tool in our whole experiment toolchain. Its recording format also isn't easy to work with, since it can't natively be opened by any [data wrangling](https://en.wikipedia.org/wiki/Data_wrangling) tool out there (e.g. [pandas](https://pandas.pydata.org/)). And since the tool is mostly for real-time data *processing* and we're mostly doing *recording*, there's a whole aspect of the tool that we don't use, making it uselessly complex for our use cases.

# LabStreamingLayer

> The [lab streaming layer](https://github.com/sccn/labstreaminglayer) (LSL) is a system for the unified collection of measurement time series in research experiments that handles both the networking, time-synchronization, (near-) real-time access as well as optionally the centralized collection, viewing and disk recording of the data.



[0]: https://intempora.com/products/rtmaps/