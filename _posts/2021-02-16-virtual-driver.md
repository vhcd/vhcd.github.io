---
published: false
title: Virtual Driver
---
In most driving simulation scenarios, there are two types of cars. The one that's driven by the participant (commonly refered to as *ego*), and others that are controlled from the scenario. With the rise of autonomous driving, it's not uncommon to have scenarios where *ego* also is controlled from the scenario. How do we control all those cars, making sure that their external behavior appears realistic, all the while being easy to configure for researchers building their experiment?

# Birth of the Virtual Driver

At [LESCOT](https://lescot.univ-gustave-eiffel.fr/en/), we've been working on a cognitive model of the driver (COSMODRIVE, [1], [2], [3]) for a few decades. In that time, we've implemented different versions of the model, each focused on one part or another based on projects requirements. After the last iteration within the [Holides](http://holides.eu/) project, we decided to "spin off" what we call the *operational* and *execution* modules into a seperate project called the **Virtual Driver**.

As part of our cognitive model, those modules would take as input *tactical goals*, and produce as outputs human-like car control (e.g. pedal depression, steering wheel angle). Once we've externalized those, we can see that their inputs and outputs actually fits pretty well in a driving simulation environment.

# Goals

I won't go into the complete dexcription of what *tactical goals* are, since it's a very complex subject. But in the *Virtual Driver*, we've reduced them to human descriptions of road manoeuvers. The most basic example could be "Maintain a speed and keep your lane".

Currently, goals can be of different *levels*. The lowest level splits goals into *lateral* (i.e. steering) and *longitudinal* (i.e. pedals) control. That way, you can plan a trajectory along the road before actually knowing what speed you want along that trajectory. A common example is junction crossing: you pretty much know where you want the car to go (e.g. left), but it's speed will depend on external factors, such as traffic light state or incoming cars.

# OpenDRIVE

# Back towards Cosmodrive

[1]: https://www.researchgate.net/publication/281074875_A_computational_model_for_car_drivers_situation_awareness_simulation_Cosmodrive
[2]: https://www.researchgate.net/publication/242182916_Modelisation_et_simulation_cognitive_de_l'operateur_humain_une_application_a_la_conduite_automobile
[3]: https://www.researchgate.net/publication/326956022_Computational_Driver_Model_in_Transport_Engineering_COSMODRIVE