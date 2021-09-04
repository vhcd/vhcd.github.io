---
published: true
title: Anatomy of a Scenario
---
Recently I shared a short [scenario](/scenarios) I made, involving an e-scooter, a crowd, a bus and an unfortunate ending. It's definitely *not* a usual scenario from our "driving-oriented" perspective, so I thought it'd be interesting to explain how it came to life.

# Scenario

To put some context first, here's the scenario.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Something something e-scooter. <a href="https://twitter.com/hashtag/DrivingSimulation?src=hash&amp;ref_src=twsrc%5Etfw">#DrivingSimulation</a><br>I spent more time making the cinematic than the scenario, and it still looks bad. The scenario took me around an hour though, so that&#39;s nice. <a href="https://t.co/HZd1n0PxWM">pic.twitter.com/HZd1n0PxWM</a></p>&mdash; Bertrand Richard (@brifsttar) <a href="https://twitter.com/brifsttar/status/1421849840359809032?ref_src=twsrc%5Etfw">August 1, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Why did I make it? I wanted to illustrate innovative use cases for our platform, in order for our researchers to get a better grasp at what they could potentially study with those new tools. Historically, most of our simulation work focused on the *driving*, hence the term *driving simulation*. But now, we can simulate pretty much any mean of transportation, and any interaction between those. I think researchers could get a lot of interesting ideas and projects if they properly understand the power of their tools, so I hope this small scenario helps them toward that goal.

# Scene

The scene is re-used from another project we're currently working on ([SUaaVE](https://www.suaave.eu/)), and we've already talked about it in a [previous post](/making-a-scene), some I'm not going into too much details here. Basically, we use [RoadRunner](https://www.mathworks.com/products/roadrunner.html) to create the road network, and then everything above-ground is built in Unreal Engine, using a very wide range of [Marketplace](https://www.unrealengine.com/marketplace/en-US/store) products.

![scene_rr.png]({{site.baseurl}}/images/scene.png)

# E-Scooter

