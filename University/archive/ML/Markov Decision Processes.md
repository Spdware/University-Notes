In reinforcement learning I want an evaluation of the answer. I cannot show the answer to my model for the input(difference between supervised learning and reinforcement learning) I can only give a feedback to the model(Good/Bad).
### Agent-Environment Interface
The goal is to model simple problem(in terms of interactions) but also models problems where the model needs to take a sequence of decision to reach our goals. The optimality of the actions depends on the input and we don't have examples on the correct sequences. Each actions/decision taken by the model will have consequences.
**Sequential Decision Making**
In sequential decision making... 
-  we take a sequence of decisions (or actions) to reach the goal
-  the optimal actions are context-dependent
-  we generally do not have examples of correct actions
-  actions may have long-term consequences 
-  short-term consequences of optimal actions might seem negative

![](https://i.imgur.com/cN040lL.png)

The above example take the instance where a fish search for food and gives feedback on the decisions the fish takes in terms of the value above the choice. I want the model with an optimal behaviour even if the surroundings changes. Nobody is telling me the best actions but they give me a numerical feedback. Given a specific input there will be a sequence of feedback and result and the model will use them to evaluate the best sequence of action to take.
#### Agent-Environment Interface
![](https://i.imgur.com/t2tWmfs.png)

### Markov Decision Process
**Markov Decision Process: One-Step Dynamics**
Markov Property: future state (s’) and reward (r) only depend on current state (s) and action (a)(It is not a limiting assumption, it can be seen as a property of state)
In a Markov Decision Process (MDP), the one-step dynamic can be described as: $p(s',r|s,a)$ 
$p: \cal S ×\cal R×\cal S ×\cal A\rightarrow[0,1]$ 
$\sum_{s'\in\cal S}\sum_{r\in \cal R}p(s',r|s,a)=1 \text{ }\forall s \in\cal S,\forall a\in\cal A(s)$ 

![](https://i.imgur.com/Tjrb4bw.png)

When holds the Markov Property and the state and action sets are finite, the problem is a finite Markov Decision Process. To define a finite MDP, you need to define:
- state and action sets
- one-step dynamics: $p(s',r|s,a)=Pr\{S_{t+1}=s',R_{t+1}=r|S_t=s,A_t=a\}$
- we can also derive the next state distribution and expected reward as(they are useful for convenience): 
$$p(s'|s,a)\doteq Pr\{S_{t+1}=s'|S_t=s,A_t=a\}=\sum_{r\in \cal R}p(s',r|s,a)$$
$$r(s,a)\doteq \mathbb E[R_{t+1}|S_t=s,A_t=a]=\sum_{r\in \cal R}r\sum_{s'\in \cal S}p(s',r|s,a)$$
**Return**
Agent should not choose actions on the basis of immediate reward. In fact, long-term consequences are more important than short-term reward. So, we need to take into account the sequence of future reward. We define **return**, $G_t$, as a function of the sequence of future rewards: $G_t\doteq f(R_{t+1} + R_{t+2}+R_{t+3}+...)$
To succeed, the agent will have to maximize the expected return $\mathbb E[G_t]$
I can use as the function f():
- The sum of all the rewards(Total rewards)
- Average of the rewards(similar to the sum)
- Discounted rewards(for rewards very far in the view)
**Episodic Task**
For some problems we have a clear start and a clear end. 
In episodic task the agent-environment interaction naturally breaks into chunks called episodes. 

![](https://i.imgur.com/mBi3371.png)

It is possible to maximize the expected total reward: $\mathbb E[G_t]=\mathbb E[R_{t+1}+R_{t+2}+R_{t+3}+...+R_T]$
**Continuing Tasks**
In continuing task the agent-environment interaction goes on continually and there are no terminal state. The total reward is a sum over an infinite sequence and might not be finite:
$$G_t=R_{t+1}+R_{t+2}+R_{t+3}+...+R_{t+k}+...=\infty$$
To solve this issue we can discount the future rewards by a factor of $\gamma$ ($0<\gamma<1$):
$$G_t\doteq R_{t+1}+R_{t+2}+R_{t+3}+...+R_{t+k}+...<\infty$$
Thus, the expected reward to maximize will be defined as: $\mathbb E[G_t]=\mathbb E[\sum_{k=0}^{\infty}\gamma^k R_{t+k+1}]$ 
**Unify Notation for Returns**

![](https://i.imgur.com/mad5UdQ.png)

**Goal and Reward**
A goal should specify what we want to achieve, not how we want to achieve it
The Reward Hypotesis: That all of what we mean by goals and purposes can be well thought of as maximization of the expected value of the cumulative sum of a received scalar signal (reward).
Examples of reward design:
- goal-reward representation: 1 for goal, 0 otherwise
- action-penalty representation: -1 for not goal, 0 once goal reached
If I am not using a discount factor the action-penalty representation will arrive at the solution faster, otherwise the goal-reward representation can arrive at the result faster for some values of $\gamma$
Challenges to reward hypothesis
- How to represent risk-sensitive behavior?
- How to capture diversity in behavior?
### Policy
A policy, at any given point in time, decides which action the agent selects
A policy fully defines the behavior of an agent 
Policies can be:
- Markovian / Non Markovian(We will focus on Markovian)
- Deterministic / Stochastic
- Stationary / Non Stationary(We will focus on Stationary)
**Deterministic Policy**
In the simplest case the policy can be modeled as a function $(\pi:\cal S \rightarrow \cal A)$: $\pi(s)=a$
 Accordingly, the policy maps each state into an action
This type of policy can be conveniently represented using a table

![](https://i.imgur.com/Mi6xznQ.png)

**Stochastic Policy**
A more general approach is to model policy as function that maps each state 
to a probability distribution over the actions: $\pi(a|s)$
- $\sum_{a\in\cal A(s)}\pi(a|s)=1$
- $\pi(a|s)\ge 0$
A stochastic policy can be used to represent also a deterministic policy
It is not much interesting to search an optimal stochastic policy as from it it can be derive a deterministic policy.
**Markovian/Non Markovian Policy**

![](https://i.imgur.com/GBZ9TGb.png)