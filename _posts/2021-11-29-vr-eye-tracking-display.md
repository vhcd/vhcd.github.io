---
published: false
title: Live VR eye-tracking data visualization
---
Eye-tracking is widely used to study driver's behavior in simulated environment. However, we're used to either have glasses (e.g. [Pupil Core](https://pupil-labs.com/products/core/)) or fixed setups (e.g [Smart Eye Pro](https://smarteye.se/research-instruments/se-pro/)); and the newly released VR headsets with included eye-tracking (e.g. [Vive Pro Eye](https://www.vive.com/eu/product/vive-pro-eye/overview/) bring new challenges. The one we'll cover today is the visualization of live eye-tracking data, why and how we managed to implement it in our platform.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I&#39;ve managed to display live VR <a href="https://twitter.com/hashtag/EyeTracking?src=hash&amp;ref_src=twsrc%5Etfw">#EyeTracking</a> (Vive Pro Eye) data on a <a href="https://twitter.com/hashtag/UE4?src=hash&amp;ref_src=twsrc%5Etfw">#UE4</a> spectator screen, which is recorded alongside raw data with <a href="https://twitter.com/hashtag/LabStreamingLayer?src=hash&amp;ref_src=twsrc%5Etfw">#LabStreamingLayer</a>. I&#39;ll probably write up a blog entry about this, as it&#39;s not trivial but really useful to have. <a href="https://t.co/hgNsn3cpgk">pic.twitter.com/hgNsn3cpgk</a></p>&mdash; Bertrand Richard (@brifsttar) <a href="https://twitter.com/brifsttar/status/1460649252485570561?ref_src=twsrc%5Etfw">November 16, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

# Eye-tracking data

## Traditional

So before getting into the details, it's important to mention why we use eye-tracking, and what type of information we're looking to get from it.

Whether in [CAVE](/just-a-videogame#cave-rendering) or in VR, we want to know *what* the driver is looking at, and most importantly, *when* in relation to events happening in the scenarios. One common question in car and pedestrian interaction is "Did the driver see the pedestrian? If so, how far were they when it happened?".

For the *when* part, we already covered at least part of it in our [data collection](data-collection/) article, which dives a bit into the multiple data sources synchronization problematic. But to sums things up, this is the easy part, thanks to tools like [LabStreamingLayer](https://github.com/sccn/labstreaminglayer).

[![pupil_core.jpg]({{site.baseurl}}/images/pupil_core.jpg)][0]

The *what* part, on the other end, is usually quite challenging. In non-headset setups, it's fairly difficult to get the information of the actual object in the simulated world being looked at, and getting more accurate than that (e.g. what part of the object) is nearly impossible. This is mainly because both the participant and the scene are dynamic: ego and [NPCs](https://en.wikipedia.org/wiki/Non-player_character) both are moving in the world, pushing even the advanced workflows (e.g. dynamic area of interests) to their limits. And when you add on top of that the inherent relative inaccuracy of eye-trackers, it gets really complex to automate any kind of data-processing about *what* the participant is looking at.

Since we mainly work with glasses, our best alternative has been manual annotation. That is, after the experiment, we go over each video and manually annotate what object is being looked at, based on the "red dot" which is overlaid on the sene video (captured from the glasses), using eye-tracker processed data.

<video width="720" height="480" controls>
  <source type="video/mp4" src="{{site.baseurl}}/images/pupil.mp4">
</video>

It's obviously time consuming and not very reliable, but given our set of constraints, it's the best we can do.

## VR headsets

But then came VR-headsets with integrated eye-tracking, which changed output data and downstream workflows quite a bit.

The main change is that the *what* issue is now much simpler. Since both the eye-tracker and the displays are in the same reference frame (headset), it's much easier to convert eye-tracking data to simulated world. In our case, the [SRanipal Unreal Plugin](https://developer-express.vive.com/resources/vive-sense/eye-and-facial-tracking-sdk/) can give us the scene actor being looked at, with no post-processing needed! So no more manual annotation and all its downsides; we can, at runtime, know what actor is being looked at. This makes post-processing analysis way easier.

# Why live visualization?

So, if eye-tracking-VR-headsets are so great and can tell us, in realtime, what object is being looked at; why would we want a live "2D-red-dot" visualization, that was previously only used as last resort for manual annotation? Indeed, that video should be pretty much useless to us now.

Well, multiple reasons. The first is that such video is still useful during the analysis phase if you want to get a quick glimpse at the dataset. For humans, a video with a red dot is much easier to understand than a time-series dataset, and if you want to get a "feel" of your data, it's great material.

Second is that the eye-tracking data, whether in VR or glasses, still has accuracy limitations. Meaning we can't blindly trust its "actor being looked at" data, because it might actually be wrong. And during our tests, we found that it was way too often wrong. Whether your have multiple objects in close proximity at far range, if you're looking at the edge of an object, or a simple offset from the calibration; many things can induce error in that high level data.

If we look back at the example from the top of this post, I was actually looking at the two wheels, then the licence plate. But if I were to look at the "object being looked at" data, it would just say "ground". Quick disclaimer though: I didn't calibrate the eye-tracker before recording this, so usually we don't have that big offsets.

The third reason behind the need for this type of visualization is as a safety measure so that the experimenter can check, during the experiment, that the eye-tracker is working as intended. We also implemented live [smoke tests](https://en.wikipedia.org/wiki/Smoke_testing_(software)) for that, but having a mean for a human to verify it is always reassuring for the experimenter.

A final advantage to having the video is that it can easily be used to illustrate specific cases in publication, conference or simply when discussing between colleagues.

# Implementation

Now that we've explained why we want "red-dot" live visualization for VR eye-tracking data, let's dive into how we actually implemented it in our Unreal Engine simulation platform.

## Spectator screen

One obvious thing is that we want the live eye-tracking visualization to *not* appear in the participant's headset; only the experimenter should be able to see it. That's where the [Spectator Screen](https://docs.unrealengine.com/4.27/en-US/SharingAndReleasing/XRDevelopment/VR/VRHowTos/VRSpectatorScreen/) comes into play.

## Drawing

## Recording

[0]: https://pupil-labs.com/products/core/