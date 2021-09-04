---
published: false
title: Anatomy of a Scenario
---
Recently I shared a short [scenario](/scenarios) I made, involving an e-scooter, lots of pedestrians, a bus and an unfortunate ending. It's definitely *not* an usual scenario from our "driving-oriented" perspective, so I thought it'd be interesting to explain how it came to life in less than an hour of work.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Something something e-scooter. <a href="https://twitter.com/hashtag/DrivingSimulation?src=hash&amp;ref_src=twsrc%5Etfw">#DrivingSimulation</a><br>I spent more time making the cinematic than the scenario, and it still looks bad. The scenario took me around an hour though, so that&#39;s nice. <a href="https://t.co/HZd1n0PxWM">pic.twitter.com/HZd1n0PxWM</a></p>&mdash; Bertrand Richard (@brifsttar) <a href="https://twitter.com/brifsttar/status/1421849840359809032?ref_src=twsrc%5Etfw">August 1, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

# Scene

The scene is re-used from another project we're currently working on ([SUaaVE](https://www.suaave.eu/)), and we've already talked about it in a [previous post](/making-a-scene), some I'm not going into too much details here. Basically, we use [RoadRunner](https://www.mathworks.com/products/roadrunner.html) to create the road network, and then everything above-ground is built in Unreal Engine, using a very wide range of [Marketplace](https://www.unrealengine.com/marketplace/en-US/store) products.

![scene_rr.png]({{site.baseurl}}/images/scene_rr.png)

# E-Scooter

The E-Scooter was bought from [a pack](https://www.unrealengine.com/marketplace/en-US/product/vehicle-pack-gest) of static vehicles, which I then rigged to create a ridable version of it. I also made an small [animation blueprint](https://docs.unrealengine.com/4.27/en-US/AnimatingObjects/SkeletalMeshAnimation/AnimBlueprints/)