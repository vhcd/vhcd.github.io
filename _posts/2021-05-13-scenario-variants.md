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

