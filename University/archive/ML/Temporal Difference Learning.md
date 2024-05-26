Last family of algorithm to solve policy evaluations and policy improvement. We are in a similar case than Monte Carlo. Differently from Dynamic Programming we assume that we learn from experience. It tries to solve the needing of observing entire episodes. To compute the empirical averages we need a series of returns and each reach the end solution(two drawbacks: can be applied only to episodic task, if I am not in terminal state and cannot say that I can return and even if I am in an episodic situation finding the way of return ca be very difficult).

![](https://i.imgur.com/gZiHr0e.png)

Windy table where the goal is to reach G in the least number of steps. The wind value represent how much I will be moved up in the environment in that column. With Monte Carlo at the start I would use a random policy but that would imply that I cannot reach G denying the learning process. With Monte Carlo the learning at the state will be very bad. 
### Temporal-Difference Policy Evaluation – TD(0)
Combine the DP idea that you divide the problem in immediate reward and the discounted value function in another state (bootstrapping)

![](https://i.imgur.com/orZtKW5.png)

![](https://i.imgur.com/XXW7fSU.png)

We use a sample average on the return but in this case G is not a complete return as it is decomposed in immediate return and discounted value.
TD can learn before knowing the final outcome
- TD can learn after every step
- MC must wait until end of episode before return is known
TD can learn without the final outcome
- TD can learn from incomplete sequences
- MC can only learn form complete sequences
TD works in continuing (non–terminating) environments MC only works for episodic (terminating) environments

![](https://i.imgur.com/i7DDBix.png)

Bootstrapping: update involves an estimate
- MC does not bootstrap(doesn't require the decomposition of the value function, not using Bellman equation, works on non Markovian problems)
- DP bootstraps
- TD bootstraps(When I train the algorithm my knowledge of my current value function I decompose it in two parts: immediate rewards and discounted value functions)
Sampling: update does not involve an expected value
- MC samples
- DP does not sample(Need to know the 1-step ahead procedure)
- TD samples
### SARSA
It is the equivalent of Monte Carlo policy iteration in Temporal Difference Learning.

![](https://i.imgur.com/Ua6ExSj.png)

The idea is to leverage the idea of decomposing the expected return as immediate rewards plus discounted value function in the next time value. I take in consideration current value and the TD error(target minus current Q). SARSA derives from the elements in the update rule

![](https://i.imgur.com/panZy80.png)

SARSA is an on policy algorithm(the policy that I'm using to generate data is the same that I am trying to learn) and the policy is an $\epsilon$-greedy policy(still a good policy to resolve the problem with a non policy approach). 
### Q-Learning

![](https://i.imgur.com/0xFC1VQ.png)

Use Bellman Optimality Equation to create the policy. The next action will depend on the policy I am using to explore the problem. 

![](https://i.imgur.com/a3it9UB.png)

The update rule now the best action according to my knowledge in the next state.
The exploration is now free from the goodness of my results as they are independent but still lead to an optimal policy. Remind to not use random policy for exploration, try to make it limited and gives priority to the most promising moves.
SARSA is better than Q-Learning when there are problems where the movement is equal in each part of the track than a limited patch where the movement is really penalised.
SARSA might take into account the worst case scenario in its analysis.
### Eligibility traces
Instead of choosing TD(0) or Monte Carl I try to compute all the possible definition of target and average them. In practice by introducing the eligibility traces parameter that is waiting all the observed return while I am learning. 