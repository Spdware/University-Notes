These are algorithm used to solve a problem where I have to take a decision in an uncertain scenario(Ex: select/advice a movie).

![](https://i.imgur.com/2hDIzkm.png)

It is like a Markov Decision Problem with a single state and with not knowing the result of the choice. The name derive from the fact that the actions taken are named bandits.
### Action Values
The value of each action is defined as the expected reward:$q^*(a)\doteq \Bbb  E[A_t=a]=\sum p(r|a)r,\forall a\in{1,..,k}$
The goal of the agent is to maximize the expected reward: $argmax_a q^*(a)$
Indicator function: a function that is equal to 1 if the value is a specified one 0 otherwise
The more sample I go through the more the empirical average is going to be close to the real value.

![](https://i.imgur.com/CaP7X36.png)

### Incremental update and non-stationary problems

![](https://i.imgur.com/sCDutS5.png)

![](https://i.imgur.com/O9pN3i5.png)

I can also comprehend updates in the parameter of my evaluated actions that I am looking at with the addition of a parameter that will indicates how much I have to look at the past samples.
### Epsilon-Greedy Action Selection
![](https://i.imgur.com/RXDInCq.png)

I create a budget for exploration/exploitation and see if I am in a case where I am exploring or exploiting. It is called greedy as I am taking a choice on the base of what I think is the optimal choice in base of the data I have without looking at an actual optimization process.

![](https://i.imgur.com/w5ti8Z5.png)

### Optmistic Initial Values
I tweak my initial value in the function.
Limitations of optimistic initial values
- Optimistic initial values only drive early exploration
- They are not well-suited for non-stationary problems
- We may not know what othe optimistic initial value should be
### UCB Action Selection
Can I behave in a smart way and select uniformly. Assume I get a fair evaluation so it is reasonable that I should explore some actions that I am really sure for my action in several time.

![](https://i.imgur.com/XFKN4Pf.png)

I want to narrow down the confidential interval only when I am certain it will be the best move(Hit the one with the greater upperbound).

![](https://i.imgur.com/xxeV6Ap.png)

