---
published: false
title: Why not CARLA?
---
> [CARLA](http://carla.org/) is an open-source simulator for autonomous driving research. In addition to open-source code and protocols, CARLA provides open digital assets (urban layouts, buildings, vehicles) that were created for this purpose and can be used freely. The simulation platform supports flexible specification of sensor suites and environmental conditions.

Seeing how close it is to our platform, one common question we're asked is why we didn't build it on top of CARLA, given that we even chose to same engine as them. That's what we'll explain today.

# Disclaimer

Given the subject, this blog entry will obviously not put CARLA in the best of light. But you should keep in mind that all of this is from the point of view of someone working on *driving simulation*, which is not CARLA's target audience. Over the years, the team behind CARLA managed to build an amazing open-source tool, and highlighting some shortcomings doesn't change any of that.

# Scripting

To work with CARLA, you need to write Python scripts to load, configure and run simulations. Python is a great language to work with, but that choice has many consequences, most of which are more trouble than they're worth.

## One language to rule them all

Unreal Engine comes with two programming languages: [C++](https://docs.unrealengine.com/en-US/ProgrammingAndScripting/ProgrammingWithCPP/index.html) and [Blueprints](https://docs.unrealengine.com/en-US/ProgrammingAndScripting/Blueprints/index.html). It also has [Python](https://docs.unrealengine.com/en-US/ProductionPipelines/ScriptingAndAutomation/Python/index.html) support, but for editor scripting only. And it will soon have a whole new language with *Verse*. Why add runtime Python to that already complex ecosystem?

I'm guessing they chose to do so since it's the go-to language for [Machine Learning](https://en.wikipedia.org/wiki/Machine_learning) (e.g. [TensorFlow](https://www.tensorflow.org/), [PyTorch](https://pytorch.org/). That way, from a single Python script, you can setup your simulation environment, connect it to your machine-learning and voila, you're all set. I get the appeal.

But if, like us, your workflow isn't exactly that, it suddenly becomes much more complex. CARLA's Python API encapsulates Unreal as a whole, so you can't easily switch between let's say Blueprint and Python.

Using the Python API, you lose something so important: the Unreal Editor. Everything you do is through scripting. Which could be ok, but in its implementation, is not. And we'll see that in the next sections

## Incomplete interface

The Python API obviously has its limits. It's focused on what CARLA does: car simulation for machine learning. Which means that most of Unreal API, that's available either through Blueprint of C++, isn't available in CARLA's Python API.

Using the available API subset won't get you much farther than what CARLA is designed to do. Don't expect to easily add a dog crossing a road, or generate random licence plates. That's not available in the API. To do that, you'll have to go back to Unreal, implement it, and most important: interface it to Python.

## Bridging the gap

Making things available in CARLA's Python API is obviously tedious, as is often the case when multiple programming languages interact.

You first have to design how to interface the feature you're interested in. For some, it's pretty easy. A licence plate generator could have a simple `generate_licence_plate()` method as interface, and that would be it. But anything manipulating objects or assets will be much more tricky.

Then you have to actually implement it on both side, or maybe even three as I think there's an intermediate layer between Unreal and Python. Which also means documenting this on all sides involved, adding tests and so on.

But even with all that, there are some things I just can't imagine working. Take [nDisplay](https://docs.unrealengine.com/en-US/WorkingWithMedia/nDisplay/index.html) for example, which is a must-have for us. How would you integrate that around CARLA's Python API?

## Why oh why

Given all that, I do not see a single reason, for us, to use this Python API. It's extra work, but for what?

I feel that this Python API is a substitute to Unreal's [Level Blueprint](https://docs.unrealengine.com/en-US/ProgrammingAndScripting/Blueprints/UserGuide/Types/LevelBlueprint/index.html), or even [Levels](https://docs.unrealengine.com/en-US/Basics/Levels/index.html). From my point of view, there is *nothing* I can do using CARLA's Python API that I can't do in a Level and Level Blueprint.

And I'd much rather using native, fully documented, supported and widely-adopted tools that Unreal provide. The Python API just adds layers and layers of complexity on top of Unreal, for no apparent benefit.

# Not a *driving simulator*

CARLA is not a driving simulation platform. It shares a lot of similarities and features with such tools, but that's not what it is.

[![Two-Set-Venn-Diagram.jpeg]({{site.baseurl}}/images/Two-Set-Venn-Diagram.jpeg)][0]

As in the Venn diagram above, there's overlap between the two. But there are missing features, and features we don't need.

As illustrated with the nDisplay case above, missing features are not just a case of "you'll have to add them anyway", whether in CARLA or Unreal. You'll also have to integrate them in *a platform that was designed to answer a problem that is not the problem you want to solve*. Driving simulation comes with its own set of challenges, and solving them might imply different design decisions than what CARLA did.

The same goes for CARLA-specific features that are of no use for driving simulation. The Python API shines as a striking example. CARLA probably added this API for machine-learning reasons. We don't do machine learning, but if we wanted to use CARLA, we'd have to leave with those consequences anyway.

The Venn diagram could be as high as 90% overlap between CARLA a driving simulation, but the missing 10% could still be enough to justify incompatible design decision on one side of the other. If you decide from the start that your platform will be used both for autonomous driving machine learning *and* driving simulation, you can design around all requirements and ensure and architecture that will satisfy everybody.

CARLA is a project focused on autonomous driving, and is designed as so.

# Re-implementing the wheel

CARLA comes ready-to-use, with existing cars, buildings, scenes and much more. All of that free! That is absolutely amazing, especially since those assets are licenced under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), meaning you can use those even without CARLA.

However, when you look at those assets, you'll realize that they spent a large amount of work on content you can buy for not much in the Marketplace. CARLA has free equivalent of [Procedural Buildings](https://www.unrealengine.com/marketplace/en-US/product/city-downtown-pack), [Spline Meshes](https://www.unrealengine.com/marketplace/en-US/product/nv-spline-tools), [Sky and Weather](https://www.unrealengine.com/marketplace/en-US/product/ultra-dynamic-sky) and much more. But in all cases, the Marketplace products are of higher quality, with better documentation, etc.

Before we bought any Marketplace item, we relied a lot on CARLA's assets. But slowly, we started phasing out those and replacing them with Marketplace products.

CARLA being open-source, it obviously can't rely on Marketplace content. So they have no other choice than to re-implement existing products. But from our perspective, we much prefer using Marketplace products.

# Too early

not 1.0, quality not perfect (e.g. opendrive)

# What could have been?

re: simulation email
py/ml integration, everything else in-editor (euw/eubp)

[0]: https://www.lucidchart.com/pages/fr/exemple/diagramme-de-venn-en-ligne