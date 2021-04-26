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

Given all that, I really wonder

# Not a *driving simulator*

Useless features for us

# Re-implementing the wheel

Many MP products redev

# Too early

not 1.0, quality not perfect (e.g. opendrive)

# What could have been?

re: simulation email
py/ml integration, everything else in-editor (euw/eubp)
