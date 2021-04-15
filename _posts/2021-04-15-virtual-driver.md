---
published: true
title: Virtual Driver
---
In most driving simulation scenarios, there are two types of cars. The one that's driven by the participant (commonly refered to as *ego*), and others that are controlled from the scenario. With the rise of autonomous driving, it's not uncommon to have scenarios where *ego* also is controlled from the scenario. How do we control all those cars, making sure that their external behavior appears realistic, all the while being easy to configure for researchers building their experiment?

# Birth of the Virtual Driver

At [LESCOT](https://lescot.univ-gustave-eiffel.fr/en/), we've been working on a cognitive model of the driver (*COSMODRIVE*, [1], [2], [3]) for a few decades. In that time, we've implemented different versions of the model, each focused on one part or another based on projects' requirements. After the last iteration within the [Holides](http://holides.eu/) project, we decided to "spin off" what we call the *operational* and *execution* modules into a seperate project called the **Virtual Driver**.

As part of our cognitive model, those modules would take as input *tactical goals*, and produce as output human-like car control (e.g. pedal depression, steering wheel angle). Once we've externalized those, we can see that these inputs and outputs actually fit pretty well in a driving simulation environment. The *Virtual Driver* is therefore what we currently use to control all cars in our scenarios.

# Goals (input)

I won't go into the complete description of what *tactical goals* are, since it's a very complex subject. But in the *Virtual Driver*, we've reduced them to human descriptions of road manoeuvers. The most basic example could be "maintain a speed and keep your lane".

Currently, goals can be of different *levels*. The lowest level splits goals into *lateral* (i.e. steering) and *longitudinal* (i.e. pedals) control. That way, you can plan a trajectory along the road before actually knowing what speed you want along that trajectory. A common example is junction crossing: you pretty much know where you want the car to go (e.g. left), but its speed will depend on external factors, such as traffic light state or incoming cars.

Here are some lower level goal examples, each having a variety of parameters that we won't go into:
* Keep a lane
* Maintain a speed
* Reach a speed in an amount of time/distance
* Follow a lead car
* Follow a hand-drawn trajectory

The second, higher, level merges lateral and longitudinal controls to offer a more intuitive way to give orders to cars from your scenario. Under the hood, they're composition of lower level goals. Here are some examples:

* Change lane
* Cross a junction
* Maintain lane and speed

# Human-like control (output)

The *Virtual Driver* produces human-like outputs, that can directly be plugged into any simulator's car dynamic model. That way, from the participant's point of view, any car's external behaviour (e.g. dynamics) looks realistic enough to ensure optimal immersion. 

On the technical side, the output from the *Virtual Driver* actually are exactly the same as you would get from a real human using a standard controller. This means we can swap from real to virtual driver (and vice versa) at any point within our scenarios! This is especially useful for autonomous driving or take-over studies, where shared car control can be challenging. 

# Back towards Cosmodrive

Under the hood, the *Virtual Driver* makes heavy use of the [OpenDRIVE](/opendrive) description of the road network. This is required to know where our lane is, what "turn left at the next junction" means and so on.

Our goal in the near future is to rebuild layers after layers of *Cosmodrive* on top of this new *Virtual Driver*. This implies for example adding even higher level goals, building an internal mental representation of the world, or implementing a whole perception module, that would in the end remove the need for OpenDRIVE files, since the model would be able to perceive the environment itself.

# Virtual Driver in scenarios

This last project is, of course, very challenging and will require extensive work, but for now, our *Virtual Driver* is perfect for our driving simulation platform, as it allows intuitive control for the scenario author, as can be seen in the following scenario excerpt shown in our [scenarios](/scenarios) entry.

![lvlbp.png]({{site.baseurl}}/images/lvlbp.png)

Each car has a *Virtual Driver*, and if you want it to cross a junction, there's a box for that! And if you don't really want to individually control each car, there's a checkbox named "Autonomous", which as its name suggests, will let the *Virtual Driver* decide what to do. For now, it simply wanders around the scene, following its lane (and crossing junctions, of course). But we've already started to make it smarter, and that will be the subject of another future entry.

[1]: https://www.researchgate.net/publication/281074875_A_computational_model_for_car_drivers_situation_awareness_simulation_Cosmodrive
[2]: https://www.researchgate.net/publication/242182916_Modelisation_et_simulation_cognitive_de_l'operateur_humain_une_application_a_la_conduite_automobile
[3]: https://www.researchgate.net/publication/326956022_Computational_Driver_Model_in_Transport_Engineering_COSMODRIVE
