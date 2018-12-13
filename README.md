# Deep Reinforcement Learning with Double Q-learning

> 🤖 Flappy Bird hack using Deep Reinforcement Learning with Double Q-learning

## Requirements

```
numpy
torch
moviepy
pygame
ple
```

## Description

In this project, we will attempt to implement the algorithm developed by Google DeepMind's [Hado van Hasselt](https://hadovanhasselt.com/about/), [Arthur Guez](http://www.gatsby.ucl.ac.uk/~aguez/) and [David Silver](http://www0.cs.ucl.ac.uk/staff/d.silver/web/Home.html) in their paper about ["Deep reinforcement learning with double q-learning"](https://arxiv.org/abs/1509.06461). In this paper, authors tackled the overestimation implications of deep q-learning algorithm. In particular, they have presented the existence of DQN (Deep Q-Network) overestimation via some selected games in Atari 2600. To reduce such overestimation problem, this paper introduces **'Double Deep Q-learning Algorithm'** which is presented in tabular settings and could be generalized to work with large-scale function approximation settings. This paper concludes the much better overall performance by presented algorithm.


## Paper Summary

1. Background

    1. **Deep Q Networks**  
    DQN overcomes unstable learning by mainly 4 techniques.

        1. **Experience Replay**: Experience Replay stores experiences including state transition, rewards and actions, which are necessary data to perform Q learning, and makes mini-batches to update neural networks

        2. **Target Network**: In TD (Temporal Difference) error calculation, target function is changed frequently with DNN. Unstable target function makes training difficult. So Target Network technique fixes parameters of target function and replaces them with the latest network every thousand steps.

        3. **Clipping Rewards**: Each game has different score scales. For example, in Pong, players can get `1` point when winning the play. Otherwise, players get `-1` point. However, in Space Invaders, players get 10~30 points when defeating invaders. This difference would make training unstable. Thus Clipping Rewards technique clips scores, which all positive rewards are set `+1` and all negative rewards are set `-1`.

        4. **Skipping frames**: Arcade Learning Environment is capable of rendering `60` images per second. But actually people don't take actions so much in a second. AI doesn’t need to calculate Q values every frame. So Skipping Frames technique is that DQN calculates Q values every `4` frames and use past `4` frames as inputs. This reduces computational cost and gathers more experiences.

    1. **Double Q-learning**: The max operator in standard Q-learning and DQN, uses the same values both to select and to evaluate an action. This makes it more likely to select overestimated values, resulting in over-optimistic value estimates. To prevent this, we can decouple the selection from the evaluation. This is the idea behind Double Q-learning.

2. **Overoptimism** due to estimation errors: Thrun and Schwartz (1993) gave a concrete example in which Q-learning's overestimations even asymptotically lead to suboptimal policies, and showed the overestimations manifest themselves in a small toy problem when using function approximation. Later van Hasselt (2010) argued that noise in the environment can lead to overestimations even when using tabular representation, and proposed Double Q-learning as a solution.

    ![](./fig/overoptimization.png)

3. **Double DQN**: The idea of Double Q-learning is to reduce overestimations by decomposing the max operation in the target into action selection and action evaluation. Although not fully decoupled, the target network in the DQN architecture provides a natural candidate for the second value function, without having to introduce additional networks. The authors therefore propose to evaluate the greedy policy according to the online network, but using the target network to estimate its value. In reference to both Double Q-learning and DQN, the authors refer to the resulting algorithm as Double DQN.

    ![](./fig/ddqn.png)

4. **Empirical Results**
    1. Results on Overoptimism
    2. Quality of the learned policies
    3. Robustness to Human starts


## Project Goal

- **Minimum Goal**: Implement the method proposed in the paper and test it on an Atari game. Eg. verify the algorithm on ping-pong, etc.
- **Maximum Goal**: Implement this algorithm on a new environment (Eg. a new game simulator) and propose certain ad-hoc modifications for achieve better performance.


## Environment configuration

For our project, we are planning to implement the proposed algorithm on ATARI 2600 games which is available at [PyGame Learning Environment](https://pygame-learning-environment.readthedocs.io/en/latest/). Additional resources are also available at Pygame and PyPi community. A particular simulator environment, that our team is interested in - **"Flappy Bird"**, is available [here](https://github.com/ntasfi/PyGame-Learning-Environment).

### Running it on `Google Colaboratory` (Free GPU)

> **Note**: Make sure the GPU is available in the notebook

##### Mounting Google drive on Colab Notebook (Optional)

```
from google.colab import drive
drive.mount('/content/drive/')
```

navigate to the drive

```
cd drive/My\ Drive
```

##### Environment setup

Download the repository

```
!git clone https://github.com/drl-dql/DQN-Flappy-Bird.git
```

Navigate to the directory

```
cd DQN-Flappy-Bird/
```

Configure the notebook:

```
!bash setup.sh
```

##### Training the agent

Navigate to the `src` directory

```
cd src/
```

Execute file `main.py`

```
!python main.py --train=True --video_path=./video_XXX --logs_path=./logs_XXX 
```

## Experiment results

### Agent preview


<table class="tg">
  <tr>
    <td class="tg-c3ow"><img src="./gifs/env_78000.gif"></td>
    <td class="tg-c3ow"><img src="./gifs/env_92000.gif"></td>
    <td class="tg-c3ow"><img src="./gifs/env_100000.gif"></td>
  </tr>
  <tr>
    <td class="tg-c3ow">Best Agent</td>
    <td class="tg-c3ow">Risky Play</td>
    <td class="tg-c3ow">Bad Training</td>
  </tr>
</table>


### Loss and Average Reward

<p align="center">
  <img src="./fig/best.png" />
</p>

<p align="center">
  <img src="./fig/worst.png" />
</p>


## References

[1] Hado van Hasselt, Arthur Guez and David Silver. [*"Deep Reinforcement Learning with Double Q-learning"*](https://arxiv.org/abs/1509.06461)

[2]  K.  Fukushima.Neocognitron:   A  hierarchical  neural  net-work capable of visual pattern recognition.Neural Networks,1:119–130, 1988.

[3]  H. V. Hasselt.  Double q-learning.  InAdvances in Neural In-formation Processing Systems, pages 2613–2621, 2010.

[4]  Y. LeCun,  L. Bottou,  Y. Bengio,  and P. Haffner.   Gradient-based learning applied to document recognition.  InProceed-ings of the IEEE, volume 86, pages 2278–2324, 1998.

[5]  V. Mnih, K. Kavukcuoglu, D. Silver, A. A. Rusu, J. Veness,M. G. Bellemare, A. Graves, M. Riedmiller, A. K. Fidjeland,G. Ostrovski, et al.  Human-level control through deep rein-forcement learning.Nature, 518(7540):529, 2015.

[6]  T. Simonini.  Improvements in deep q learning: Dueling dou-ble dqn, prioritized experience replay, and fixed..., Jul 2018.

[7]  N.Tasfi.Pygamelearningenviron-ment.https://github.com/ntasfi/PyGame-Learning-Environment, 2016.

[8]  H.  van  Hasselt,  A.  Guez,  and  D.  Silver.    Deep  reinforce-ment learning with double q-learning.CoRR, abs/1509.06461,2015.

