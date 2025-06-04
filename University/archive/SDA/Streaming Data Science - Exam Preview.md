ar# Questions to test the breadth of your TSA knowledge
### What is the i.i.d. assumption? How do SML and TSA allow the loosening of part of it?
The i.i.d. assumption implies that data are independent and identically distributed, so each data point is independent from the others belonging to the same distribution. SMA allow the loosening of the i.i.d. data assumption detecting data drift and changing the data distribution learning to the new setting, but forgetting what it learned before. TSA assumes that data aren't i.i.d. and works on the learning of the model and forecasting the new data points
### Which characteristics can a non-stationary time series have? Illustrate it with an example.
A non - stationary time series can have a trend and a seasonality. An example ca be the articles and post related to the new Playstation consoles. As they have a cycle of more or less 5-7 years we can see an increase of mentioning of the tag Playstation console when the life cycle is ending and this trend is seen for each console.
### Why is the white noise a predictable time series? What is its forecast? What is its error? Explain it w.r.t. the possible components of a time series.
The White Noise is predictable as it is not dependent to the time of observation and its best forecast is its mean that is equal to zero. Its error correspond to the white noise variance. The White Noise do not present trends thanks to the fact that its mean and variance are constant so the WN cannot have trend and seasonality.  
### Which are the methods to test for stationarity? Explain one of them in detail.
We can use statistical tests for stationary like the ADF test, the KPSS test, the OCSB test or the CH test. The ADF is a type of statistical test called unit test. It uses an autoregressive model and optimizes an information criterion across multiple different lag values. **A unit root in a time series implies unpredictability** because it is associated with non-stationarity, and non-stationary data often exhibits erratic and unpredictable behavior. This makes it difficult to make reliable predictions about the future behavior of a time series with a unit root. Here's why a unit root leads to unpredictability:
- _Non-Stationarity_: A unit root suggests that a time series variable has a trend component, and its statistical properties (such as mean and variance) do not remain constant over time. In other words, the data does not have a fixed, stable level. This lack of stationarity means that the data can exhibit irregular and unpredictable patterns.
- _Persistence of Shocks_: When a time series has a unit root, it is susceptible to "shocks" or unexpected changes that can have a lasting impact. For example, if there's an unexpected increase or decrease in a variable's value, the unit root implies that the variable may not revert to a stable level. Instead, the shock can lead to persistent deviations, making it difficult to predict when the variable will return to its initial state.
- _Cumulative Effects_: Unit root processes can accumulate changes over time. This means that past values of the variable can continue to influence future values. As a result, the variable's path can be affected by its historical behavior, making it challenging to forecast its future values.
#### Interpreting the ADF test results

The null hypothesis of the test is that the time series can be represented by a unit root, that it is not stationary (has some time-dependent structure). The alternate hypothesis (rejecting the null hypothesis) is that the time series is stationary.

- **Null Hypothesis (H0)**: If failed to be rejected, it suggests the time series has a unit root, meaning it is non-stationary. It has some time dependent structure.
- **Alternate Hypothesis (H1)**: The null hypothesis is rejected; it suggests the time series does not have a unit root, meaning it is stationary. It does not have time-dependent structure.

We interpret this result using the p-value from the test. A p-value below a threshold (such as 5% or 1%) suggests we reject the null hypothesis (stationary), otherwise a p-value above the threshold suggests we fail to reject the null hypothesis (non-stationary).

