[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

RLlib integration brings support between the [Ray/RLlib](https://github.com/ray-project/ray) library and the [CARLA simulator](https://github.com/carla-simulator/carla), allowing the easy use of the CARLA environment for training and inference purposes.


## Setup Carla

To use CARLA, go to the [**CARLA releases**](https://github.com/carla-simulator/carla/releases) webpage and download version 0.9.11. While other versions might be compatible, they haven't been fully tested, so procede at your own discretion.

Additionally, set the **CARLA_ROOT** environment variable to the folder containing the CARLA server.

## Project Organization

This repository is organized as follows:

* **aws** has the additional files to run all of this in an AWS instance.

* **rllib_integration** contains all the infraestructure used to set up the CARLA server, clients and the training and testing experiments. It is important to note that **base_experiment.py** has the base settings of the experiments (at `BASE_EXPERIMENT_CONFIG`), as well as the `BASE_CORE_CONFIG` at **carla_core.py**, were all the CARLA defualt settings are set up. Additionally, if you want to create your own pseudosensor, check out **sensors/birdview_manager.py**, which is a simplified version of the [CARLA's no rendering mode](https://github.com/carla-simulator/carla/blob/master/PythonAPI/examples/no_rendering_mode.py).

* **dqn_example**, as well as all the **dqn_*** files provide an easy to understand example on how to use the tools available at the previously explained folder, and uses ray's [DQNTrainer](https://github.com/ray-project/ray/blob/master/rllib/agents/dqn/dqn.py#L285). This includes both training and inference:
    * **dqn_train.py** is the entry to the training. It has one compulsory argument, which should be the path to the experiment configuration. Here, the configuration is parsed and given to ray, along with the experiment and CARLA environment classes.
    * **dqn_config.yaml** is a yaml configuration file. This should be the argument given to the _dqn_train.py_ file. Both the experiment and CARLA settings, as well as the ray ones can be changed from here. These will be updated on top of the default ones.
    * **dqn_inference.py** is the script used to start an inference. For this example, one argument is needed, the path to a pytorch checkpoint. 
    * **dqn_inference_ray.py** holds the same functionality as the previous inference file but in this case, it is done using the 
    ray library
    * At **dqn_example/dqn_experiment.py**, the experiment class is created, with all the information regarding the action space, observation space, actions and rewards needed for the DQN. It is recommended that all experiments inherit from `BaseExperiment`, at **rllib_integration/base_experiment.py**

