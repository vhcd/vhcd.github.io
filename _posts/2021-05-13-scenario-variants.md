---
published: false
title: Scenario Variants
---
The basis of a driving simulation experiment is the [scenarios](/scenarios). But quite often, an experiment requires not one but multiple scenarios. And most often, those multiple scenarios can be described as some kinds of *variants* from one another. How do we structure our experiment around this *variant* concept?

# What's a variant?

Before getting into, I'll first mention that this subject is closely related the [Design of experiments](https://en.wikipedia.org/wiki/Design_of_experiments), including all of its problematics (e.g. *learning effect*). As this is a tech blog, I won't cover any of those aspects.

To get better insights into driver's behavior, one way is to immerse them in not just one situation, but multiple *variants* of that kind of situation, and study how their behavior varied based on the situation differences.

The most obvious variants are the ones directly related to the study itself. I'll take as example a hypothetical study, in which we want to study driver's acceptance of various [AEB](https://en.wikipedia.org/wiki/Collision_avoidance_system) behaviors. Those different behaviors are our most obvious variant. And we could stop at that, having the exact same scenario and situation played for each AEB behavior and call it a day.

But for various experimental design reasons, we might want to add other varying parameters. For example, we might want to test AEBs in different situations, such as the type of pedestrian (e.g. adult, child) running across the road and triggering the AEB. We might also want the situation to not always occur at the same place in the scene, and at varying length of times after the beginning of the scenario.

The more you add varying parameters, the more complex your experiment gets. Let's say you have 5 AEB behaviors and 3 pedestrians types, that's already 15 *variants*. Some people would actually create 15 scenarios, involving a lot of copy/pasting and therefore being very error-prone in the long run (e.g. if you then want to slightly change one AEB behavior, you need to do it in all scenarios involving that AEB).

# Implementing Variants

So, how do we tackle that problem? The first step is to define what our variants will be. In Unreal Engine, this can be done in a few clicks with [Structures](https://docs.unrealengine.com/en-US/ProgrammingAndScripting/Blueprints/UserGuide/Variables/Structs/index.html#usingcustomstructs).

![ue4_variant.png]({{site.baseurl}}/images/ue4_variant.png)

Now that we have defined what "a variant" will be, we need to define the actual list of all the variants that we'll want to have. To do this, we use [Data Tables](https://docs.unrealengine.com/en-US/InteractiveExperiences/DataDriven/index.html#datatables), which are pretty much simple spreadsheets, with columns matching a... structure! So if we simply give the Data Table our newly created structure, we can then fill it with our variants.

![ue4_variants.png]({{site.baseurl}}/images/ue4_variants.png)



