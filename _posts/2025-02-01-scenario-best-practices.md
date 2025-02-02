---
published: true
title: 'Best Practices for Scenario development'
---

Over the past few years, we've shipped a few driving simulation experiments using Unreal, so I thought it would be interesting to sum up some of the best practices we're following to make this and future journeys easier.

Some of those will be trivial and common knowledge for anyone already in this business, some have already been talked about here, and others might be a bit specific to us. As with anything you read on the internet, this isn't intended to be the absolute right way of doing things, but it's just what works for us right now.

# Scripted Ego

This one [isn't new](/scenario-authoring/#automate-ego), but whether any scenario is with autonomous vehicule or not, we'll still script the Ego behavior just as we would any other car (using our [Virtual Driver](/virtual-driver)), so that we don't have to manually drive during development and testing. For us, switching between "autonomous" or manual driving for Ego is just a checkbox, so we can swap easily if need be.

# Spawn Points

In line with Scripted Ego, [Spawn Points](/whats-new-2024-11/#new-features) allow us to start the scenario at any predefined location within it. As driving simulation scenarios tend to be heavily scripted, with tight coupling between events, we can't just randomly select a location along the road and expect everything to run as expected. So we instead place Spawn Points at regular intervals, and for each one the desired initial world state is configured by the designer.

Benefits here are multiple. During development and testing, it allows the designer to focus on their desired situation, and not bother with previous ones. And even during the experiment itself, if the run had to be temporarily stopped for any reason (e.g., crash, participant break, external event), it can be restarted to a location close to where it had stopped, avoiding having the participant "relive" the whole scenario.

# One File Per Actor

[OFPA](https://dev.epicgames.com/documentation/en-us/unreal-engine/one-file-per-actor-in-unreal-engine) is great. Use it even outside of [World Partition](https://dev.epicgames.com/documentation/en-us/unreal-engine/world-partition-in-unreal-engine) (it's *Production Ready* for traditional levels since 5.2). Advantages:
* Multiple people can work concurrently on the same level
* Commits are easier (and lighter!) as you only submit edited actors
* Less risk of someone touching/breaking something they weren't supposed to

Honestly we haven't really used it yet (I missed the *Production Ready* status change), but we've had all the issues mentioned above by *not* using it. The only drawback is you need to commit in-engine to resolve the actor names, which we don't usually do with our [Git workflow](/pipeline/#superproject).

# (Most) Actors deserve their own Blueprint

Given our reliance on [Fab](/marketplace/) products, 99% of assets used in any given scenario are already available and ready to use in our wide collection of content. This means that when we want to add a car or a pedestrian to a scenario, we usually just look for one we like in the *Content Browser*, drag it into our scene and that's it.

But what we should really do is instead make a child Blueprint of that asset, give it a name related to the specific situation we plan on using it in, and drag this new one into our scene. This means that we can script things inside the Blueprint and not rely too much on the Level Blueprint (more on that later), or add and edit features (e.g., sound, animation) later on, without having to load/edit the whole level.

Of course, you don't want to do that for every actor in your scene: every tree or building doesn't need its own Blueprint. There's no clear line between which actor needs its own one and which one doesn't. So it's up to the scenario designer to decide, and obviously experience helps a lot. But we try to keep that in mind every time we're dropping an existing asset into our scene.

# Nightly builds

This one is fairly common, but [nightly builds](https://en.wikipedia.org/wiki/Daily_build) are great. Don't wait, and start packaging your project on day 1. Get the build pipeline working as soon as possible, and catch packaging issues as early as possible.

It's also useful for colleagues involved in the project, who want to check on the current state of things but don't have Unreal installed (nor should they). They can just grab the latest build and test it for themselves.

# Nightly renders

At first we did nightly renders for non-interactive scenarios only, because in those cases, nightly builds actually are nightly renders, as no build is generated. But then we realized that since we're scripting ego in all cases, we might as well do nightly renders.

We haven't actually done that yet, but the idea is that a render is much easier to work with as a build. It's lighter to "install", you can fast forward/backward through it, there's no user input required, you could even diff it with previous renders! And if you're discovering a sneaky bug late in development (e.g., small visual artifact), having renders can help quickly find when the issue was first introduced. And for researchers or partners involved in the project, it's much more user friendly to get a nice video than a complete build if you want to check on progress.

# The Level Blueprint can be your friend

I wrote a [whole post](/post-mortem-1/) on that subject, so I won't expand much on it here. But the Level Blueprint *can* be very useful in some cases. I had hoped Ari Arnbjörnsson's [*Myth-busting "Best Practices" in Unreal Engine*](https://www.youtube.com/watch?v=S2olUc9zcB8) would include a section on that subject, because it would fit well: the Level Blueprint gets a lot of hate, for mostly valid reasons, but it still can be a very powerful and useful tool. It obviously depends on the use case.

# *DRY* and the *Rule of three*

[*Don't Repeat Yourself*](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) (DRY) is a fairly common and very good principle in software development. In other words, don't copy/paste code, and instead structure your code to avoid the duplication.

But there's also the less known [*Rule of three*](https://en.wikipedia.org/wiki/Rule_of_three_(computer_programming)):

> It states that two instances of similar code do not require refactoring, but when similar code is used three times, it should be extracted into a new procedure

By blindily applying the DRY principle, you can end up spending most of your time refactoring your code each time you encounter any type of code duplication. My rule is therefore to allow *one* copy/paste of code. Because as the *Rule of three* explains, two occurences of the same code might not be enough to properly exctract the correct pattern behind it.

My more general approach to this is to always allow some dirtyness inside any of my codebase, but be very aware of it, and have some sort of idea of what the cleaner design would be. And after some undefined dirtyness threshold is crossed, refactor where it makes sense.

# More to come?

This article, as with most things in this blog, is as much knowledge for your as for us. The idea is to learn from every project we make, and capitalize that knowledge somewhere so that we can do better next time. Which means this article might evolve over time. So who knows what we'll learn next?
