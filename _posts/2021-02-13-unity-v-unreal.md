---
published: false
title: 'Driving simulation, Unity or Unreal?'
---
When it comes to game engines, the market is dominated by two giants: [Unity][unity] and [Unreal][unreal]. It's worth mentioning that they're not the only actors in town, you can also find [Godot][godot], [CRYENGINE][cyengine], [Amazon Lumberyard][lumberyard], [UNIGINE][unigine], etc. But when it boils down to it, the question is: Unity or Unreal?

|                                             |                                                           |
|:-------------------------------------------:|:---------------------------------------------------------:|
| ![]({{site.baseurl}}/images/unity-logo.jpg) | ![]({{site.baseurl}}/images/UE_Logo_Horizontal_Black.png) |

First of all, and to state the obvious, there's no single answer to that question. Both are amazing tools with a wide variety of features. So we'll focus on highlighting there differences, especially those most important when building a driving simulator.

# Who's using what?

One interesting thing to study is who's using what in the existing driving simulation world. Historically, everyone was either running their own engine or [OpenSceneGraph][osg]. However, things are changing in the past few years, and even though historic actors aren't making a full move to new engines, they're at least integrating them in their solutions.

* [AVSimulation][avs], [PreScan][prescan], [VI-grade][vigrade], [CARLA] are using Unreal
* [Metamoto][metamoto], [Cognata][cognata], [LGSVL][lgsvl] are using Unity
* [IPG][ipg] is using Unigine

If we look at the academic side, both [NADS][nads] and [ITS Leeds][its] are using Unity. If we try to look for organisations that develop their in-house driving simulation platform (as we are), both [Scania][scania] and [WMG at the University of Warwick][wmg] are using Unreal.

# Target markets

This section might be more subjective, as it's difficult to find objective data on this subject. It's basically what I perceive from reading both engine's blogs and Twitter feeds.

Unity is undoubtedly the mobile market king, and seems to invest heavily in that. It's a bit more targeted towards small teams looking to make small to mid-range games. Even though it's widening its target market beyond games, this move somehow feels a bit late.

[![ue-build.jpg]({{site.baseurl}}/images/ue-build.jpg)][0]


Unreal has made a strong move to various industry sectors in the past years: automotive, arch-viz, military and lately virtual production. Even though games still are at their core, they're serious about every market expension they make. Their [Build][build] event, [Automotive Field Guide][afg], or just the fact that their [Industry Manager][sloze] is active on Twitter are great signs of their commitment.

[0]: https://www.unrealengine.com/en-US/events/build-detroit-19-showcases-real-time-automotive-design-and-visualization

[lgsvl]: https://www.lgsvlsimulator.com/
[scania]: https://www.unrealengine.com/en-US/spotlights/real-time-simulation-of-new-hmi-concepts-at-scania
[wmg]: https://www.unrealengine.com/en-US/spotlights/meet-the-hybrid-real-time-simulator-for-testing-autonomous-vehicles
[build]: https://www.unrealengine.com/en-US/events/build-for-automotive-2020
[afg]: https://www.unrealengine.com/en-US/spotlights/the-automotive-field-guide-building-an-automotive-platform-with-unreal-engine
[sloze]: https://twitter.com/slfeeding