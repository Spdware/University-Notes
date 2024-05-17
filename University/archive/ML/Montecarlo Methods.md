Solves the problem of not knowing one step dynamic of my problem.
Monte Carlo methods can be used in two ways:
- model-free: no model necessary and still attains optimality
- simulated: needs only a simulation, not a full model
Monte Carlo methods learn from complete sample returns
Only defined for episodic tasks
I need several path of solution but each of them need to be complete(cannot learn from partial solution of the Markov Decision Problem)
### Monte Carlo Policy Evaluation
- Goal: learn $V_π(s)$
- Given: some number of episodes under π which contain s
- Idea: Average returns observed after visits to s

![](https://i.imgur.com/sfCqrcx.png)

Every-Visit MC: average returns for every time s is visited in an episode
First-visit MC: average returns only for first time s is visited in an episode
Both converge asymptotically
We can observe the value returned several time at various observations. Just compute the value function as the average of G. 

![](https://i.imgur.com/6Gm6ZJK.png)

To keep it easy we can use an incremental average:

![](https://i.imgur.com/mllnBnG.png)

If you expect the problem to change in time this will keep updates of the various values in time
Montecarlo is useful when it is easy to create an infinite number of episodes to train your algorithm.
### Policy Iteration

The idea is that the things are less straightforward than expected. 
The idea is that the policy improvement is: $\pi'(s)={argmax\atop{a}}\{r(s,a)+\gamma\sum_{s'\in \cal S}p(s'|s,a)V_\pi(s')\}$
It is impossible in Montecarlo so I apply the action by the function not requiring information

![](https://i.imgur.com/si4ovfx.png)

The price for doing that I need to compute the empirical average of the pair states(all possible sequence starting from state s and applying action a)

![](https://i.imgur.com/FnbUTHV.png)

![](https://i.imgur.com/BDacL6r.png)

### $\epsilon$-Soft Monte Carlo Policy Iteration
An  $\epsilon$-Soft Monte Carlo Policy is a policy that guarantee that all the action in the policy have a small probability to be chosen. 

![](https://i.imgur.com/1YpxG2L.png)

The larger the $\epsilon$ the larger the exploration
The $\epsilon$ greedy policy guarantee at least the probability and the rest of probability is dedicated to the best action in accordance to the model in use(greedy as it use almost all budget for the best action). The idea is that in order to guarantee some level of exploration we shift from an idea of learning a deterministic policy to learn a probabilistic policy. This guarantee some level of exploration. With $\epsilon$ =0 the policy is completely greedy.

![](https://i.imgur.com/NXxB3bb.png)

By doing the policy improvement I will obtain a policy that is at least equal to my policy, usually it is better. 

![](https://i.imgur.com/qSokRLv.png)

### Off-policy Learning
**On-Policy Learning**
- The agent learns the value functions of the same policy used to select actions
- Exploration/Exploitation Dilemma: we cannot easily learn an optimal deterministic policy
**Off-Policy Learning**
- The agent select action using a behaviour policy b(a|s), used to generate data
- These data is used to learn the value functions of a different target policy $\pi$(a|s), policy I want to evaluate and improve
- While b(a|s) can be an explorative policy, the agent can learn an optimal deterministic policy $\pi^*(a|s)$

![](https://i.imgur.com/GUSUe4t.png)

We need to use the technique called **Importancce Sampling**. One of its limitations is that no matter the technique used is always impossible to learn a policy $\pi$ that choose an action in the state s if this cannot happen in b. Importante sampling allow to estimate expectation of a distribution different w.r.t. the distribution used to draw the samples.

![](https://i.imgur.com/BSVSfAK.png)

![](https://i.imgur.com/dMvelFu.png)

![](https://i.imgur.com/G08Q71z.png)

We also want to know the transition probability(we cannot in Monte Carlo) so we need to compute a fraction  so using a new policy b the transition probability is up and down the fraction and so I only need to know the two policies.

![](https://i.imgur.com/NpIeEbO.png)

The correct algorithm is that you apply exactly the equation, so we observe all the return from state s and divide it for how many time I visited state s in my data(Unbiased estimator for the value function in s). An alternative solution is to compute a biased estimator that compute the sum of probability of all the trajectory of the state s. This estimator is biased and the bias will converge to zero, but on the other hand the estimator will have a low variance and suffer less from problem when starting with episodes with very few data. 

![](https://i.imgur.com/SQjtvjD.png)

Completely decouple the policy that generate data from the policy that need to be evaluate.