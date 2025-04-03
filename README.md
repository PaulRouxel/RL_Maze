# Reinforcement Learning from scratch
Reinforcement Learning applied to a grid, that could be turn into a maze by adding obstacles and checkpoints. Using the value iteration algorithm from scratch.

The idea is to demonstrate the importance of the different rewards to solve the maze (or just going from the start to the end).

# Logic

The grid is defined by its width ($$x1$$) and its length ($$x2$$), such as $$(x1, x2) \in [|2;20|]^2$$.   
  
Then we can choose how much obstacles we want in the grid (dead cells), such as: $$obstacles < (x1 * x2) - (x1 + x2) +1$$ in theory.  
  
For each obstacles created, a BFS is applied to check if the end is reachable from the start. However, since the obstacles generation is step by step, if we generate the maximum obstacles number and the obstacles make a turn somewhere it will be impossible to create a reachable maze. Make sure to add a margin to the maximum obstacles to avoid infinite loop while the BFS is searching a way.  

Then we can add checkpoints in the grid, which will give some bonus by stepping on them. You can add them up to: $$checkpoints = (x1 * x2) -2$$.

Our robot is made to go north, south, east and west to the next cell.

The algorithm stops when the sum of the state-values is below the corresponding threshold or when the number of iteration is above the corresponding limit.

# Mathematic

Here using the value iteration algorithm (on-policy) with Bellman's equation for the state-value function.

Each cell (state) has a modifiable reward such as:

$$R(s) =
\begin{cases} 
1000 & \text{if } s \text{ is the end state} \\
0.01 & \text{if } s \text{ is a checkpoint state} \\
-0.02 & \text{otherwise}
\end{cases}$$


The state-value of each state is modified every iteration until convergence, the policy is then modified for each state to get to the next state with the maximum state-value. Such as :

$$V(s) =  R(s) + \gamma max(\sum P_s(s')  V(s'))$$

with $$s'$$ being the next state.

with $$\gamma$$ being the discount factor, usually set to $$\gamma = 0.9$$.

with $$P_s (s')$$ being the probability to get the next state, set to $$0.8$$ to get to next state with the highest state-value, when going to this next state there is a $$0.1$$ probability to go on the right state and $$0.1$$ to go on the left state.

The policy is then set to go from the actual state to the next state with the highest state-value such as:

$$\pi (s) = argmax( \sum P_s(s')V(s'))$$

