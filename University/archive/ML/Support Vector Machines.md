Tries to apply similar principles behind kernel methods but don't requires to compute the matrix k completely. So these techniques are called sparce kernel methods.
### Separable Problems
Start from the idea of solving a binary determination techniques. The model we are working is the linear separation model seen in perceptron without the explicit mapping. 

![](https://i.imgur.com/GexvQsU.png)

$y(.)=\mathbf w^T\phi(\mathbf x)+b$
To get the real distance we can take the distance and multiply it by t

![](https://i.imgur.com/sVawSsa.png)

Different linear separation are really equals or there is one that is better than the others(Usually the best is the one that has the maximum distance by the closest point in the two separated spaces).

![](https://i.imgur.com/aemd2VQ.png)

I want to find the one that is maximising the margin. In reality the margin needs to be symmetric in respect to the positive and negative points. There is a bit of ambiguity in how the position vector and the weight b are defining the solution.
The first part of the solution is to introduce Canonical Hyperplanes(Linear separator):

![](https://i.imgur.com/pksznHX.png)

We want to be sure that the numerator of the distant equation computed in the two closest points is equal to 1/-1. Requiring this I am forcing the problem to consider only one solution. So the only thing affecting the margin the only thing I can focus on is the norm of the weight vector needing to minimise it(constrain optimisation problem). 
We want to solve these type of problem to maintain the kernel trick is relying to the technique called Lagrange Multiplier:

![](https://i.imgur.com/kieuask.png)

The problem is translated in a new one where the variable $\alpha$ is non negative and we want to maximise the new function in respect to $\alpha$ and we want to mimize in respect to the original variable w.

![](https://i.imgur.com/pTJKpIX.png)

If the points aren't sit on the margins the parenthesis part is positive and so L contains something positive and something negative and then $\alpha$ needs to be 0. 

![](https://i.imgur.com/zKRpgOz.png)

This because our problem is still expensive and it depends also on the dimension of the space.
The benefit is that I have a free choice on the kernel function permitting to make complex problem linearly separable. 
### Non-separable Problems
In some case it might not be convenient to find a linear separator as the model becomes complex in regards to the bias-variance separators. The non separable problems are problems resolved by a simpler model regarding bias- covarinace. In these problems i introduce some variables called Slack variables representing the error I am committing. This variable will be zero if they are on/outside the margins but it will positive if the point is on the positive side and it is inside the margin.

![](https://i.imgur.com/imWqz9d.png)

So the problem settings become: 

![](https://i.imgur.com/sCAlzHX.png)

If we increase C the model becomes more complex as every mistake it is expensive. 

![](https://i.imgur.com/VlUlVDv.png)

The idea is that the points outside the margins are going to be considered as zero not counting in the determination. The points on the margins/inside the margins will be counted in the solution and will divide in on margin and inside the margin. 
One of the problem is that the optimal choice of C is very critical. For this reason there is an effort for searching an alternative settings where we don't search C:

![](https://i.imgur.com/GruvYNI.png)

The cost of the solution cannot go below v(If I have a lot of Margin Error I also have a lot of Support Vectors).  
### Training SVM in Practice
How can I train in practice? The sparcity of the machine is sparcity of the solution. The sparcity property is exploited after we found the solution and not resolve the computational cost of the computation.
#### Chunking

![](https://i.imgur.com/EcRlsk1.png)

Instead of solving a large optimization problem I resolve iteratively small optimization problem. I start with solving my working set and use training to find some support vector. Then I apply the model learned on the entire dataset finding the Worst set(worst point in the original data, larger slack variable, its size is up to equally the working set). I then use this set and the previous Support vector to find the new dataset. One problem is that the dimension of the vector is not kept in check and it can increase indefinitely per each iteration.
#### Osuna’s Method

![](https://i.imgur.com/qHwzfcG.png)

Similar put solve the problem of the size of the working set. It don't identify the Worst set but identifies only a small part of the dataset and substitute them in the working set. 
#### Sequential Minimal Optimization

![](https://i.imgur.com/bZWwxSX.png)

The idea is to sequentially solve simple problem(I consider two sample at once). I can compute analytically the solution for these two points and use the Lagrange computation to update the Lagrange multiplier for the points in the dataset.
### Multi-class SVM
Difficult to transfer the bidimensional SVM to multiclass problem so we use the previous shceme:

![](https://i.imgur.com/QeRPOfY.png)

Solve a multiclass problem training several binary classification problems

![](https://i.imgur.com/ixQKsQF.png)

You need way more model so if we have 3 is 3 but with 5 we need 10 model but now I train the model to discriminate between the classes so I train only on a part of the data.
In general I have some tradeoff as it requires more models but these models are less expensive. The test could be expensive as I am testing on a large number of models.

![](https://i.imgur.com/niQlR2a.png)

The measure problem is that this model have critical case concentrated on the initial part of the tree. But at the end there is no ambiguity and it retains the benefit to train small data on small portion of data but at test time you need to find the single path and the number of test is linear to the number of classes.
### SVM for Regression

![](https://i.imgur.com/XK1E8x8.png)

![](https://i.imgur.com/NXclfR0.png)

It not include a linear separator, can be found in ML libraries. The idea is I have my model and have a regression line and use it as my thresholds on the maximum  error value. The points that are Support vectors now are the points that are on the margins. It is happening not in the input space. You are applying the kernel trick and working in the hyperspace and will have a linear model. The model won't be a parametric model but the parameters of the model depends on the support vectors found during the calculations.