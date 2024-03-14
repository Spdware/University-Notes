How to evaluate and select models in ML. 
### Bias Varinance
It is a theoretical framework that allow to have a deeper understanding of why you have some error in a ML model. 

![](https://i.imgur.com/8jSGnY5.png)

This is the average error the model is going to have for every input. Much more complete assesement of the model than the loss function as the loss function is only based on the dataset and not on all the possible inputs. The problem reside in the fact that the Bias Variance is a theoretical process.

![](https://i.imgur.com/sqBccMy.png)

This result can be generalized also for classification. 
The function f(.) is generating data. $E[(t-y(\mathbf x))^2]$(expected error) is representing all the possible model the neural network will give and for every dataset it will receive.
The higher the $\sigma^2$ the higher the noise and the error. Data noise($\sigma^2$) is the variance of data and does not depend neither from data sampling nor from the model complexity.
#### Model Variance and Bias

![](https://i.imgur.com/VDyvmoR.png)

Variance: variance measures the difference between each model learned from a particular dataset and what we  expect to learn: $variance=\int E[(y(\mathbf x)- E[y(\mathbf x)])^2]p(\mathbf x)d\mathbf x$ 
Decreases with simpler models, decreases with more samples
A higher variance lead to a model more sensitive to the dataset used to train it.
The variance contains a term related to the dimension of the dataset. The larger the dataset the smaller the variance and so the error(More data means the capability to handle more complex data).
Bias: bias measures the difference between truth(f) and what we expect to learn($E[y(\mathbf x)]$): $bias^2=\int (f(\mathbf x)- \bar y(\mathbf x))^2p(\mathbf x)d\mathbf x$.
Decreases with more complex models.
It is like a systematic error the model is doing

![](https://i.imgur.com/0Q3dVkg.png)

We want a smooth function to have a greater probability of having a smaller variance. 
### Model Assessment in Practice

![](https://i.imgur.com/owqMP0o.png)

![](https://i.imgur.com/5js7Rjt.png)

![](https://i.imgur.com/lNBCKht.png)

Increasing the size of the test set will give me a more accurate test error but it will give me less points for the training set and it is a zero gain function. A better solution is to introduce a new set of data called validation set. It will be introduced in a clever way going to cut out only a minimal part of data.

![](https://i.imgur.com/fvYctmY.png)

