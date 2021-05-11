---
published: false
title: VR Headsets
---
For the past ten years, the world of [Virtual Reality headsets](https://en.wikipedia.org/wiki/Virtual_reality_headset) has been redefined, lead by Facebook's [Oculus](https://www.oculus.com/) and HTC's [Vive](https://www.vive.com/). Those relatively cheap devices allow for better immersion in various types of environments, and are now being used in a wide range of both industry and research entities. But, even though we've previously mentionned use of [CAVE](/nDisplay) simulators, we never talked about VR headsets. Why is that?

# Why we don't use them

The title is voluntarily misleading, as we own and use multiple VR headsets, but not for *driving simulation* just yet. The technology has been and is still improving a lot, and we've been playing with 3 generations of headsets for over 5 years now; using them for demos, *pedestrian simulation* or just internal testing. But we haven't used VR headsets for any *driving simulation* experiment, nor do we have any planned in the short term. And there are multiple reasons behind that.

## Low resolution

HTC recently announced its [Vive Pro 2](https://www.roadtovr.com/htc-vive-pro-2-specs-price-release-date-announcement/) with a single eye resolution of 2448×2448, compared to the original Vive's 1080x1200. So there's a lot of progress on the resolution front, but is that enough for driving simulation? I'm still not sure.

[![screendoor.jpeg]({{site.baseurl}}/images/screendoor.jpeg)][0]

On a driving simulator, you need to be able to see a lot of things, far and accurately. When driving at 90km/h, you perceive objects that can be well over 100m and cover only a very small fraction of your field of view. If drivers can't make up *what* it is they see in the distance because there's not enough pixels, that's a big issue. As a consequences, drivers might alter their driving behaviour for *simulation* reasons, which is something we try to avoid as much as we can (even though we obviously can't totally get rid of).

You also want to see your dashboard with as much accuracy as possible. Nowadays, needle displays tend to disappear for more digital interfaces, but there's still a lot of information on the dashboard (or [infotainment system](https://en.wikipedia.org/wiki/In-car_entertainment)), which is often hard to read on VR headsets.

## Head data

At [LESCOT](https://lescot.univ-gustave-eiffel.fr/), we study driver's behavior, and in order to do that, we use multiple data sources related to the head of the participant, which can be incompatible (to some degree) with VR headsets.

Historically, we've been using video recordings of driver's face to gauge their reaction, expressions, or more recently emotions. With a headset covering half their faces, this becomes pretty much impossible.

[![fnirs.png]({{site.baseurl}}/images/fnirs.png)][1]

We also make heavy use of [eye tracking](https://en.wikipedia.org/wiki/Eye_tracking)

## Mirrors

## Dashboard interaction

# Why we will

## Varjo

## Manus-VR

## nDisplay

## Portable and development setup

[0]: https://www.reddit.com/r/oculus/comments/40x9f9/has_anyone_managed_to_get_a_photo_from_cv1_screen/
[1]: https://lescot.univ-gustave-eiffel.fr/