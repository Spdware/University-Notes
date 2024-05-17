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
- TD bootstraps
Sampling: update does not involve an expected value
- MC samples
- DP does not sample
- TD samples