- **p-value > 0.05**: Fail to reject the null hypothesis (H0), the data has a unit root and is non-stationary.
- **p-value <= 0.05**: Reject the null hypothesis (H0), the data does not have a unit root and is stationary.
###  Which are the typical time series components? Illustrate your explanation with an example.
A Time Series is composed of a Trend component, representing the overall persistent, long-term change, a Seasonal component, representing the regular periodic fluctuations, and a Residual component, representing the residual fluctuation(hopefully stationary). An example can be the San Remo music festival articles and post during the year. The Trend part can be represented by the increase in publications during the months that anticipates the festival with the peak during the festival week. The seasonality component can be derived from the fact that the festival is an annual thing and the residual component  can be derived from the post that may appear during the year. 
### Which are the methods to detrend a time series? Explain one of them in detail.
There are three methods to detrend a time series:
- Method 1: Trend elimination by differencing(Differencing one or more times removes the trend)
- Method 2: Trend estimation by model fitting & removal(We estimate the trend fitting a model and then we remove it)
- Method 3: Trend estimation by using “centered” moving averages(We estimate the trend using a “centered” moving average and we remove it
Trend elimination by differencing works by creating a new series where the value at the current time step is calculated) as the difference between the original observation and the observation at the previous time step : 
$$value(t) = observation(t) - observation(t-1)$$
This has the effect of removing a linear trend from a time series dataset. This method can resolve a linear trend for each iteration. This mean that we can remove a linear trend iterating one time, a quadratic trend iterating twice and so on.
### Which are the methods to identify seasonality in a time series? Explain one of them in detail.
There are  three methods to identify seasonality:
- Method 1: seasonal difference(transformation of the series to a new time series { S𝑡 } where the values are the differences between the value of { 𝑋𝑡 } at time t and the the value of { 𝑋𝑡 } a period d before)
- Method 2: Estimate the trend using “centered” moving average
	- Given the seasonality’s period d
		- If d is even, the «centered» moving average is defined as
			$$\hat {m_t}=(0.5x_{t-q}+x_{t-q+1}+...+x_{t+q-1}+ 0.5x_{t+q})/d$$
		- If d is odd, the «centered» moving average is defined as
			$$\hat {m_t}=(x_{t-q}+x_{t-q+1}+...+x_{t+q-1}+x_{t+q})/d$$
