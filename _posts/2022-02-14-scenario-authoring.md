---
published: false
title: Scenario Authoring
---
We've already talked about [scenarios](/scenarios), which are at the very core of driving simulation. But this was a rather high level overview, and today I want to go a bit deeper to discuss how we're actually implementing our scenarios, and how we try to make the process as easy and efficient as possible

# Requirements

At [LESCOT](https://lescot.univ-gustave-eiffel.fr/), we've been authoring a very wide range of scenarios for decades, and we've gathered a lot of feedback from it over the years. When we started working on our Unreal Engine platform, we use that to create a small set of requirements regarding scenario authoring, ensuring that what we create fits our needs as best as possible.

## Scripting design

In a previous article, I quickly went over "[What are scenarios anyway?](/scenarios#what-are-scenarios-anyway)", illustrating some of the complex use-cases we previously encountered. From experience, scenario requirements complexity only grows over time, and future experiments will need more attention to various details.

However, not all scenarios need to be complex. Or at least, not all *parts* of a scenario. We might want to create one or more abstraction levels, each with its own complexity. This is, for example, what we do with our [scenario variants](/scenario-variants).

The reason behind all this is *who* designs and authors the scenario. The most common and natural way to describe a scenario is "When X happens, do Y", and chain all that together, in a similar fashion to [state diagrams](https://en.wikipedia.org/wiki/State_diagram). Researchers often think of scenarios that way, but depending on length and complexity, designing a whole scenario with that paradigm can become difficult to maintain. Engineers, on the other hand, might be more familiar with [OOP](https://en.wikipedia.org/wiki/Object-oriented_programming) and other methodologies, which might be easier to work with for experts, but too complex to grasp for non-programmers.

So all in all, we wanted our authoring process to allow a wide range of paradigms, from natural "when... do..." to complete programming language interface.

## Testing

When authoring scenarios, the most time-consuming task often is testing. Once again, it depends on complexity, but if you have a 30+ minutes scenario, with lots of intertwined events, testing the part you're working on can be frustrating if it occurs at the 20th minute. Usually, authors try to subdivide scenarios in individual parts that can be tested independently; but that's not always possible, and you'll always have to test the connections eventually.

We really wanted to try to improve that. Not only is this time-consuming, it's especially frustrating to whoever's doing it. And not only can frustration lead to errors, but we also want to make this authoring process as pleasant as possible for everyone involved.

# Solutions

For each of our requirement category, we'll go over the solutions we implemented or envision. For each case (and for our overall project), our philosophy is to avoid complex solutions, and to try to reuse existing features.

## Scripting design

As discussed in a previous article, we mostly rely on [Blueprint Visual Scripting](https://docs.unrealengine.com/en-US/Engine/Blueprints/index.html) to implement scenarios, which is the native Unreal Engine solution to scripting. It solves a lot of "scenario" issues by itself, but it's not really enough for our requirements.

### Stages

Most importantly, Blueprint aren't state machines. They're mostly designed for event-based scripting. Which, in theory, is what we want; "when... do..." is just that: react to events. However, videogame approch to events is mostly *cartesian-distance* based, whereas driving simulation is more *road-time* based. In other words, videogames rely on physical [trigger volumes](https://docs.unrealengine.com/4.27/en-US/Basics/Actors/Triggers/) that can be placed in the world, and that will execute stuff when the player gets in it. Driving simulation is usually more interested in *time* between actors on a *road*, e.g., "When ego is less than 3s from the traffic light, make it yellow".

[![trigger_place.jpg]({{site.baseurl}}/images/trigger_place.jpg)][0]

Not only do we want *road-time* events, but we also want them to happen in a very specific sequence, that we can have very strict control upon. Something like a state diagram, where we can ensure that everything will be played out the way we want to.

My solution to that is **Stages**. It's a single new Blueprint node that allows implementing the "when... do..." paradigm in traditional Blueprint scripting.

![nextstage.jpg]({{site.baseurl}}/images/nextstage.jpg)

This node, along with [Events](https://docs.unrealengine.com/4.26/en-US/ProgrammingAndScripting/Blueprints/UserGuide/Events/), allows the scenario author to clearly define the sequence in which events will occur, and for each one, what will trigger it and what action will result from it. Since it's only a single node, we can still use all the other Blueprint features and nodes; and users can rely on all associated resources when authoring scenarios.

This paradigm is also compatible with more traditional *cartesian-distance* event system, or with any other that can be done in Blueprint. For example, we also try to split scenario per-actor (when applicable), and stages can easily be used inside each actor's Blueprint. No limitation there!

<video width="720" height="480" controls>
  <source type="video/mp4" src="{{site.baseurl}}/images/stages.mp4.mp4">
</video>

And as you can see above, the [Blueprint debugger](https://docs.unrealengine.com/4.27/en-US/ProgrammingAndScripting/Blueprints/UserGuide/Debugging/) works particularly well with this "stages" paradigm, as we can clearly see going through the stages. If you end up in the wrong stage, the debugger will easily tell you where you are and why you're there.

### Flow

[Flow][flow] is an open-source community-lead Unreal Engine plugin.

> Flow plug-in for Unreal Engine provides a graph editor tailored for scripting flow of events in virtual worlds.

As soon as I read that, I was *very* interested in the plugin. It took me quite a while to fully grasp what the plugin offers, but I'm quite confident that this is the ultimate solution to scenario design and implementation in Unreal Engine.

[![flow.png]({{site.baseurl}}/images/flow.png)][flow]

But I haven't even tried to implement a scenario with *Flow*, and I don't plan on using it in the near future. Why, you may ask?

The plugin is still quite new and not officially supported by Epic Games, which means much less resources to help users getting started. And even though the idea behind *Flow* is fairly easy to understand, implementing an actual flow seems much more technical than a standard Blueprint. It feels like a lot of groundwork is required to create *flow nodes* (mostly in C++), tailored to our driving simulation use case, that can then be used in higher level *flow graphs*.

*Flow* really is something I want to explore as a solution to scenario design, but I think I'll wait for both the plugin to mature, and for us to have a stronger experience in Unreal Engine. And with UE5's *Verse* language around the corner, I think it's safer to see what Epic's new scripting language has to offer before jumping to an unofficial one.

But I really encourage you to check the [Flow][flow] project out. It immediatly garned traction upon initial release, has lots of people interested and contributing, an active Discord server and a really experienced maintainer.

## Testing

To make testing easier, we 

### Automate ego

### Time dilation

[0]: https://docs.unrealengine.com/4.27/en-US/Basics/Actors/Triggers/
[flow]: https://github.com/MothCocoon/FlowGraph
