---
title: Parallel
---


# Parallel API

In addition to the main API, we have a secondary parallel API for environments where all agents have simultaneous actions and observations. An environment with parallel API support can be created via `<game>.parallel_env()`. This API is based around the paradigm of *Partially Observable Stochastic Games* (POSGs) and the details are similar to [RLlib's MultiAgent environment specification](https://docs.ray.io/en/latest/rllib-env.html#multi-agent-and-hierarchical), except we allow for different observation and action spaces between the agents.

All parallel environments can be converted into AEC environments by splitting a simultaneous turn into sequential turns, with observations only from the previous cycle.

We provide tutorials for creating two custom Parallel environments: [Rock-Paper-Scissors](https://pettingzoo.farama.org/content/environment_creation/#example-custom-parallel-environment), and a simple [gridworld environment](https://pettingzoo.farama.org/tutorials/environmentcreation/2-environment-logic/)

## Usage

Parallel environments can be interacted with as follows:

``` python
from pettingzoo.butterfly import pistonball_v6
parallel_env = pistonball_v6.parallel_env(render_mode="human")
observations = parallel_env.reset(seed=42)

while env.agents:
    # this is where you would insert your policy
    actions = {agent: parallel_env.action_space(agent).sample() for agent in parallel_env.agents}

    observations, rewards, terminations, truncations, infos = parallel_env.step(actions)
env.close()
```

## ParallelEnv

```{eval-rst}
.. currentmodule:: pettingzoo.utils.env

.. autoclass:: ParallelEnv

    .. py:attribute:: agents

        A list of the names of all current agents, typically integers. These may be changed as an environment progresses (i.e. agents can be added or removed).

        :type: list[AgentID]

    .. py:attribute:: num_agents

        The length of the agents list.

        :type: int

    .. py:attribute:: possible_agents

        A list of all possible_agents the environment could generate. Equivalent to the list of agents in the observation and action spaces. This cannot be changed through play or resetting.

        :type: list[AgentID]

    .. py:attribute:: max_num_agents

        The length of the possible_agents list.

        :type: int

    .. py:attribute:: observation_spaces

        A dict of the observation spaces of every agent, keyed by name. This cannot be changed through play or resetting.

        :type: Dict[AgentID, gym.spaces.Space]

    .. py:attribute:: action_spaces

        A dict of the action spaces of every agent, keyed by name. This cannot be changed through play or resetting.

        :type: Dict[AgentID, gym.spaces.Space]

    .. automethod:: step
    .. automethod:: reset
    .. automethod:: seed
    .. automethod:: render
    .. automethod:: close
    .. automethod:: state
    .. automethod:: observation_space
    .. automethod:: action_space

```
