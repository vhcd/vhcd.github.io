---
published: false
title: Virtual Driver
---
In most driving simulation scenarios, there are two types of cars. The one that's driven by the participant (commonly refered to as *ego*), and others that are controlled from the scenario. With the rise of autonomous driving, it's not uncommon to have scenarios where *ego* also is controlled from the scenario. How do we control all those cars, making sure that their external behavior appears realistic, all the while being easy to configure for researchers building their experiment?

# Virtual Driver

At [LESCOT](https://lescot.univ-gustave-eiffel.fr/en/), we've been working on a cognitive model of the driver (COSMODRIVE, [1], [2], [3]) for a few decades. In that time, we've implemented different versions of the model, each focused on one part or another based on projects requirements. After the last iteration within the [Holides](http://holides.eu/) project, we decided to "spin off" what we call the *operational* and *execution* modules into a seperate project called the **Virtual Driver**.

The Virtual Driver

[1]: https://www.researchgate.net/publication/281074875_A_computational_model_for_car_drivers_situation_awareness_simulation_Cosmodrive
[2]: https://www.researchgate.net/publication/242182916_Modelisation_et_simulation_cognitive_de_l'operateur_humain_une_application_a_la_conduite_automobile
[3]: https://www.researchgate.net/publication/326956022_Computational_Driver_Model_in_Transport_Engineering_COSMODRIVE