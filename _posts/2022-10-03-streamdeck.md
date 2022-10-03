---
published: true
image: streamdeck.jpg
title: StreamDeck for Driving Simulation Experiments
---
I'm always on the lookout for new and often out of place tools that could find their role in our research workflows. One example is [Veyon](https://veyon.io/): built for classroom computers management, it also happens to be perfect for managing [nDisplay clusters](/whats-new-2021-11/#automation), at the core of our simulators. [StreamDeck][sd] are a recent addition to our fleet, and today we'll discuss all the use cases in which those will greatly help.

Disclaimer: we're not sponsored by StreamDeck's [elgato](https://www.elgato.com/en) in any way, we just really like the product and think others might too.

# *Stream-*What now ?

![streamdeck.jpg]({{site.baseurl}}/images/streamdeck.jpg)

What is a StreamDeck you may ask? Basically, it's a button box where each button has its own LCD display. There are various sizes, from 6 to 32 buttons. It comes with software, so you can bind pretty much anything on any button press.

Marketed towards [online streamer](https://en.wikipedia.org/wiki/Online_streamer), the simple design and easy customization of those devices have lead them to be used in many applications beyond their target audience.

I first came across those when looking if [Art. Lebedev](https://www.artlebedev.com/)'s mid-2000s [Optimus Maximus keyboard](https://www.artlebedev.com/optimus/maximus/) ever went into production, or if something similar made it to market. I thought such devices would easily be configured for various use cases at our lab, so once I discovered that StreamDecks where a thing, I quickly bought a couple to see what could be done with it.

# Experimenter Deck

The application that immediatly came to mind was what I'd call an "[Experimenter Deck](/worfklow-2/#streamdeck)".

I'm a huge believer in [reproducibility](https://en.wikipedia.org/wiki/Reproducibility) and automation. Working in a psychology lab, where all experiments involve both human subjects and complex simulation hardware, means reproducibility can be quite challenging to achieve at a high level. Not only does the experimenter have to interact with the subject, which can induce bias, they also have to interact with lots of simulation equipment, which can result in operator errors.

![streamdeck_monipost.jpg]({{site.baseurl}}/images/streamdeck_monipost.jpg)

I saw StreamDecks as an opportunity to help make the researcher's job easier when running the experiment. The idea is to reduce the researcher's interaction with all the hardware and software running the experiment to the minimal set of buttons on this deck.

My goal, and we're not there just yet, is for the researcher be able to specify what they want their input to the overall experiment system to be, and have the StreamDeck include all of those, and only those. In other words, researchers should be able to run their whole experiment using nothing but the StreamDeck as input device.

In such setups, most processes would therefore be automated, reducing risk of human error and improving the experiment quality and reproducibility. Researchers would only need to provide input when they deemed it necessary for the [protocol](https://en.wikipedia.org/wiki/Protocol_(science)). StreamDecks, and their customizable buttons, makes creating such interfaces an achievable goal. We're starting to create button layouts for each experiment we run, each one having their own input types.

# Annotating/coding

In psychology research, we often rely on [behavior coding](https://dictionary.apa.org/behavior-coding) to extract more information from raw data. This process is usually done manually using specialized software, like Noldus' [The Observer XT](https://www.noldus.com/observer-xt), [iMotions](https://imotions.com/platform/), or the open-source (therefore my favorite) [BORIS](https://www.boris.unito.it/).

"Coding" usually involves watching a video, and either clicking on buttons using the mouse, or the faster alternative: pressing keybord shortcuts. However, when your coding scheme is large enough, remembering all keyboard shortcuts becomes quite challenging, which here also can easily become a source of operator error.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Serving you <a href="https://twitter.com/hashtag/Ethogram?src=hash&amp;ref_src=twsrc%5Etfw">#Ethogram</a> realness. <br><br>Next stop: <a href="https://twitter.com/BORIS_behav_obs?ref_src=twsrc%5Etfw">@BORIS_behav_obs</a>! Everyone cross your fingers &amp; put good inter-rater reliability vibes into the universe!<a href="https://twitter.com/hashtag/AnimalBehavior?src=hash&amp;ref_src=twsrc%5Etfw">#AnimalBehavior</a> <a href="https://twitter.com/hashtag/PhDLife?src=hash&amp;ref_src=twsrc%5Etfw">#PhDLife</a> <a href="https://twitter.com/hashtag/ScienceTwitter?src=hash&amp;ref_src=twsrc%5Etfw">#ScienceTwitter</a> <a href="https://t.co/2hUgDHGR6C">pic.twitter.com/2hUgDHGR6C</a></p>&mdash; Karli &quot;swell selkie soul&quot; Rice Chudeau 🦭 (@KarliRChudeau) <a href="https://twitter.com/KarliRChudeau/status/1521217924597395456?ref_src=twsrc%5Etfw">May 2, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

As shown above, there are very affordable and easy to implement solutions to this, but StreamDeck are just perfect for this. Setup your layout and start coding. Icons can even be animated if that helps!

# Non-driving related tasks

The research field studying driver's behavior loves their Non-Driving Related tasks, or NDRT as we call them. As its name suggests, it's everything a driver might do while behind the wheel that's *not* driving, and that's most likely a distraction from the car operating. That includes setting the GPS, using a phone or changing radio station.

There are many ways to create distractions while driving in a simulator, some are so common, like the "Surrogate Reference Task" (SuRT), that they're actual [ISO standards](https://www.iso.org/obp/ui/#iso:std:iso:ts:14198:ed-1:v1:en).

But since each project has its own goals and requirements, we sometimes have to design tasks ourselves to make sure we have the exact type of distraction that fits our needs. Those often involve some type of button box, allowing the subject to provide different types of inputs based on external stimulus.

[![launchpad.png]({{site.baseurl}}/images/launchpad.png)][launchpad]

So we're always looking for new tools to build interfaces with which the participant will interact. Not so long ago, I suggested the use of a [Launchpad][launchpad] as the base of a new NDRT, and apparently it did the job quite well. We've yet to use a StreamDeck in such cases, but it's safe to say that it would also find its place there.

# Emergency Soft Stop

A simulator is a complex machinery, which often involves actuators, either for cockpit motion, or at the very least for steering wheel force feedback. As such, all our simulators are equipped with multiple emergency stop buttons, stopping all motions.

But not all risks come from actuators. Anyone using driving simulators can tell you that the absolute predominant risk is [simulator sickness](https://en.wikipedia.org/wiki/Simulator_sickness). When symptoms arise, you want the participant to get out of the simulator quickly, but not too much. They should first stop the car, and get out as soon as they feel like doing so. Stopping the simulation too abruptly might worsen the symptoms, so it's best to only use such measures as last resort.

However, that's not always possible. With the rise of autonomous driving scenarios, the participant sometimes doesn't have control over the vehicle, and therefore can't stop it. In other extreme cases, simulator sickness can appear very quickly (e.g., after a sharp turn), and the driver might have difficulties engaging any manoeuvre, being in a shock-like state.

During an actual experiment, the researcher is in the control room and therefore has access to all safety features (on their [Experimenter Deck](/streamdeck/#experimenter-deck)!) that act as a "Emergency Soft Stop", such as:

* Stopping all vehicles (including ego) with gentle braking
* [Fading-out](https://en.wikipedia.org/wiki/Dissolve_(filmmaking)) all displays
* Stopping the simulation

But during demonstration, testing, or other events, it can happen that the simulator operator is in the car, either as passenger or driver. In such case, they can't access the main "Experimenter Deck" to stop the simulation.

![streamdeck_emergency.jpg]({{site.baseurl}}/images/streamdeck_emergency.jpg)

Which is why we added a smaller StreamDeck ([MK.2](https://www.elgato.com/en/stream-deck-mk2)) inside the car, with bindings for all the "Emergency Soft Stop" functions. That way, in case of simulator sickness, we always have a way to gently stop the simulation from the actual cockpit.

# And more?

Those are just a few use cases for StreamDecks in our driving simulation experiments. It's been less than a year since we discovered those devices, so we'll probably find even more over time. The [Stream Deck Mobile](https://www.elgato.com/en/stream-deck-mobile) also looks very interesting, but we haven't had the chance to test it yet. But we've yet to find a use case for the [Stream Deck Pedal](https://www.elgato.com/en/stream-deck-pedal)!

[launchpad]:https://novationmusic.com/en/launch/launchpad-x
[sd]: https://www.elgato.com/en/stream-deck
