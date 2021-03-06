# Exercise 4 - Independent Q-Learning, Joint Action Learning, and WoLF-PHC for discrete multi-agent RL

In this task, you will control a team of two attacking agents in a discretized soccer domain. We move away from full HFO for this task and just use a simple discretized domain. The attacking agents will be controlled using Independent Q-Learning (each agent independently using Q-Learning without sharing information with other agents), Joint Action Learning with opponent modelling (see the readme in the "JointActionLearner" folder for details), and WoLF-PHC as described in [**Bowling and Veloso, 2001**](http://www.cs.cmu.edu/~mmv/papers/01ijcai-mike.pdf). 

Just like in Exercise 2, each position in the gridworld is associated with a probability of scoring a goal. Furthermore, a defending player is positioned in the environment and acts as an obstacle. (The position of the defending player will not change during the course of the episode.) A ball-carrying agent will be punished with a penalty if it moves into a location occupied by a defending player.

However, unlike Exercise 2, the team of attacking agents will only receive a reward of **+1** if one of the members of the team covers the defending player (by moving into the same grid location as the defender) while the other member kicks the ball into the goal. As a result, an optimal policy for this domain should be one where one agent covers the defending player while the other moves to an advantageous location and scores a goal. This requires some coordination between both of the trained agents.

Full codes for the discretized multi-agent RL domain can be found in the `DiscreteMARLUtils` folder. In particular, `DiscreteMARLUtils/Environment.py` contains the implementation of the interface between the HFO domain and your agent controller. You must then implement algorithms that choose actions given a certain state, and pass it through the provided `act` method. The environment will respond by providing your agents with the next state, reward, and episode completion information. 

## Implemented Files (**Contains functions to be implemented**)
1. `IndependentQLearning/IndependentQLearning.py`
2. `JointActionLearner/JointActionLearnerBase.py`
3. `WolfPHCAgent/WolfPHCBase.py`

## Environment Files (**Should not be modified**)
1. `DiscreteMARLUtils/Environment.py`
   - Specifies the environment for Exercise 4. Mostly similar with Exercise 2, just without any usage of HFO APIs. Also check at the reward functions to see how it's calculated.
2. `DiscreteMARLUtils/Agent.py`
   - Specifies the necessary functions that all agents must have.

## State Space
The environment is modelled as a 5x5 grid. The grid cell with `(0,0)` coordinate is located in the top left part of the field. At each timestep, agents will be given a state representation, in the form of a list, which has information on their teammate's location, defending player's location, and their own location on the grid. 

The location of the goal is not modelled inside the grid. Therefore, agents cannot dribble into the goal and must rely on the `KICK` action to score goals. 

## Action Spaces
Agents are equipped with a set of discrete actions. To move to adjacent grids, agents can use the `DRIBBLE_UP`,`DRIBBLE_DOWN`,`DRIBBLE_LEFT`, and `DRIBBLE_RIGHT` actions. Additionally, the `KICK` action enables the agents to shoot the ball towards the goal. 

## Reward Functions
Agents only receive non-zero rewards at the completion of each episode. In this case, a goal while an agent of the team successfully covers the defending player will result in a reward of **+1**. However, if a ball-carrying agent occupies the same grid as the defending player, it will result in a penalty to both controlled agents.

## Environment Dynamics
Environment transitions resulting from the actions are stochastic. For the dribbling actions, there will be a small probability for agents to end up dribbling into an adjacent (but wrong) grid cell. There is also some possibility of agents' kicks going wayward from the goal after executing the `KICK` action. This probability of kicking the ball depends on the location in the grid that the agent executes the `KICK` action from.

## Implementing Your Own Agents
You are required to implement Independent Q-Learning, Joint Action Learning, and WoLF-PHC. To this end, we've provided you with code snippets which indicate the functions that you should implement. To find the skeleton codes for each algorithm, open the `IndependentQLearning`, `JointActionLearner`, and `WoLFPHCAgent` folders respectively. Further instructions for each task can be found in the markdown files inside the aforementioned folders.

## Dependencies
The `DiscreteMARLUtils/Environment.py` depends on the `pygame` package to visualize the environment. Therefore, if you'd like to get a visualization of your agent, you should install `pygame` using the code below. 
```
conda install -c cogsci pygame
```

Nevertheless, the visualization slows down the execution of the code. Hence, it is not recommended to use the visualizations when training the agent.
