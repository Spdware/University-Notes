Learn from a dataset D an approximation of function f(x) that maps input x to a discrete class $C_k$ (with k=1,...,k)
$$D=\{ \langle x,C_k\rangle\}\Rightarrow C_k=f(x)$$
The target as to be encoded in a number, we need to model f, we need to encode $C_k$ and we need to optimise the approximation.
I can find my target wit or without probabilistic approaches. 

![400](https://i.imgur.com/GEnPzwC.png)

The second probabilistic approach is really difficult but if you can resolve it you can also generate new data points.
### Discriminant Function
Function that map the input in one of the classes.
I cannot use a plain linear function. So we will use a generalized linear models:
$$f(\mathbf x, \mathbf w)=f(\omega_0+\sum_{j=1}^{D-1}\omega_jx_j)=f(\mathbf x^T\mathbf w+\omega_0)$$
- f(.) is not linear in w and it allows me to maps the inputs in the way I want to encode the class.
- f(.) partitions the input space into decision regions whose boundaries are called decision boundaries or decision surfaces
- these decision surfaces are linear function of x and w as they correspond to $\mathbf x^T\mathbf w+\omega_0=const$ 
- generalized linear models are more complex to use with respect to linear models(both from a computational and analytical perspective)

![](https://i.imgur.com/QTKJQkj.png)

This function is basically a linear function that gives a result regarding the input. If we call the argument of the model y(), by definition y()= 0 will be the equation of my decision boundary. It is a linear separator.  
How can I use this function in a multiple classes problem?
1. One-versus-the-rest approach
2. One-versus-one 

![](https://i.imgur.com/NTZ5oYB.png)

### Linear Basis Function Models
So far we considered models that work in the input space (i.e., with x). However, we can still extend models by using a fixed set of basis function $\phi(x)$ . Basically, we apply a non-linear transformation to map the input space into a feature space. As a result, decision boundaries that are linear in the feature space would correspond to nonlinear boundaries in the input space.  This allows to apply linear classification models also to problem where samples are not linearly separable.
Some useful resolution can come from:
- Perceptron is a linear discriminant model proposed by Rosenblatt in 1958 
along with a sequential learning algorithm. Perceptron is devised for a two-class problem, where classes encoding is {-1,1}
$$f(\mathbf x, \mathbf w)=\begin{cases}
	\text{+1 if }\mathbf w^T\phi(\mathbf x)\ge0\\ 
    -1\text{ otherwise}
\end{cases}$$
The idea is to optimise w to find a model that is perfect.
Use as the loss the total sum of the misses points. The entire sum is going to be a positive matrix that I want to minimise/optimise. 

![](https://i.imgur.com/nld0URl.png)

I repeat until I don't have misplaced points anymore.
A property of this algorithm is that if there is a linear separation boundary in your data this algorithm will find it eventually(no guarantee on the number of steps needed to converge). This algorithm find one linear boundary separation depending on the initialization of the algorithm and the order of the scanning during the update. 
Every update I am reducing the loss. 
### Probabilistic Discriminative Approaches
The new approach is a deterministic model. The idea is to model the problem with a probabilistic view, the probability of a point to be positive/negative. We have a probability to have a parametrized model to have a loss function to identify our problem.
It is not a regression model.

![](https://i.imgur.com/d9Xn2GP.png)

We don't really use the logistic of the input but we are using a linear function as the input of the logistic model. I can quantify the probability. 
I can fit to the data with a Maximum Likelihood and we compute the probability of seeing the data in our set. We then maximise the probability in regards of the weight vector. We just need to focus on a single point and then using this probability to compute on the other data. 

![](https://i.imgur.com/IQ0tl4T.png)

It works for a binary probabilistic problem but can be easily extended to a k-class problem as you only have to compute the probability of be a problem of the k-class.
I learn a weight vector for every class. i don't have two possible probability but I have to choose one on the key.

![](https://i.imgur.com/oQvWc06.png)

The two techniques are exactly the same, the perceptron is a logistic model where we use the logistic function instead of the step function. 