- Method 3: Joint-fit method(we can fit a combined polynomial linear regression and harmonic functions to estimate and then remove the trend and seasonal component simultaneously as the following)
#### Seasonal differencing:
Seasonal differencing of a time series { 𝑋𝑡 } in discrete time t given the seasonality’s period d is the transformation of the series to a new time series { S𝑡 } where the values are the differences between the value of { 𝑋𝑡 } at time t and the the value of { 𝑋𝑡 } a period d before.
$s_𝑡 = x_𝑡 - x_𝑡 - d$
### Which are the methods to forecast a time series? Compare two methods of your choice(excluding the basic ones).
The methods to forecast a time series are the Naive approach, the Average approach, Last-k average approach, Exponential Smoothing, Double Exponential Smoothing(aka Holt's linear method) and Triple Exponential Smoothing (a.k.a. Holt-Winters method),
The Last-k average approach retain that only the last k previous values are important  to make prediction($\hat y_{t+1}=\frac{1}{k}\sum_{i=0}^{k-1}y_{t-i}$)but there is the problem of finding the best k. Exponential Smoothing is a way between the naive and the average approach where we don't consider only the last predicted value and the values don't have all the same weight.  $\hat y_{t+1}=\alpha y_t+(1+\alpha)\hat y_t$ (where $0<\alpha <1$ is the smoothing level). Note that $\alpha$ close to one implies a fast forgetting(only the most recent values influences the forecast) and an $\alpha$ close to zero implies a slow forgetting(past values have a large impact on the forecasts). Exponential smoothing is a streaming algorithm as the next prediction is the sum of the current prediction and $\alpha$ times the current error($\hat y_{t+1}=\hat y_t + \alpha(e_t)$).   
### What are the components of a SARIMAX model?
The SARIMAX model contains all the components of the SARIMA model(_p_ non seasonal AR order, _d_  non seasonal differencing, _q_ non seasonal MA order, _P_ seasonal AR order, _D_ seasonal differencing, _Q_ : seasonal MA order, _S_ : seasonality) but with an extension to incorporate _r_ exogenous variables. Exogenous variables are external factors that can impact the time series but are not influenced by it.
### Which are the benefits and limitations of \[MAE / MAPE / MSE / RMSE]? Provide a practical example where its use is beneficial and one where it should be avoided.

|      | Benefits                                                                                               | Limitations                                                                                                            |
| ---- | ------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| MAE  | Easy to calculate, understand and<br>interpret.                                                        | Sensitive to outliers; may not penalize large errors sufficiently.                                                     |
| MAPE | Provides a percentage error representation; can be used to make comparisons across different datasets. | Susceptible to division by zero, i.e., when actual values are close to zero. Biased towards actual values. Asymmetric. |
| MSE  | Penalizes larger errors more significantly than small errors                                           | Units are squared, which might be less intuitive in some contexts.                                                     |
| RMSE | Provides an interpretable scale for MSE like the original data.                                        | Sensitive to outliers like MSE; not robust against extreme values.                                                     |
- MAE can be used for forecasting the wild animal population inside the park of Monza denoting if there is a species that has an increase in population bigger than the others. It isn't beneficial for the forecast of the attendee to a Formula1 events as a big error can greatly impact on the transportation system of the city.
- MAPE can be beneficial when we wants to compare the improvements of different software suites in a company. It isn't beneficial for predicting the storage capacities of a company as there could be values equal to zero.
- MSE is beneficial for the estimation of the increase of the value of a stock. It is not useful when we wants to estimate the value of a building as $\$^2$ doesn't make sense.
- RMSE can be used for evaluating new medical treatment. It cannot be used in the prediction of the winner of SerieA as there could be a promoted team that do really well.  
### Describe how Prophet works and what each of its key components does. Highlight at least three advantages of using Prophet in time series forecasting.
Prophet is essentially “framing the forecasting problem as a curve-fitting exercise” rather than looking explicitly at the time based dependence of each observation.
In practice the Prophet procedure is an additive regression model with the following main components:
- A piecewise linear or logistic growth curve trend. Prophet automatically detects changes in trends by selecting changepoints from the data.
- A daily, weekly and yearly seasonal component modeled using Fourier series. Prophet relies on Fourier series to provide a malleable model of periodic effects. The seasonal component s(t) provides a adaptability to the model by allowing periodic changes based on sub-daily, daily, weekly and yearly seasonality.
- A user-provided list of important holidays. Holidays are modeled as independent dummy external regressors allowing the inclusion of a window of days around the holidays to model their effects.
It is also possible to add exogenous variables, i.e., additional regressors. Prophet provides a more general interface for defining extra linear regressors and does not require that the regressor be a binary indicator. Prophet is optimized for the business forecast tasks at Meta, which typically have any of the following characteristics:
- hourly, daily, or weekly observations with at least a few months (preferably a year) of history
- strong multiple “human-scale” seasonalities: day of week and time of year
- important holidays at irregular intervals known in advance (e.g. the Super Bowl)
- a reasonable number of missing observations or large outliers
- historical trend changes, e.g., product launches or logging changes
- trends that are non-linear growth curves, where a trend hits a natural limit or saturates
# Questions to test the depth of your TSA knowledge
### What’s the difference between an additive and a multiplicative model for time series decomposition? Illustrate your explanation with an example.
The residual of the addictive decomposition does not have constant variance. In the multiplicative model the trend is incorporated in the seasonality. An additive model is linear meanwhile a multiplicative model is non linear. The additive model can be used to analyze the global population growth and we can use a multiplicative model to look at the post regarding the San Remo festival.
### Why is exponential smoothing named in this way? Discuss the formula of at least one of the three methods presented in the course.
It is called this way because the weights decrease exponentially over time:
Starting with $\hat 𝑦_{t+1}= 𝛼𝑦_t + (1 − 𝛼)\hat 𝑦_t$ we can substitute for $\hat 𝑦_t$ and get
	$$\hat y_{t+1}=𝛼y_t+(1 − 𝛼)[𝛼y_{t-1}+(1−𝛼)\hat y_{t-1}]= 𝛼y_t + 𝛼(1−𝛼)y_{t-1}+(1−𝛼)^2\hat y_{t-1}$$
Continuing to substitute until the first element of the time-series leads to
$$\hat y_{t+1}=\alpha y_t+\alpha(1-\alpha)y_{t-1}+\alpha(1+\alpha)^2y_{t-2}+...+\alpha(1-\alpha)^{t-1}y_{1}+(1-\alpha)^tl_0$$
where $l_0$ represents the first fitted values at time 1
### What’s the difference between simple, double, and triple exponential smoothing? Illustrate the explanation by highlighting the respective components in the triple exponential smoothing formula.
The simple exponential smoothing predicts the next value with all the previous values considered in a weighted sum. The double exponential smoothing adds support for the trend using an additional smoothing factor (called 𝛽) to control the decay of the inﬂuence of the change in trend. The triple exponential smoothing adds support for
seasonality using a new parameter gamma that controls the influence on the seasonal component.

![](https://i.imgur.com/R6K2BSz.png)


### What’s the difference between the meaning of moving average in time series decomposition and the MA component in ARMA models? Illustrate your explanation using the corresponding formulas.
In time series decomposition we use the moving average as an estimator of the trend of the trend(if d is even $\hat {m_t}=(0.5x_{t-q}+x_{t-q+1}+...+x_{t+q-1}+ 0.5x_{t+q})/d$ or if d is odd  $\hat {m_t}=(x_{t-q}+x_{t-q+1}+...+x_{t+q-1}+x_{t+q})/d$) meanwhile the ARMA consider the MA as the model of q previous estimation error(with the MA being $y_t=c+\epsilon_t+\sum_{n=1}^{q}\theta_n\epsilon_{t-n}$). 
We can see from the formulas how the time series uses the values meanwhile the ARMA uses the errors.
### What’s the definition of Autocorrelation? How does it differ from the definition of correlation? Illustrate your explanation with an example.
Autocorrelation describes the degree of similarity between a given time series and a lagged version of itself over successive time intervals. Correlation describes the degree to which two variables $X_1$ and $X_2$ move in coordination with one another.
So autocorrelation is like taking photos of the building phase of a new house and then comparing the different phase. Correlation is like taking photos of the construction of two different buildings at determined time instants and compare them.  
### What’s the difference between the AR and the MA part of an ARMA model? What is their relation to Autocorrelation and Partial Autocorrelation? Illustrate your explanation using ACF and PACF plots.
The AR part of the ARMA model is composed of p previous values and the MA part of the ARMA model is composed of q previous estimation errors. 
The model is AR if the ACF tails off after a lag and has a hard cut-off in the PACF after a lag. This lag is taken as the value for p. The model is MA if the PACF tails off after a lag and has a hard cut-off in the ACF after the lag. This lag value is taken as the value for q.
The model is a mix of AR and MA if both the ACF and PACF tail off. 

![](https://i.imgur.com/uz9diyV.png)
![](https://i.imgur.com/bdWx7Hx.png)
### How does the Box-Jenkins Methodology for ARMA models allow us to estimate the orders of the model? Illustrate your explanation with an example.
We can use the Box-Jenkins Methodology to see when the PACF and ACF plot cut off gaining the order of the model for the AR(PACF) and MA(ACF). When ARMA(p,q) has p and q different from zero we can use the grid search. 

![](https://i.imgur.com/KZjEuA9.png)

### What are the exogenous variables in TSA? How do they contribute to the forecasting? What’s their impact on the confidence interval? Illustrate your explanation with an example.
Exogenous variables are external factors that can impact the time series but are not influenced by it. They shrink the confidence interval of the model w.r.t. the model without the exogenous part, thus making predictions more robust.
When we forecast the selling of tickets for a football match we can add the exogenous input of the rainfall in the period to see impacts on the revenues.
# Questions to test the breadth of your SML knowledge
### What are the differences between batch-oriented Machine Learning and Streaming Machine Learning?
Batch based machine learning can do random access to data, has a well defined training phase, doesn't have restriction on the memory/time for training and can access all the labelled data used for training, Streaming Machine Learning can only do sequential access, has evolving data, has strict memory/time limit requirements and maintain the characteristics of the data seen so far. 
### What are the benefits and the challenges of Streaming Machine Learning?
SML can analyze one data point at a time evolving the model accordingly, can work with incremental models and can have time and memory management. The challenges that SML needs to overcome are the fact that data streams can be non i.i.d. so needs to adapt to data drifts, there can be class imbalance so it is difficult to train correctly the model and the hyper-parameters tuning is a complex aspect.
### What is a concept drift? Which are the types of concept drift? Why is it so important to detect it? Illustrate the difference using the Bayes Theorem and with an example.
Concepts drifts are changes that occurs in the data of our stream.
The types of concept drifts are:
- Virtual/Data drift: cases in which only the input distribution changes
- Real/Concept drift: cases in which the change of input distribution causes a boundary shift
It is important to detect it because it will decrease drastically the accuracy of our model. Bayes theorem is:
$$p(y|X_t)=\frac{p(y)p(X_t|y)}{p(X_t)}$$
A concept drift is a sudden change in the properties of the target variables that can degrade the model’s performance. It can result from changes in: 
1. The probability of observing a class. 
2. The conditional probability of an item given a class. 
3. The probability that an item belongs to a specific class (real concept drift), which affects the decision boundary and requires model adaptation.
Drift speed influences detection difficulty: Faster changes are easier to detect, while gradual or incremental changes are more challenging. Drift types include sudden, gradual, incremental, recurring, and blips (outlier-like phenomena). Detecting concept drift is essential because it determines whether the current model remains effective. Unlike anomaly detection, where the model remains unchanged, concept drift detection prompts model adaptation if needed to maintain performance.
### How can you classify a concept drift with respect to the speed of change? Illustrate the difference with an example for each type.
- Abrupt/Sudden drift: the drift happens instantly. It is like in a fabric that produces triangles suddenly starts to produce circles
- Gradual drift: the drift happens over time but is done by two distinct classes. It is like in a fabric that produces triangles one day produces some circles then return to produce triangles and repeats this cycle increasing the number of consecutive circles seen and reducing the number of triangles seen until it only produces circles
- Incremental drift: the change between classes happens over time. It is like a fabric that produces triangles and one day start also producing circles in an inferior number than the triangles at the start. Over time the fabric increases the production of circles decreasing the production of triangles until it only produces circles
- Recurring drift: there is a sudden drift between two classes that can be repeated over time. It is like a fabric that can change its production between triangles and circles instantly.
### Which are the typical Streaming Machine Learning algorithms for classification? Illustrate one of them in detail.
For classification in SML we can use Naive Bayes, K-Nearest Neighbours, Online K-Nearest Neighbours, Online KNN with ADWIN, Decision Trees, Hoeffding Trees, Concept Adapting VFDT, Hoeffding Adaptive Tree, CASH problem and AutoML, EvoAutoML.
Online K-NN with ADWIN uses a fixed size sliding window to save the instances it works on. The most common label of the k instances closer to a new instance determines its label. The distance between instances is calculated (commonly) using the Euclidean Distance: $d(a,b)=\sqrt{\sum_{i=1}^{m}(a_i -b_i)^2}$. If a concept drift occurs, with K-NN there is the risk that the instances saved into the window belong to the old concept.
Use ADWIN to automatically set the size of the sliding window to save the instances
### What are the key factors to consider when building an SML Ensemble Classification model?
The key factors in an Ensemble classification model are Diversity(induce diversity among learners), Combination(combine the predictions), Adaptation(adapt to evolving data).
### How does SML regression compare to time series forecasting w.r.t. types of input features, training, forecasting horizon, and adaptability?
SML Regression models learn how to predict data coming from a continuous flow. To predict a new label, they must see the respective features. 
TSA models learn from time series in the past how to forecast future behaviours, without knowing the respective future time series value.
# Questions to test the depth of your SML knowledge
### Which are the concept drift detectors that monitor the error rate? Illustrate how one of them works
They are the Drift Detection Method (DDM), Early Drift Detection Method
(EDDM), Adaptive sliding WINdow(ADWIN).
The Early Drift Detection Method(EDDM) works considering the distance between two errors classification instead of considering only the number of errors. While the learning method is operating, it will improve the predictions and the distance between two errors will increase. When a drift occurs, the distance between two errors will decrease. Compute the average distance between 2 errors and its std, and look for outliers in the tails.
### Which are the data drift detectors? Illustrate how one of them works.
The data drift detectors are the Cumulative SUM Test (CUSUM) and Page Hinkley Test. 
The Page Hinkley Test works detecting a change in the average of a Gaussian signal and monitors the difference between $g_t$ and $𝐺_t$. Its accuracy depends on the choice of parameters v and h. 

![](https://i.imgur.com/sAI2ghY.png)

### How does ADWIN detect a concept drift? Illustrate it with an example.
ADWIN (ADaptive WINdowing) is a popular drift detection method with mathematical guarantees. ADWIN efficiently keeps a variable-length window of recent items; such that it holds that there has no been change in the data distribution. This window is further divided into two sub-windows ($W_0, W_1$) used to determine if a change has happened. ADWIN compares the average of $W_0$ and $W_1$ to confirm that they correspond to the same distribution. Concept drift is detected if the distribution equality no longer holds. Upon detecting a drift, $W_0$ is replaced by $W_1$ and a new $W_1$ is initialized. ADWIN uses a significance value $\delta \in(0,1)$ to determine if the two sub-windows correspond to the same distribution.

![](https://i.imgur.com/2EHFkOy.png)


### How does the SML version of KNN work? Illustrate it with an example.
[KNN](https://riverml.xyz/0.21.2/api/neighbors/KNNClassifier/) is a non-parametric classification method that keeps track of the last window_size training samples. The predicted class-label for a given query sample is obtained in two steps:
- Find the closest n_neighbors to the query sample in the data window.
- Aggregate the class-labels of the n_neighbors to define the predicted class for the query sample.
The strategy to perform search queries in the data buffer is defined by the `engine` parameter.
We have a stream of cats and dogs images that are analyzed by our model. A new image arrives and the model finds the n- neighbours that are closer to the characteristics of the image and then aggregate the class labels of the n-neighbours to define the predicted class  for the query sample. Then the oldest value in the window is discarded and the new value is added to the window.
### How does the SML version of Naïve Bayes work? Illustrate it with an example.
It uses a batch of n elements on which it calculates the mean and variance. 
It maintains a Gaussian distribution for each class and each feature. Each Gaussian is updated using the amount associated with each feature. The joint log-likelihood is then obtained by summing the log probabilities of each feature associated with each class.
We want to predict the next day weather. We classify each day and look at its features and calculate our metrics.  
### How does the Hoeffding Tree work? Illustrate it with an example.
The Hoeffding Tree build the decision tree incrementally. The final tree must be identical (with high probability) to a tree built using a batch decision tree algorithm. With theoretical guarantees on the error rate. A small sample can often be enough to choose the optimal splitting attribute
- Collect sufficient statistics from a small set of examples
- Estimate the merit of each attribute
We estimate the size of the sample with a Moving size: Choose the sample size that allow to differentiate between the alternatives. Use Hoeffding bound to guarantee
that the best attribute is really the best:
- Let $X_1$ and $X_2$ be, respectively, the two most informative attribute
- Split if: $H(X_1) - H(X_2) > \epsilon = \sqrt{\frac{R^2 log(1/\delta)}{2N}}$
where R is the H range, 𝛿 is the confidence bound and N is the number of instances seen by that node.
We can select a datastream of destinations in Italy. The first attribute is distance: if less than 100 km we can assign the label car, otherwise we look at the second attribute that is the wanted cost: if less than 70$ we take the train, otherwise we take the airplane label  
### How does the Hoeffding Adaptive Tree work? Illustrate it with an example.
Replace frequency statistics counters by estimators:
- Don’t need a window to store examples, since it maintains the statistics data needed with estimators
Change the way of checking the substitution of alternate subtrees, using a change detector with theoretical guarantees (ADWIN)
- Keeps sliding window consistent with the no-change hypothesis
We can select a datastream of destinations in Italy. The first attribute is distance: if less than 100 km we can assign the label car, otherwise we look at the second attribute that is the wanted cost: if less than 70$ we take the train, otherwise we take the airplane label. We can use ADWIN to see drifts in, for example, the distance as people that travel for less than 100 km could prefer to take the bike.
### What is Hoeffding Bound? How and why is it used in SML to build a decision tree?
The Hoeffding bound ensures reliable splitting by calculating the minimum number of
samples needed to confidently select the best attribute. It is used to better classify the attributes inside the datastream. 
### Why is the Poisson distribution used in SML ensemble methods? What’s the impact of the value of lambda? Illustrate how lambda is used in the SML methods.
The Poisson distribution is used in SML ensemble methods because the data streams are supposed to be unbounded and the binomial distribution tends to a Poisson(1). Lambda adds more weight during the training of the learner. Lambda is the parameter of the Poisson distribution that will assign the weight distribution to the learning process as the weight k = Poisson($\lambda$)
# Questions to test the breadth & depth of your CL knowledge
### What’s the stability-plasticity dilemma? Illustrate your explanation with an example and discuss the different learning abilities.
The stability-plasticity dilemma is the fact that a model needs to balance its capabilities to learn new knowledge(plasticity)  and the capabilities of the model to remember the past knowledge(stability), the trade-off comes from the fact that a model too plastic tends to easily forget past knowledge, meanwhile a too stable model tends to not learn new knowledge. A models that needs to learn how to recognise cats suddenly needs to recognise dogs if it is too plastic it will forgets how to recognise cats and if it is too stable it will not learn how to recognise dogs.
### What are the main differences between Streaming Machine Learning and Continual Learning paradigms? Discuss it w.r.t to the objective of the methods and the type of data they process.
Data Handling: SML processes single data points or micro-batches. CL works on experiences, which are larger batches of data that can be divided into training and test sets. 
Reuse of Data: In SML, data points are discarded after a single pass. In CL, experiences can be reused to train the model over multiple epochs.

### What is the difference between the definitions of concept drift in SML and CL? When is avoiding forgetting meaningful? Illustrate it with an example.
SML Concept Drift: Defined as changes in the data distribution, feature-target relationships, or class boundaries over time. Focuses on adapting to real drifts, where class boundaries change, making past knowledge irrelevant. Prioritizes quick adaptation and current performance, often disregarding past concepts.
CL Concept Drift: Represents the introduction of new tasks, typically involving virtual drifts (new features or related problems). Aims to retain knowledge from past tasks while learning new ones. Balances learning stability and plasticity to avoid forgetting relevant past knowledge.
In SML if we are learning how to recognize dogs and suddenly we start to receive images of cats the model will start to recognize cats forgetting how to recognize dogs. In CL if we are learning how to recognize dogs and suddenly receive cats images the model will start learning how to recognize cats STILL remembering how to recognize dogs.
### What are the three main scenarios in Continual Learning?
1. Incremental Task Learning: Tasks are distinct, with task labels available during training and testing. The model uses task labels to focus on specific parameters for each task. Goal: Retain performance on previous tasks while learning new ones.
2. Incremental Domain Learning: Tasks involve the same classes but in new feature subspaces (changing distributions). The model adapts to new distributions without forgetting previous knowledge. Example: Recognizing even/odd numbers in a dataset where features vary across tasks.
3. Incremental Class Learning: Each task introduces new classes. The model must expand its classification ability over time while retaining prior knowledge. Example: Adding birds and fish to a model initially classifying only cats and dogs, with no task labels available.
### How do CL replay strategies work? Explain one specific strategy in detail. What are the other two categories of CL strategies? Briefly explain them.
CL Replay Strategies: The Replay strategy consists in mixing old examples to new ones during each experience to not forget old tasks. The downside of this strategy is that the training set size increases with the number of tasks. We can 
Latent Replay (Example): Stores a random subset of data points from previous tasks in a replay memory(RM). When training on a new task, a subset of the current data replaces some RM data. Replays old and new data together, enabling knowledge retention while learning new tasks.

![](https://i.imgur.com/klohNIg.png)


CL regularization strategy consists in changing the loss function by adding regularization terms to the new task’s problem and not losing old ones.
CL Architectural strategy consists in adding new parameters to the network, for some of these strategies the number of parameters could increase with the number of tasks and usually they require task labels.
### How do CL regularization strategies work? Explain one specific strategy in detail. What are the other two categories of CL strategies? Briefly explain them.
CL regularization strategy consists in changing the loss function by adding regularization terms to the new task’s problem and not losing old ones.
Elastic Weight Consolidation adds a regularization term to the loss to penalize changes on important parameters for previous tasks. The idea is to search for the optimal configuration lying in the overlapping of different tasks’ solution spaces.

![](https://i.imgur.com/RDD1qTJ.png)

Optimizing very different tasks together could result in not finding a good trade-off:
- Some tasks may perform well, while others badly.
- In the worst scenario, we could find a solution that does not fit all the tasks.
EWC results, in fact, in a tug-of-war over the direction of change of each task.

CL Replay Strategies: The Replay strategy consists in mixing old examples to new ones during each experience to not forget old tasks.
CL Architectural strategy consists in adding new parameters to the network, for some of these strategies the number of parameters could increase with the number of tasks and usually they require task labels.
### How do CL architectural strategies work? Explain one specific strategy in detail. What are the other two categories of CL strategies? Briefly explain them.
CL Architectural strategy consists in adding new parameters to the network, for some of these strategies the number of parameters could increase with the number of tasks and usually they require task labels.

![](https://i.imgur.com/QpO1zTH.png)

It starts with a single Neural Network (column).
 Whenever a new task appears:
- It freezes the parameters of the old columns.
- It adds a new column.
The hidden layer i (i>1) of the column k receives:
- The output of the hidden layer i-1 of k.
- The outputs of the hidden layers i-1 of all the previous columns j (j<k).
It uses transfer learning to combine the previous knowledge with the current one. It freezes old columns to avoid forgetting. As the Multi-Head models, it needs the task label.
CL Replay Strategies: The Replay strategy consists in mixing old examples to new ones during each experience to not forget old tasks.
CL regularization strategy consists in changing the loss function by adding regularization terms to the new task’s problem and not losing old ones.

### Which are the primary evaluation metrics used in Continual Learning? Illustrate what they measure and why they are all useful to assess the properties of a method.
Primary Evaluation Metrics in Continual Learning: 
- Usually, each experience is split into train and test sets. After each experience's training, we compute the metrics on all the experiences' test sets (we are supposed to have all the test sets available).
	- Average Accuracy: Mean of final performances on all the experiences’ test sets. $ACC = \frac{!}{T}\sum_{i=1}^{T}R_{T.i}$ 
	- A Metric: For each experience’s training, we test on the current and the previous experiences. 
$$A= \frac{\sum_{i=1}^{N}\sum_{j=1}^{i}R_{i,j}}{\frac{N(N+1)}{2}}$$
- To measure the impact of the current experience’s training on future/previous experiences, we introduce two other metrics:
	- Forward Transfer Metric: It measures how the current training improves performance for the next experience.  $\frac{1}{1+T}\sum_{i=2}^{T}R_{i-1,i}-\bar b_i$
	- Backward Transfer Metric: It measures how the final model has improved or decreased the performances on the previous experiences. $\frac{1}{1+T}\sum_{i=2}^{T-1}R_{T,i}-R_{i,i}$
	A negative value indicates forgetting!