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

# Programming languages

One of the main difference between Unity and Unreal is their programming language. Unity uses C# (and now [Bolt][bolt]), whereas Unreal uses C++ and [Blueprints][bp].

The C# vs. C++ debate is a whole other topic that I won't get into. Let's just say that C# certainly is an easier language to get into, C++ might have a very slight performance edge, though it's narrowing. For us, one important factor was that we had quite a few libraries with C++ classes, so working in a C++ environment makes our life easier by not having to interface our C++ classes to C#.

[![uebp.png]({{site.baseurl}}/images/uebp.png)][1]

On the visual programming subject, I was heavily conflicted. On the one side, it's easy to use, especially for some researchers that may not be to keen on writing code. On the other side, well, [there's a lot of other sides][vpbad]. Versioning can be difficult, [spaghetti code][spaghet] can take a very literal meaning [messy][hell], etc.

# CAVE rendering

Rendering the scene on a 360° screen comes with a specific set of challenges. Unless you're doing desktop or VR-headset experiments, you'll probably have to face those. And Unity and Unreal both have very different ways of handling it. 
 
[![CAVE](/images/360sim.jpg)][2]

Unity is a bit messy on this subject. Historically, it had a feature called [Cluster Rendering][cluster], which answered this problematic, but this features kind of disappeared, leaving many users wondering [what to use instead][unity-ndisp]. A replacement was announced for 2019, then 2020, and as I'm writting this in 2021, there's no official word from Unity.

So in the meantime, external solutions were introduced. [MiddleVR][middlevr] is the leading one, though the price might not be suitable for everyone. Then there's [UniCAVE][unicave], which is open-source, but from what I've head, requires some extra work to get it compatible with Unity's latest releases. Another one is [VR Distrib][vrdistrib], which I have no feedback on.
 

[0]: https://www.unrealengine.com/en-US/events/build-detroit-19-showcases-real-time-automotive-design-and-visualization
[1]: https://www.reddit.com/r/unrealengine/comments/ci9myr/enough_with_the_spaghetti/
[2]: https://www.cnet.com/roadshow/news/general-motors-gm-360-degree-simulator/

[lgsvl]: https://www.lgsvlsimulator.com/
[scania]: https://www.unrealengine.com/en-US/spotlights/real-time-simulation-of-new-hmi-concepts-at-scania
[wmg]: https://www.unrealengine.com/en-US/spotlights/meet-the-hybrid-real-time-simulator-for-testing-autonomous-vehicles
[build]: https://www.unrealengine.com/en-US/events/build-for-automotive-2020
[afg]: https://www.unrealengine.com/en-US/spotlights/the-automotive-field-guide-building-an-automotive-platform-with-unreal-engine
[sloze]: https://twitter.com/slfeeding
[bolt]: https://blogs.unity3d.com/2020/07/22/bolt-visual-scripting-is-now-included-in-all-unity-plans/
[bp]: https://docs.unrealengine.com/en-US/ProgrammingAndScripting/Blueprints/index.html
[hell]: https://blueprintsfromhell.tumblr.com/
[vpbad]: http://mikehadlow.blogspot.com/2018/10/visual-programming-why-its-bad-idea.html
[spaghet]: https://en.wikipedia.org/wiki/Spaghetti_code
[cluster]: https://docs.unity3d.com/560/Documentation/Manual/ClusterRendering.html
[unity-ndisp]: https://forum.unity.com/threads/cluster-rendering-or-ndisplay-unreal-engine-equivalent.642805/
[middlevr]: https://www.middlevr.com/
[unicave]: https://widve.github.io/UniCAVE/
[vrdistrib]: http://www.vrdistrib.com/