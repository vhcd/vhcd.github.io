---
published: false
image: streamdeck.jpg
title: StreamDeck for Driving Simulation Experiments
---
I'm always on the lookout for new and often out of place tools that could find their role in our research workflows. One example is [Veyon](https://veyon.io/): built for classroom computers management, it also happens to be perfect for managing [nDisplay clusters](/whats-new-2021-11/#automation), at the core of our simulators. [StreamDeck][sd] are a recent addition to our fleet, and today we'll discuss all the use cases in which those will greatly help.

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

[sd]: https://www.elgato.com/en/stream-deck