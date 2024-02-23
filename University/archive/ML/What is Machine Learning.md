Machine Learning implies a computer program which is learning from experience __E__ with respect to some class of task __T__ and performance measure __P__ improving its behaviour from experience __E__
The knowledge comes from Experience(relevant only if you expect things to go similar to the past) and Induction.
Machine Learning cannot create information but it can extract information from data

![250](https://i.imgur.com/LyngM1N.png)

We need computers to make informed decision on new, unseen data. Often is too difficult to design a set of meaningful rules. ML allows to automatically extract relevant information from previous data and exploit it on new one. We can also use ML to get computers to program themselves(automating automation). In this case writing code is the bottleneck, we should let the data do the work instead.
# Machine Learning Paradigms

![350](https://i.imgur.com/iRaLgyt.png)

### Supervised Learning
Learn from data a model that maps known inputs to known outputs.
I use two different modes:
- Classification: Learn the differences
- Regression: Find a model predicting output from the input
- Probability Estimation

![300](https://i.imgur.com/onHDP1J.png)

Training set: $$\text{D }=\text{ \{\langle x,t\rangle\} }\Rightarrow\text{t }=f(x)$$
Use various techniques as Linear Models, Artificial Neural Networks, Support Vector Machines, Decision trees, etc...
### Unsupervised Learning
Learn some previously unknown patterns and efficient data representation from the input

![300](https://i.imgur.com/vaGb3Hn.png)

Training set:$$D=\{x\}\Rightarrow f(x)$$
Tasks:
- Representation Learning
- Dimensionality Reduction: represent data points in a more compact way
- Clustering: group data by a characteristic
### Reinforcement Learning
Learns the optimal policy with only a positive/negative feedback.
Training set:
$$\text{D }=\{\langle x,u,x',r\rangle\}\Rightarrow\pi^*(x)=arg\text{max}_u\{Q^*(x,u)\}$$
