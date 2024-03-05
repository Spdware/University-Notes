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