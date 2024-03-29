### Introduction to Kernel Methods
Often we want to capture nonlinear patterns in the data
- Nonlinear Regression: input-output relationship may not be linear
- Nonlinear Classification: Classes may not be separable by a linear boundary
Linear models are not just rich enough, it is not feasible for the complexity of the model. Kernel methods allow to make linear models work in nonlinear settings by mapping data to higher dimensions where it exhibits linear patterns.
We want to map a non linear input space in a linearly separable feature space

![](https://i.imgur.com/vauQSGa.png)

And then we want to find a linear separator. This come with a price that is the increasing the number of features effectively needed to solve our problem.
This take in some problem:
- Curse of dimensionality! The number of features grows significantly with the number of input variable and the mapping become quickly computationally unfeasible
Kernels methods deal with this issue: they don’t require to explicitly compute the feature mapping, they are expensive but computationally feasible. The idea of kernel methods is finding a way to solve the problem in a way with I'm not affected too much from the dimensionality of the features space.
The first ingredient is the kernel function: $k(x,x')=\phi(\mathbf x)^T\phi(\mathbf x')$ . The idea is to use this function as I know it is able to catch similarities between the samples. This function can help me building a model not affected by the dimensionality of the features space as there are many spaces where I can find a more cheaper representation than computing the dot product between features vector in the features space. The kernel methods tries to revisit all the techniques seen so far and finding a way to only use the kernel function.
#### Kernel trick
It is possible to rework the representation of linear models to replace all the 
terms that involve $\phi(\mathbf x)$ with other terms that involve only $k(\mathbf x, .)$. 
In other words, the output of linear model can be computed only on the basis of the similarities between data samples (computed with the kernel function)
### Kernel Ridge Regression

![](https://i.imgur.com/jfTUeG2.png)

With this we can easily find the relation $\bar w=\phi^T\mathbf a$. Now we see how I can reapply the new variable in the solution. 

![](https://i.imgur.com/UQDn7Ne.png)

The matrix $\Phi$ is composed of all the vector samples and the matrix dimensionality is NxM. $\Phi^T$ each column is a feature vector for a sample so the dimensionality is MxN. So the result will be a NxN matrix called K. K will contains all the kernel function of the various sample vector.   

![](https://i.imgur.com/w48ChEx.png)

![](https://i.imgur.com/yBfymtA.png)

We replace w with its new value and then resolve. We obtain a prediction computed as the linear combination of the target values of the samples in the training set. I don't need in any point to compute the feature mapping. I am multiplying a vector by something that would be a Nx1 and this is the target vector. Linear combination of the target value in my dataset. 

![](https://i.imgur.com/pZzDgTR.png)

In the original representation we don't need to know how to compute the real space into a feature space but we only need the kernel function. This will open the possibilities to do something more interesting. 
### Kernel Function Design
We defined the kernel function as the dot product in the feature space:
$$k(x,x')=\phi(\mathbf x)^T\phi(\mathbf x')=\sum_{i=1}^M\phi_i(\mathbf x)\phi(\mathbf x')$$ ![](https://i.imgur.com/y6fvWNb.png)

But the typical way to design kernel. 3 way:
- You know the feature mapping but is cheaper to compute the kernel function
- Design the kernel directly without any knowledge of the corresponding feature space. You need to guarantee that the resulting function is valid(It need to exist a feature space with this kernel function)
- Set of rules with which we can compute a kernel function starting from another one
In general, we must make sure that the designed kernel functions is valid, that is it correspond to a scalar product into any feature space. The Gram matrix has to be positive semi-definite(For every possible non real zero vector the property holds).

![](https://i.imgur.com/BorQxGJ.png)

#### Rules to design valid kernels
Given valid kernels $k_1(x,x')$ and $k_2(x,x')$ the following rules can be applied to design a new valid kernel:
1. k(x, x')=c$k_1$(x.x') where c > 0 is a constant
2. k(x, x')=f(x)$k_1$(x, x')f(x') where f(.) is any function
3. k(x, x')=q($k_1$(x,x')) where q(.) is a polynomial with non-negative coefficients
4. k(x, x')=exp($k_1$(x, x'))
5. k(x, x')=$k_1$(x, x')+ $k_2$(x, x')
6. k(x, x')=$k_1$(x, x')$k_2$(x, x')
7. k(x, x')=$k_3(\phi(\mathbf x), \phi(\mathbf x'))$ where 
#### Gaussian Kernel

![](https://i.imgur.com/ycEBn5h.png)

Its feature space has infinite dimension. It is the most used kernel used among kernel methods techniques. It permit to learn an arbitrary complex problem.

#### Kernels for Symbolic Data

![](https://i.imgur.com/RS2Bzap.png)

### Kernel Regression
#### k-NN Regression
The k Nearest Neighbour (k-NN) can be applied to solve a regression problem by averaging the K nearest samples in the training data:
$$\hat f(\mathbf x)= \frac 1 k \sum _{\mathbf x_i \in N_k(\mathbf x)}t_i$$
![300](https://i.imgur.com/u3IUI69.png)

#### Nadaraya-Watson Model
In k-NN regression the model output is generally very noisy due to the discontinuity of neighborhood averages. Nadaraya-Watson model (or kernel regression) deal with this issue by using kernel function to compute a weighted average of samples:
$$\hat f(\mathbf x)= \frac{\sum_{i=1}^N k(\mathbf x, \mathbf x_i)t_i}{\sum_{i=1}^N k(\mathbf x, \mathbf x_i)}$$
Typical choices for kernels are: Epanechnikov Kernel (bounded support) and Gaussian Kernel (infinite support)
### Gaussian Processes
Find a way to generalise Bayesian linear regression to use kernel methods.

![](https://i.imgur.com/DewkN0z.png)

In general, a Gaussian Process is defined as a distribution probability over a 
function y(x) such that the set of values y($x_i$) − for an arbitrary {$x_i$} − jointly have 
a Gaussian Distribution. In our specific case $p(\cal y)=\cal{N}(\cal {y} |0,\textbf K)$   where $K_{nm}=k(\mathbf x_n,\mathbf x_m)= \tau^2\phi(\mathbf x_n)^T\phi(\mathbf x_m)$. This gives a probabilistic interpretation of the kernel function as: $k(\mathbf x_n, \mathbf x_m)= \mathbb E[y(\mathbf x_n), y(\mathbf x_n)]$. The $\tau^2$ constant is not giving a problem as in the calculus it is irrelevant. This gives a probabilistic interpretation to the model. The Gram matrix is the covariance of the y variable. This implies that the kernel function can give me the correlation between two elements of the space. Different kernel will give different representation of the correlation between two points of the space.
The typical choice of kernel are the Gaussian Kernel or the Exponential Kernel. Gaussian Kernel usually used to resolve the problem of regress function and if you need to find a smooth function if you need also noise variation in the space you should use Exponential Kernel.
