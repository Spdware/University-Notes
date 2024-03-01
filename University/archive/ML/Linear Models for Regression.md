# Supervised Learning
We turn now to a discussion of supervised learning, starting with regression. The goal of regression is to predict the value of one or more continuous target variables _t_ given the value of a D-dimensional vector x of input variables. We have already encountered an example of a regression problem when we considered polynomial curve fitting.
The simplest form of linear regression.
models are also linear functions of the input variables. However, we can obtain a much more useful class of functions by taking linear combinations of a fixed set of nonlinear functions of the _input variables_, known as _basis functions_.
More generally, from a probabilistic perspective, we aim to model the predictive distribution _p(t|x)_ because this expresses our uncertainty about the value of _t_ for each value of _x_. From this conditional distribution we can make predictions of _t_, for any new value of _x_, in such a way as to minimize the expected value of a suitably chosen loss function.
### Linear Basis Function Models
The simplest linear model for regression is one that involves a linear combination of
the input variables:  $y(x, w) = w_0 + w_1x_1 + ... + w_Dx_D$ where  $\text{x}=(x_1,...,x_D)^T$
This is often simply known as linear regression. 
We therefore extend the class of models by considering linear combinations of fixed nonlinear functions of the input variables, of the form: 
$$y(\text{x, w})=\omega_0+\sum_{j=1}^{M-1}\omega_j\phi_j(\text{x})\text{, where }\phi_j(\text{x})\text{ are known as basis function}$$
The total number of parameter are M as the maximum index is _M_. The parameter $\omega_0$ is called _bias parameter_ as it allows any fixed offset in the data. Is useful to define a dummy basis function $\phi_0(\text{x})=1$ so y(*x*, *w*) is:
$$y(\text{x, w})=\sum_{j=0}^{M-1}\omega_j\phi_j(\text{x})=\text{w}^T\phi(\text{x})$$
$$\text{where w}=(\omega_0,...,\omega_{M-1})^T\text{   }\phi=(\phi_0,...,\phi_{M-1})^T$$
The function y(x,w) can be non-linear if we use nonlinear basis functions BUT the model is still linear because the function is linear in __w__.  
 One limitation of polynomial basis functions is that they are global functions of the input variable, so that changes in one region of input space affect all other regions. This can be resolved by dividing the input space up into regions and fit a different polynomial in each region, leading to spline functions.
