It aims at studying the general laws of inductive learning, by modeling:
- Complexity of hypothesis space
- Bound on training samples
- Bound on accuracy
- Probability of succesfull learning
- ...
This allows to answer to questions like:
- How many training samples do a learner need to converge (with some probability) to a successful (with some minimum accuracy) hypothesis? 
- How many training samples will be misclassified by the learner before converging to a succesful hypothesis?
We expect something that confirm what we obtain from machine learning library results
The problem is to define the function to map a given input in a target and find the function and the hypotesis space that correspond to the examples. 

![](https://i.imgur.com/GioxGzA.png)

A learner (_L_) wants to learn a concept(c) that maps the data in the input space ($X$) to a target (t)
Let assume that L found an hypothesis ℎ∗ with no errors on the training data
How many training samples of $X$ are necessary to be sure that $L$ actually learned the true concept i.e. $h^*\equiv c$ 
### No Free Lunch Theorems
On average every ML model will behave like a random guess. We are focusing on classification and binary classification.
Let $ACC_G(L)$  be the **generalization accuracy** of learner $L$  i.e., the accuracy of L on 
samples that are not in the training set
Let ℱ be the set of all the possible concepts 
For any learner L and any possible training set:
$$\frac{1}{\cal F}=\sum_{\cal F}ACC_G(L)=\frac{1}{2}$$
- Proof Sketch: for every concept $f$ where $ACC_G(L)=0.5+\delta$ , exists a concept $f$' where $ACC_G(L)=0.5 + \delta:\forall \mathbf x\in \cal D, f'(\mathbf x);\forall x\notin \cal D, f'(\mathbf x)\neq f(\mathbf x)$
- Corollary: for any two learners $L_1$ and $L_2$, if $\exists f$ where $ACC_G(L_1)>ACC_G(L_2)$ then $\exists f'$ where $ACC_G(L_2)>ACC_G(L_1)$

This accomplish two factors:
- There is not a ML algorithm that is superior to the other(The solution can be derived after the definition of the problem)
- We always operate under assumptions. We assume that the hypotesis space contains a good approsimation of the function to derive. We use some techniques to reduce the complexity of the model(We want a model not too complex, no overfitting of the data)
### Probably Learning an Approximately Correct Hypothesis
![](https://i.imgur.com/w8eBUjl.png)

Binary classification is the more convenient setting to compute this analysis. 

![](https://i.imgur.com/E29ESEd.png)

We want to compute on all possible data and so we calculate the true error
The error is also defined by the region covered only y the defined space or the concept(This definition doesn't comprehend the fact that the true error cannot be zero, this lead to not having idea of the sign of my distribution, I can make mistake on a region not much populated or density populated)
We want to find a limit to the true error: 
- $error_{true}$ is the probability of making a mistake on a sample
- we can compute $error_{\cal D}$ that is the average error probability on $\cal D$
- assuming a Bernoulli distribution for the error probability, the 95% CI is:
$$error_{true}(h)=error_{\cal D}(h)\pm1.96\sqrt{\frac{error_{\cal D}(h)(1-error_{\cal D}(h))}{n}}$$ 
This isn't correct because this is not a true confident interval but not work for us as our training error are completely independent from the actual data used. No reason to suppose our model will perform the same on unseen data and training data. But we trained our model specifically to behave correctly on unseen data. 
#### Version Space
A hypothesis h is consistent with a training dataset $\cal D$ of the concept c if and only if h(x)=c(x) for each training sample in $\cal D$(We have zero training error)
$$Consistent(h, \cal D)\stackrel{def}{=}\forall \ ⟨x,c(x) ⟩\in \cal D, h(x)=c(x) $$
The version space $VS_{H,D}$ with respect to hypothesis space H and labelled dataset $\cal D$, is the subset of hypotheses in H consistent with $\cal D$
$$VS_{h,\cal D}\stackrel{def}{=}\{h\in H |Consistent(h,\cal D)\}$$
From now on, we consider only **consistent learners**, that always output a 
**consistent hypothesis**, i.e., an hypothesis in $VS_{H,\cal D}$ assuming is not empty

![](https://i.imgur.com/PyhuITH.png)

In practice:
- Let say that $\delta$ is the probability to have $error_{true} > \varepsilon$ for a consistent hypothesis:
	$$|H|e^{-\varepsilon N}\le\delta$$
 - We can bound N after setting $\varepsilon$ and $\delta$:
	$$N\ge\frac{1}{\varepsilon}(ln|H|+ln(\frac{1}{\delta}))$$
-  We can bound $\varepsilon$ after setting N and $\delta$:
$$\varepsilon \ge \frac{1}{N}(ln|H|+ln(\frac{1}{\delta}))$$
#### Probably Learning an Approximately Correct Hypothesis
![](https://i.imgur.com/A7JqAN6.png)

![](https://i.imgur.com/xCxdEEb.png)

Proof
![](https://i.imgur.com/cwur0SF.png)

![](https://i.imgur.com/sQ6Dba4.png)

error = noise+$bias^2$+ variance (The bias influence the most the error)
#### PAC-Learning with Infinite Hypotheses Spaces
![](https://i.imgur.com/3kaRiss.png)

![](https://i.imgur.com/slfx5ev.png)