The E-Scooter was bought from [a pack](https://www.unrealengine.com/marketplace/en-US/product/vehicle-pack-gest) of static vehicles, which I then [rigged](https://en.wikipedia.org/wiki/Skeletal_animation) to create a ridable version of it. I also made a small [animation blueprint](https://docs.unrealengine.com/4.27/en-US/AnimatingObjects/SkeletalMeshAnimation/AnimBlueprints/) for the rider, which places hands and feet at the right position using [two-bone IK](https://docs.unrealengine.com/4.26/en-US/AnimatingObjects/SkeletalMeshAnimation/NodeReference/SkeletalControls/TwoBoneIK/).

The e-scooter is moved by its [Virtual Driver](virtual-driver/), though in this instance it's definitely not  an advanced use case, as it's a simple spline following.

![scooter-spline.jpg]({{site.baseurl}}/images/scooter-spline.jpg)

# Crowd

The crowd is made out of four different types of pedestrian we currently have available in our platform. Each have their benefits, and mixed together they can give great results. We'll go over each of them, starting from the most basic to the most advanced.

First, we have some static posed humans, coming from the free [Twinmotion Posed Humans 1 pack](https://www.unrealengine.com/marketplace/en-US/product/twinmotion-posed-humans) on the Marketplace. They're very detailed and have a wide variety of poses.

![tm_posed.jpg]({{site.baseurl}}/images/tm_posed.jpg)

Then, we have [Skeletal Mesh Actors](https://docs.unrealengine.com/4.26/en-US/Basics/Actors/SkeletalMeshActors/), which come from [RenderPeople](https://renderpeople.com/), and on which we play idle animations. Those mostly come from [this pack](https://www.unrealengine.com/marketplace/en-US/product/generic-npc-anim-pack) which was free-for-the-month a while back. These pedestrians are not travelling in the world, but at least they have some basic movement.

The third kind of pedestrian we have is smart enough to move within the world. They're still using RenderPeople meshes for visuals, but the animation is driven by the [Advanced Locomotion System](https://www.unrealengine.com/marketplace/en-US/product/advanced-locomotion-system-v1) (ALS). To guide them within the crowd, target points are placed on the plaza, and the handy [Simple Move to Location](https://docs.unrealengine.com/4.26/en-US/BlueprintAPI/AI/Navigation/SimpleMovetoLocation/) takes care of the rest.

And last but not least, our smartest pedestrians can walk along sidewalk. For appearance, we use elements from the [Citizen NPC](https://www.unrealengine.com/marketplace/en-US/product/citizen-npc) pack, for which we made a small Blueprint that allows us to assemble them easily. For animation, we either use ALS or a simple walking animation. And to follow the sidewalk, we rely on the [OpenDRIVE](opendrive/) description of the road network, which includes information about sidewalks.

# Bus

The bus is an [Irisbus Citelis](https://en.wikipedia.org/wiki/Irisbus_Citelis), which was made by [Marsupi3D](http://www.marsupi3d.net/), who very kindly allowed us to use some of his work for our research. We're very pleased with this bus, as it is the same exact model that our local transportation agency uses! Meaning we get that "realism" bonus for our experiments' participants.

![citelis.jpg]({{site.baseurl}}/images/citelis.jpg)

I rigged the bus in [Blender](https://www.blender.org/), including the doors. In Unreal, I then created a matching animation blueprint and vehicle. I also had to create a custom [Physics Assets](https://docs.unrealengine.com/4.26/en-US/InteractiveExperiences/Physics/PhysicsAssetEditor/), to allow [NPC](https://en.wikipedia.org/wiki/Non-player_character) to get in and out of the bus. For that, I used [VMR](https://www.unrealengine.com/marketplace/en-US/profile/VMR)'s [Bus](https://www.unrealengine.com/marketplace/en-US/product/bus) as reference, which I highly recommend.

If you look at the picture above, you can catch a glimpse of the kid, which is inside the bus from the get go. Before making the scenario, I wondered how I would get the kid to move with the bus and then get out. It turns out it couldn't be simpler: just place the kid on the bus, and he will move with it. No trick required!

The bus is also driven by our Virtual Driver, with basic spline following for lateral control, and our [new stop goal](whats-new-2021-08/#virtual-driver), which allows us to accurately stop the bus at the... well, bus stop.

# Collision

If you follow my Twitter feed, you may remember that I played with collisions a bit. However, I mostly focused on *cars* hitting other things.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Still messing around with collision for <a href="https://twitter.com/hashtag/DrivingSimulation?src=hash&amp;ref_src=twsrc%5Etfw">#DrivingSimulation</a> in <a href="https://twitter.com/hashtag/UE4?src=hash&amp;ref_src=twsrc%5Etfw">#UE4</a>. <a href="https://t.co/sM6jyAx3wx">pic.twitter.com/sM6jyAx3wx</a></p>&mdash; Bertrand Richard (@brifsttar) <a href="https://twitter.com/brifsttar/status/1408770257087500293?ref_src=twsrc%5Etfw">June 26, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

As it turns out, pedestrian and e-scooter collision is a bit harder to both detect and then simulate. The physics bodies are (relatively) small, complex, and there's one more to deal with than with car collision: the car's driver isn't impacted by the collision, but the e-scooter rider obviously is.

So I cheated a bit, and used a [Trigger Box](https://docs.unrealengine.com/4.26/en-US/Basics/Actors/Triggers/) near the collision point, which activates very basic [ragdoll physics](https://en.wikipedia.org/wiki/Ragdoll_physics) on all involved actors. The code in itself is straight out of ALS, and it's more or less a single Blueprint node, so I didn't spend much brainpower on this.

# Putting all this together

This scenario took me less than an hour from start to finish. The majority of which was spent trying to figure out how to make the kid move with the bus, only to realize that the easiest solution possible was the right one. Other than that, it's just dropping lots of pedestrians in the world, drawing some splines and connecting nodes in the [Level Blueprint](https://docs.unrealengine.com/4.26/en-US/ProgrammingAndScripting/Blueprints/UserGuide/Types/LevelBlueprint/).

![escooter_bp.jpg]({{site.baseurl}}/images/escooter_bp.jpg)