####  Maximum likelihood and least squares
 consider the least squares approach, and its relation to maximum likelihood, in more detail. We assume that the target variable _t_ is given by a deterministic function _y(__x__, __w__)_ with additive Gaussian noise so that  $t = y(x, w) +\epsilon$  where $\epsilon$ is a zero mean Gaussian random variable with precision(inverse variance) $\beta$ . 
 Thus we ca write: $p(t|\text{x, w})=\aleph(t|y(\text{(x, w)}, \beta^{-1})$ . Recall that, if we assume a squared loss function, then the optimal prediction, for a new value of _x_, will be given by the conditional mean of the target variable.
 For a Gaussian conditional distribution similar to the one above the conditional mean will be simply $\mathbb{E}[t|x]=\int tp(t|x)dt=y(x,w)$. Note that the Gaussian noise assumption implies that the conditional distribution of t given x is unimodal, which may be inappropriate for some applications. 
 likelihood function, which is a function of the adjustable parameters __w__ and β, in the form:
 $$p(t|X,w,\beta)=\prod_{n=1}^N\aleph(t_n|\mathbf w^T\phi(\mathbf x_n),\beta^{-1})$$ Note that in supervised learning problems such as regression (and classification), we are not seeking to model the distribution of the input variables. Thus x will always appear in the set of conditioning variables(__x__ can be dropped from the expression).
 Taking the logarithm of the likelihood function, and making use of the standard form for the univariate Gaussian, we have:
 $$ln (p(t|\mathbf w,\beta))=\sum_{n=1}^Nln(\aleph(t_n|\mathbf w^T\phi(\mathbf x_n),\beta^{-1}))=\frac{N}{2}ln\beta-\frac{N}{2}ln(2\pi)-\beta E_D(\mathbf w)$$
 where the sum-of-square error function is defined by $E_D(\mathbf w)=\frac{1}{2}\sum_{n=1}^N\{t_n-\mathbf w^T\phi(\mathbf x_n)\}^2$ 
 We can use maximum likelihood to determine __w__ and β. 
 From the gradient of the log likelihood function equation setted to zero and solved in __w__ we obtain $\mathbf w_{ML}=(\varPhi^T\varPhi)^{-1}\varPhi^T\mathbf t$ which are known as the normal equations for the least squares problem. Here $\varPhi$ is an N×M matrix, called the _design matrix_, whose elements are given by $\varPhi _{nj} = \varPhi_j (\mathbf x_n)$, so that 
 $$\varPhi=\begin{pmatrix}  
   \varPhi_0(\mathbf x_1) & \varPhi_1(\mathbf x_1) & ... & \varPhi_{M-1}(\mathbf x_1) \\  
   \varPhi_0(\mathbf x_2) & \varPhi_1(\mathbf x_2) & ... & \varPhi_{M-1}(\mathbf x_2)\\ 
   ... & ... & ... & ...\\
   \varPhi_0(\mathbf x_N) & \varPhi_1(\mathbf x_N) & ... & \varPhi_{M-1}(\mathbf x_N) 
\end{pmatrix}$$
At this point, we can gain some insight into the role of the bias parameter $\omega_0$. If
we make the bias parameter explicit, then the error function become:
$$E_D(\mathbf w)=\frac{1}{2}\sum_{n=1}^N\{t_n-\omega_0-\mathbf w^T\phi(\mathbf x_n)\}^2$$
Setting the derivative with respect to $\omega_0$ equal to zero, and solving for $\omega_0$, we obtain:
$$\omega_0=\frac{1}{N}\sum_{n=1}^{N}t_n-\sum_{j=1}^{M-1}\omega_j\frac{1}{N}\sum_{n=1}^{N}\phi_j(\mathbf x_n)=\bar t-\sum_{j=1}^{M-1}\omega_j\bar\phi_j$$
Thus the bias $\omega_0$ compensates for the difference between the averages (over the training set) of the target values and the weighted sum of the averages of the basis function values. We can also maximize the log likelihood function with respect to the noise precision parameter β, giving: 
$$\frac{1}{\beta_ML}=\frac{1}{N}\sum_{n-1}^{N}\{t_n-\mathbf w_{ML}^T\phi(\mathbf x_n)\}^2$$
and so we see that the inverse of the noise precision is given by the residual variance of the target values around the regression function.
#### Geometry of least squares

![](https://i.imgur.com/F6WKXuH.png)

#### Sequential learning
Batch techniques, such as the maximum likelihood solution, which involve processing the entire training set in one go, can be computationally costly for large data sets. If the data set is sufficiently large, it may be worthwhile to use sequential algorithms, also known as on-line algorithms, in which the data points are considered one at a time, and the model parameters updated after each such presentation. Sequential learning is also appropriate for realtime applications in which the data observations are arriving in a continuous stream, and predictions must be made before all of the data points are seen.

![](https://i.imgur.com/R2ItnnS.png)

#### Regularized least squares
We introduced the idea of adding a regularization term to an error function in order to control over-fitting, so that the total error function to be minimized takes the form
$E_D(\mathbf w) + λE_W (\mathbf w)$ where $\lambda$ is the regularization coefficient that controls the relative importance of the data-dependent error $E_D(\mathbf w)$ and the regularization term $E_W(\mathbf w)$. 
One of the simplest forms of regularizer is given by the sum-of-squares of the weight vector elements  $E_w(\mathbf w)=\frac{1}{2}\mathbf w^T\mathbf w$. If we also consider the sum-of-squares error function given by $E(\mathbf w)=\frac{1}{2}\sum_{n=1}^{N}\{t_n - \mathbf W^T\phi(\mathbf x_n)\}^2$  then the total error function becomes 
$$\frac{1}{2}\sum_{n=1}^{N}\{t_n-\mathbf w^T\phi(\mathbf x_n)\}^2+\frac{\lambda}{2}\mathbf w^T\mathbf w$$
This particular choice of regularizer is known as weight decay because in sequential learning algorithms, it encourages weight values to decay towards zero, unless supported by the data. In statistics, it provides an example of a parameter shrinkage method because it shrinks parameter values towards zero. It has the advantage that the error function remains a quadratic function of w, and so its exact minimizer can be found in closed form. Specifically we obtain $\mathbf w = (\lambda\mathbf I+ \varPhi^T\varPhi)^{-1}\varPhi^T\mathbf t$, that represent a simple extension of the the least-square solution. 

![500](https://i.imgur.com/TR07JES.png)

![](https://i.imgur.com/7i1G6X1.png)

#### Multiple outputs
 In some applications, we may wish to predict K > 1 target variables, which we denote collectively by the target vector $\mathbf t$. This could be done by introducing a different set of basis functions for each component of $\mathbf t$, leading to multiple, independent regression problems. However, a more interesting, and more common, approach is to use the same set of basis functions to model all of the components of the target vector so that $y(\mathbf x,\mathbf w)=W^T\phi(\mathbf x)$ where y is a K-dimensional column vector, $\mathbf W$ is an M × K matrix of parameters, and $\varPhi(\mathbf x)$ is an M-dimensional column vector with elements $\mathbf \varPhi_j (\mathbf x)$, with $\varPhi_0(\mathbf x)=1$ as before. Suppose we take the conditional distribution of the target vector to be an isotropic Gaussian of the form $p(\varPhi t| \mathbf t, \mathbf W, \beta)=\aleph(\mathbf t|\mathbf W^T\varPhi(\mathbf x), \beta^{-1}\mathbf I)$. If we have a set of observations $\mathbf t_1,..., \mathbf t_N$ , we can combine these into a matrix $\mathbf T$ of size N × K such that the $n^{th}$ row is given by $\mathbf t_n^T$ Similarly, we can combine the input vectors $\mathbf x_1,..., \mathbf x_N$ into a matrix $\mathbf X$. The log likelihood function is then given by:
 $$ln(p(\mathbf T|\mathbf X,\mathbf W,\beta))=\sum_{n=1}^N ln\aleph(\mathbf t_n|\mathbf W^T\phi(\mathbf x),\beta^{-1}\mathbf  I)=\frac{NK}{2}ln(\frac{\beta}{2\pi})-\frac{\beta}{2}\sum_{n=1}^N\lVert\mathbf t_n-\mathbf W^T\phi(\mathbf x_n)\rVert^2 $$
 As before, we can maximize this function with respect to $\mathbf W$, giving $\mathbf W_{ML}=(\varPhi^T\varPhi)^{-1}\varPhi^T\mathbf T$. If we examine this result for each target variable $t_k$, we have $\mathbf w_k=(\varPhi^T\varPhi)^{-1}\varPhi^T\mathbf t_k=\varPhi^\dagger \mathbf t_k$ where $t_k$ is an N-dimensional column vector with components $t_nk$ for n = 1,...,N. Thus the solution to the regression problem decouples between the different target variables, and we need only compute a single pseudo-inverse matrix $\varPhi^†$ , which is shared by all of the vectors $\mathbf w_k$.
### Bayesian Linear Regression
