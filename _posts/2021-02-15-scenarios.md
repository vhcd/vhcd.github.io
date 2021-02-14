---
published: false
title: Scenarios
---
An important and critical stage in driving simulator experiments is scenario authoring. We want to offer as much control as possible to researchers, allowing them to build any experiment they can imagine. But we also want this process to be as easy and intuitive as possible, so that non-experts can start working on their scenario as early as possible in the experiment design phase, allowing for quick and iterative development.

# OpenSCENARIO

After [OpenDRIVE](/opendrive) was initially released, the [PEGASUS](https://www.pegasusprojekt.de/en/) consortium started to work on an even more ambitious standard: [OpenSCENARIO](https://www.asam.net/standards/detail/openscenario/).

> ASAM OpenSCENARIO defines a file format for the description of the dynamic content of driving and traffic simulators. The primary use-case of OpenSCENARIO is to describe complex, synchronized maneuvers that involve multiple entities like vehicles, pedestrians and other traffic participants.

In the same way as OpenDRIVE, OpenSCENARIO is currently between two versions. Its 1.x release is still being worked on, all the while a much more ambitious [2.0 Concept](https://www.asam.net/index.php?eID=dumpFile&t=f&f=3408&token=afd0585fb2e8e6d760b441fdf485548407ddb977) is nearing release. Even if the end goal is for the two releases two [converge](https://www.asam.net/index.php?eID=dumpFile&t=f&f=3468&token=22f02c42a0a47696cae7e81a2310c74ecf7f218c), right now the standard is in a weird place.

It doesn't help that, just as the beginning as OpenDRIVE, tooling is not quite there yet. Even though you can find some players (like [esmini](https://github.com/esmini/esmini)), I don't have knowledge of any *editor* out there. I know that [VectorZero](https://www.vectorzero.io/) was working on a [Scenario Editor](https://tracetransit.atlassian.net/wiki/spaces/VS/pages/764116999/Scenario+Editor+ALPHA+User+Guide) before it was bought by Mathworks, but no news on that. The current "two versions" state of the standard definitely doesn't help anyone looking to fill that space.

[![openscenario.png]({{site.baseurl}}/images/openscenario.png)][0]

That alone could be enough to deter us from using OpenSCENARIO, but I'm not really pleased with the standard itself. I haven't spent much time studying it, but the 1.x, and its XML format, was too restrictive in its scenario definition, and I've found quite a few examples of scenarios we had previously implemented that wouldn't have been possible with this standard.

The 2.0 Concept release, on the other hand, is too broad in coverage, ranging from automated testing for ADAS development, to driver model, and more. Last I checked, they even had their own [Domain-specific language](https://en.wikipedia.org/wiki/Domain-specific_language), which might be a good idea in theory, but doesn't really align with our "easy and intuitive" philosophy.

So I'm not a big fan of OpenSCENARIO, but I'm still following their work, hoping that the future will bring more features in alignment with our "simple" driving simulation requirements.

# What are scenarios anyway?

One reason why OpenSCENARIO doesn't really fit our needs is because we tend to push the definition of what a scenario is. As an engineer, I'm always looking for new ways to allow researchers to expand simulator experiments beyond the current state of the art. We're finding new and innovative ways to study drivers behavior.

One example is a [study](https://www.researchgate.net/publication/343126505_Innovative_methodological_framework_for_virtual_human_centered_design_An_application_to_automated_driving_effects_on_driver's_decisions-making) by [Jonathan Deniel](https://www.researchgate.net/profile/Jonathan_Deniel), where he looked at automated driving effects on driver's decision-making. To do that, participants experienced multiple aumotated lane-changes, and based on their feedback for each, custom scenarios were procedurally generated at runtime to have more in-depth insights for each participant.

Another example is a complex study on take-over from automated driving in critical situations, where we designed and tested multiple HMIs to help the driver take over. For this experiment, we had to implement various innovative HMIs, sometimes requiring complex interactions, and develop scenarios to make it so that each one could be run with each HMI.

In this same experiment, we debuted a new way to get insights into driver's mental reprensetation. At the end of each scenario, right after the take-over, the scene freezes, all actors disappear, and 6 slightly different thumbnails appear on the touch screen, each showing the situation that the participant experienced a few second before, but with slight modifications, in a similar way to the [Spot the difference](https://en.wikipedia.org/wiki/Spot_the_difference) puzzle. The participant could select each thumbnail to view the full scene on the simulator to clearly identify the differences (e.g. car in the blindspot, pedestrian). The participant then had to select the scene he thought was the one he had jut experienced, allowing us to have data about what he had, hadn't or thought he had perceived. This innovative feature will either be the subject of a future publication, or in the very least of post in this blog.

Those examples illustrate some ways in which we're expanding the usual definition of what a scenario is, allowing researchers to study more subjects at greater depth, and requiring agile development of the platform.

# Beyond scenarios: *Passation*

[0]: https://www.asam.net/conferences-events/detail/webinar-asam-openscenario/
