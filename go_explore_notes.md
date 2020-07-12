# First return then explore (Go-Explore) - Notes

### High Level Overview

Problems this paper aims to solve:
* sparse rewards
  * detachment - The agent fails to find interesting places to explore from (eg. stuck in one room of a house)
  * derailment - The agent fails to find previous states

#### Cells and States

Go-Explore solves the above problems by archiving states in *cells* so the algorithm can return to interesting states it's been to before to further explore from there, hence solving the detachment and derailment problem.

![cell_state](https://i.imgur.com/ygnEQZ1.png)

![exploration_phase](https://i.imgur.com/3AVzFhb.png)

The author's note that one benefit of saving state cells in a enviornment like Atari benchmark suite is that you don't need a policy to return to states. You can just pick a previous cell and reset the simulator to that state and take random actions to explore from there. This is nice if you have access to the underlying infra of the simulator but is not very useful when you don't have a lot of control over the enviornment eg most closed source games. For this reason they also introduce a policy for this scenario later in the paper.

## Policy Based Go-Explore

![image](https://i.imgur.com/OkHCJq5.png)

The authors select the next cell based on the following metric where $C_{steps}$ is the number of steps the agent spent in the cell.
$$W= \frac{1}{0.5C_{steps}+1}$$

## Robustification

Robustification is basically taking a trajectory (series of steps) and doing imitation learning. Eg if you start at the end of the trajectory you walk back a few steps and have the agent learn to reach the goal and repeat this process until your agent is able to reach the goal. This helps the agent learn in stochastic enviornments. 

## Bonus

"In terms of infrastructure, each exploration phase run was performed on a single worker machine
equipped with 44 CPUs and 96GB of RAM, though memory usage is substantially lower for most games. Each
robustification run was parallelized across 8 worker machines each equipped with 11 CPUs, 24GB of RAM, and 1
GPU. Policy-based Go-Explore was parallelized across 16 worker machines each equipped with 11 CPUs, 10GB of
RAM, and 1 GPU."
