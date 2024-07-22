---
published: true
title: 'Buttkicker + Unreal Engine'
image: 'uebk.png'
---

Want to use a [Buttkicker](https://thebuttkicker.com/) in an Unreal Engine project? There are many ways to go about it, so let's cover a few.

# What's a Buttkicker?

You can think of it as a "shaky subwoofer on steroids". It's a device used in the simulation community (e.g., plane, racing) to increase immersion by adding vibration to the cockpit.

[![](/images/bksim.png)][0]
<center><i>Source: FPZERO Simulators</i></center>

# Problem

The effect's strength is derived from the audio signal, which has its advantages but also drawbacks. In our case, the main issue is that we want to control the vibration independently of the simulation sound; for example we might want to have some feedback from the device even when no sound is being played.

So let's explore the solutions to control our Buttkicker outside of the main audio channels.

# Solutions

## Use the Buttkicker SDK

Well, it doesn't exist, so... The amplifier has a USB input, which gave me some hopes that their would be some SDK with that; but no, it's just an audio device.

## Use an external sound adapter...

Since an audio signal is the only way to control the Buttkicker, and since we also want to control it seperately from our main audio, a straightforward solution could be to use a second audio adapter on the computer (like [this one][1]) dedicated to the device. Then we would just have to send specific audio signal to this adapter to control the Buttkicker.

[![](/images/audio_adapter.png#center)][1]

### ... in Unreal

Let's do that in Unreal! Well, vanilla Unreal doesn't really handle multiple audio output devices. And by that, I mean it definitely doesn't. I dove into the source code (which I just love be able to do), and saw that both [`FMixerDevice`](https://github.com/EpicGames/UnrealEngine/blob/5.3/Engine/Source/Runtime/AudioMixer/Public/AudioMixerDevice.h) and the Windows interface [`FMixerPlatformXAudio`](https://github.com/EpicGames/UnrealEngine/blob/5.3/Engine/Source/Runtime/Windows/AudioMixerXAudio2/Private/AudioMixerPlatformXAudio2.h) very much expect only one device being used at a time.

However, we could explore other audio interfaces within Unreal. I'm guessing [Wwise](https://www.audiokinetic.com/en/products/wwise/) can do that, and [SDL](https://www.libsdl.org/) probably too. I didn't explore any of those solution further, as it was beyond my very low motivation threshold for this project.

### ... outside Unreal

[Python](https://www.python.org/) is [pretty cool](https://xkcd.com/353/), you know? There's a lib for everything, including, [playing sound](https://python-sounddevice.readthedocs.io/) on [any device](https://python-sounddevice.readthedocs.io/en/latest/usage.html#device-selection).

So we implemented the following:
* On Unreal's side, send whatever information we want to use as variable for the Buttkicker. We used [ZeroMQ](https://zeromq.org/) ([Unreal plugin](https://github.com/hdhauk/UnrealZeroMQ)) because we like it, but TCP or UDP would also work as well.
* On Python's side, we continuously [play a signal](https://python-sounddevice.readthedocs.io/en/0.4.6/examples.html#play-a-sine-signal) modulated according to the data received. All that just on the dedicated Buttkicker audio adapter, of course.

And it works great! A bit too complex for my taste though...

## Use MetaSound...

Even though the "two audio adapters" solution works fine, I wanted to try to find something less complex, ideally that doesn't require either special hardware or external software.

[MetaSound](https://docs.unrealengine.com/5.3/en-US/metasounds-the-next-generation-sound-sources-in-unreal-engine/), Unreal's new sound system, allows us to output sound to [any audio channel](https://docs.unrealengine.com/5.3/en-US/metasounds-reference-guide-in-unreal-engine/) we want. So let's imagine that I...
* Am using a Stereo sound system (actual non-Buttkicker sound)
* Plug the Buttkicker to the Rear output on my sound adapter
* Tell Windows that I'm instead using a Quad sound system
* Use a MetaSound Source to only output on the Rear channels
* Vary said sound according to whatever variable in the simulation

Would that work? Well, kind of. It has a few limitations, the major one being that since Unreal thinks there's a Quad system, it spatializes sounds accordingly, but since the rear speakers don't actually exist, all that sound is lost (i.e., sent to the Buttkicker), and you end up not being able to hear anything happening behind you.

Another issue is that even if the "Buttkicker" sound is not played on the main audio device, the inverse is not true: the main audio sound is played on the Buttkicker. So if a loud sound plays on the Stereo system, it will produce vibration on the device. And if no "Buttkicker-only" sound is sent to the device, it will fallback to whatever background "real" sound is going through.

## ... with some rewiring

Since the drawbacks mentioned above were a bit too problematic for us, we instead did some rewiring: we [split](https://www.bhphotovideo.com/c/product/1598532-REG/hosa_technology_ymm_152_stereo_3_5mm_male_trs.html) the center/bass output, sending the center to the actual 5.1 speaker system, and the bass to the Buttkicker. The only drawback to this solution is that the surround system subwoofer isn't used, but since we have a Buttkicker in the first place, it's not problematic. Other than that, there are no spatialization issues, no bleeding either (except bass bleeding, which isn't a big problem anyway).

# Conclusion

![](/images/metasound_bk.png)

In the end, the MetaSound approach proved the right idea. It allowed us to create a very intuitive, fully integrated "API" straight in Unreal. Prototyping new effects was super fast, as we could "live" the simulation end result just by starting a *Play in Editor*.

We're so happy with the result that we're now planing to add Buttkickers to other simulators. The vibrations really add a lot (even more than we expected), and having full control of those right from our scenario design tool is a huge plus.

[0]: https://www.fpzero.co.uk/blog/buttkickers-what-are-the-benefits/
[1]: https://www.startech.com/en-us/cards-adapters/icusbaudiob