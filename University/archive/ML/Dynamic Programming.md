First technique to try to resolve the problem of finding the optimal policy with nonlinear equations and not knowing $\pi^*$. 
Dynamic programming is approaching the problem recursively. We will see two algorithm:

![](https://i.imgur.com/5BDnAYQ.png)

### Policy Evaluation
The algorithm is simple: if you apply the Bellman Expectation Equation for all the state of the problem and use a specified verify function. You go through all the states applying the Bellman Expectation Equation using all the previous found values as the value function. 
This lead to get the true value of the policy function

![](https://i.imgur.com/T2pbhXI.png)

![](https://i.imgur.com/pBNd2UX.png)

### Policy Improvement
I can derive the optimal policy computing the optimal policy selecting the action a from the optimal choice. 
In dynamic programming I have to do that on a general function $\pi$ non optimal. 
If I apply this greedy algorithm choosing the action greedly from what I know? The answer is that there are two options:
- $\pi'$ is equal to $\pi$ it means $\pi$ is already the optimal policy $\pi^*$  
- $\pi'$ is as good as $\pi$(easy way to compute the optimal policy) 

![](https://i.imgur.com/04PjOd6.png)

With this theorem I cannot get something worse only something as good or better than what I already have.
### Policy Iteration
Idea is keep iterating the policy evaluation starting from a policy, even random policy. After the first policy evaluation I apply a policy improvement to find a new policy that maximise the initial policy. At this point I repeat these steps until I obtain an optimal policy. At the end you know that you reached $\pi^*$ when the improvement step is not changing the policy(This is by definition). Guaranteed to reach this result but it is not guaranteed the number of steps to reach it.

![](https://i.imgur.com/5mXPk1B.png)

To propagate the information of a policy optimization is slow as it takes at east one iteration to get to propagate the information
### Generalized Policy Iteration
Instead to alternate evaluation and improvement we try to do a small number of policy evaluation and improvement doing partial evaluation and improvement still converging to the optimal policy.
#### Value Iteration
Takes the idea to the extreme combining together the update of the policy evaluation and policy improvement. This is done doing the update with respect to Bellman equation considering the greedy version of the policy computed by the policy improvement.

![](https://i.imgur.com/40ciXDb.png)

![](https://i.imgur.com/cmn4LNe.png)

Even simpler as it takes only one cycle to complete every step.
### Efficiency of DP
The DP implementation can or cannot be useful in programs.
The Asynchronous Dynamic Programming is one idea to have an improvement. The idea is that in a program tries to update from random state to converge to the solution and search one optimal way. One drawback is that you could not find the best optimal way.
It can scale up to few million of states and then start to struggle. If you have more states you can scale the problem using parallel programming.
Linear programming approaches can be also used instead of DP but they do not typically scale well on larger problems.
DP is extremely effective with not gigantic problems , but doesn't scale well on gigantic problems. To solve some problems/limitations of DP we can use two families of solutions: Montecarlo's Families and Bandits