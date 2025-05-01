# The evolution of segmentation
Segmentation has a part in the strategical part of the process, it is a way to understand the context and the market we are in. It also helps in extracting customer segments to understand the type of customers the company wants to serve. 
## DIFFERENT WAYS OF SEGMENTING MARKETS
![](https://i.imgur.com/y0TASme.png)

Segmentation started from quantitative approaches and then merged with qualitative approaches to understand the customer at 360 grade.
- No segmentation: the market is not very differentiate, needs of customers are the same for all the typology, so we can have a single offer for everyone
- Customization: each person has completely different needs, so the company needs to approach each one with a different offer
- Single variable: segment the market based on a variable and differentiate the customer offers following this variable. 
- Multivariable: my customer are suddivided inmultiple categories and I can characterize the customer with some variables
## PATTERNS OF MARKET SEGMENTATION
![](https://i.imgur.com/jLKF4UY.png)

- Concentrated Preferences: the market is really concentrated on a single feature so we can have a single offer
- DIffused preferences: needs an offer for each customer
- Clustered Preferences: customers have different preferences, but there are some similarities that can be used to cluster them in a single offer.
## What is clustering?
Clustering means grouping similar individuals based on shared behaviors or features, without having to define those groups in advance. Groups that have common needs and should respond in the same way to a marketing action. Used to identify "lookalike" patterns and design.
**EXAMPLES OF VARIABLES**
- Purchase frequency
- Channel usage
- Content engagement
- Response to campaigns
- Digital affinity
The idea is that clustering is made of technique that identify similarities in the customers' consumes that let us aggregate them.
## MARKET SEGMENTATION AND TARGET SELECTION
![](https://i.imgur.com/RhNhocG.png)

## MARKET SEGMENTATION AND TARGET SELECTION
![](https://i.imgur.com/z7wLaDC.png)

The more you combine segmentation and the more complete the customer satisfaction will be completed. The more you move from the first two variables(that are statics) to the one that are more dynamics. 
## MARKET SEGMENTATION AND TARGET SELECTION
![](https://i.imgur.com/PlvjcFo.png)

You need to target a specific type of customer.
Then needs to understand if our products are coherent with the customer environment. Then we need to analyze the target buyer to understand his/her needs.
## SEGMENTATION IN THE MARKETING PROCESS
![](https://i.imgur.com/jrI5cTA.png)

You need to profile the customer segments to understand if they are relevant to our business and can become a real opportunity. 
Targeting is evaluating the attractiveness of each segment. Once selected the target segment we need to see how we can position in this segment, how can we position in it. Once we decided we need to communicate our position to the customer to remain relevant in their mind.
## MARKET STRATEGIES ACCORDING TO EXPRESSED PREFERENCES
![](https://i.imgur.com/zN5YAUK.png)

Easier to have the specialization on a single customer in automation as usually the customer has really specific needs. There are ways to automate/make efficient these hyperspecialization.
## MARKET SEGMENTATION PROCESS
![](https://i.imgur.com/BBCWAqN.png)

- Survey Stage: data acquisition stage, you try to understand the information you have regarding the customer and, in case you do not already have the information in house, you need to do some interviews.
- Analysis stage: apply your euristical approaches to these data to extrapolate the customer segments
- Profiling stages:after the application of your models you have to profile the results that you obtained
## WHAT MAKES A GOOD CLUSTER?
A good cluster is a cluster that is:
- INTERNALLY CONSISTENT: formed by elements similar in nature
- CLEARLY DISTINGUISHABLE: formed by elements different in nature from other clusters’
- ACTIONABLE & APPROACHABLE: it is possible to design a specific message or action from it and it is possible to approach it.
- MEASURABLE & BUSINESS-RELEVANT 
NOTE THAT
Clustering is not just a data science task, it is a way to turn raw behavior into marketing strategy.
A strong segment/cluster needs to be internally consistent(users/customers need to be consistents internally), outside needs to be diversity. Need to be actionable and approachable: the segment needs to be approachable for financial actions and needs to be "easy" to develop a product for a customer.
The segments need to be business relevant, no need to have millions of open opportunities if they fragment too much your capabilities.
## THE SEGMENTATION PROCESS
1. Select the variables to be considered and the methods to be used: The final aim is to develop a product/market matrix
2. Data collection and analysis: Find out is the selected variables are significant and sufficient to have a useful segmentation
3. Combine the data: Heuristic approaches vs statistical approaches
4. Describe and name the segments
If you do this in a good way you will have a product/market matrix

![](https://i.imgur.com/KuZ8b6i.png)

This matrix will represent the best market for your company.
## HEURISTIC APPROACHES: SEGMENTING THROUGH VARIABLE CROSSING
The first simple way to address clustering is to cross 2 or more variables, below a couple of common examples:
![](https://i.imgur.com/Tpdv5Vh.png)

On the left there is an european company classification, it uses 2 variables(turnover on the left and number of employee on the right).
The other one if Recency Frequency Monetary value and is used to understand the customer better. This are heuristic as they combine these three variables and we can see the customers that are better for us.
### DEEP DIVE RFM SEGMENTATION
This segmentation approach groups customers based on purchasing, Recency, frequency and Monetary value.
![](https://i.imgur.com/m4DAWKP.png)

### PORTER’S SEGMENTATION WITH SUCCESSIVE ELIMINATIONS
In this method, you start from all the possible segmentation variables on a market. Segments are created removing from all the possible combinations of variables those combinations that are nonsense (e.g., expensive goods for poor people).
You select a group of segmentation variables, choose the most relevants(based on what you think is the best approach), compare them and see if there are combinations in the matrix that are not relevant crossing out them. Then position the product in the matrix to have the product matrix. 

![](https://i.imgur.com/org2sQB.png)

## COMMON CLUSTERING TECHNIQUES
**HIERARCHICAL**
- DESCRIPTION: Builds a nested tree of clusters (dendrogram), merging or splitting groups step-by-step. Suitable for smaller datasets or when cluster structure is unknown.
- USE CASE EXAMPLE: Customer lifetime value tiers, profiling loyalty levels.
Useful has it can show the ramification of your segmentation
**K-MEANS**
- DESCRIPTION: Most common clustering algorithm; partitions data into a predefined number "k" of clusters based on distance/similarity.
- USE CASE EXAMPLE: Digital behavior segmentation (e.g., e-commerce customer groups, user engagement levels).
**LATENT CLASS ANALYSIS**
- DESCRIPTION: Statistical technique that identifies unobserved ("latent") subgroups within a population based on response patterns across observed variables. It estimates the probability that an individual belongs to each latent group.
- USE CASE EXAMPLE: Segmentation of survey respondents based on attitudes or behaviors (e.g., health behavior profiling, etc.).
Tries to identify subgroups in the population and afterwards the model assign a probability on each element of the market and the more distant the label is to the unit the more distant it is to belonging to the cluster. Powerful when you do not know what to do/you want to see if there are some cluster.
**COMPLEX ML-BASED CLUSTERING**
- DESCRIPTION: Uses advanced machine learning techniques to dynamically identify clusters (e.g., deep learning, etc.). Often operates in real time.
- USE CASE EXAMPLE: Dynamic personas (evolving clusters updated in real-time), Behavioral clustering from mobile app logs, Clustering of unstructured content (e.g., user comments or reviews), Real-time segment creation for next best action systems
These approaches are interchangables, but there are cases where one approach is better suited for the needs.
### LATENT CLASS ANALYSIS
This approach identifies hidden (latent) groups within a population based on response patterns across observed variables:
1. CHOOSE THE NUMBER OF LATENT CLASSES
2. ESTIMATE CLASS MEMBERSHIP PROBABILITIES FOR EACH POINT 
3. ASSIGN INDIVIDUALS PROBABILISTICALLY
Each individual is assigned to a latent class based on the highest probability.
 ![](https://i.imgur.com/lDFq6N6.png)

### K-MEANS CLUSTERING
This approach partitions the data into clusters each represented by its centroid:
1. Choose the number of clusters “k” (cfr. following slide) 
2. Assign each point to the nearest centroid 
3. Update centroids “position” based on assigned points
The more the classes are near the center of the centroid the more they are similar. You continues to analyze the data until the situation doesn't change too much even if you change some parameters. Very fast as an approach, very little computational expensive.

![](https://i.imgur.com/2kUdQ5X.png)

#### HOW TO CHOOSE «K»: ELBOW METHOD
![](https://i.imgur.com/iZZXSbS.png)

### HIERARCHICAL CLUSTERING
A clustering technique that builds a hierarchy of clusters by merging smaller
clusters (agglomerative) or splitting larger ones (divisive).
APPROACHES
1.  Agglomerative (Bottom-up): each point has its own cluster
2. Divisive (top-down): all points in one big cluster
WHEN TO USE IT
a. When you don’t know how many clusters to expect
b. When interpretability  matters
c. When analyzing nested data (e.g., subsegments)
PROS
- No need to predefine clusters
-  Clear visualization (dendrogram)
CONS
- Computationally expensive
- Sensitive to outliers
![](https://i.imgur.com/ikxr9N8.png)

The drawback of this approach is that is really expensive in regard to the computation. This approach helps to understand what happened and helps in understanding the nesting of the data.
## COMPARING COMMON CLUSTERING TECHNIQUES
![](https://i.imgur.com/qOOycjF.png)

# From Target to Dynamic Personas
Needs to use qualitative clustering approaches as Personas to better understand our market. The target is no more enough to clearly understand the market.
## INNOVATION CURVES IN PROFILING
![](https://i.imgur.com/9XcKsex.png)

Target 2.0 start to see how the consumer is behaving and why he is doing so
Today Target 2.0 is not enough and we need to jump to Dynamic Personas , a specific way to address the new needs of the customers and how they think about it. Recent concept, static personas are from 1993 and helped designer to develop tools/web pages as personas gave a clear idea of how the consumer could behave, it clarifies how someone behave and how the segmented persona acts.

> “Personas are not real people, they are hypothetical archetypes of actual users defined with significant rigor and precision”
	Alan Cooper - 1998

Buy personas are really interesting nowadays as they are the one that represent the customers that will buy our products. 

![](https://i.imgur.com/Tjw4E2o.png)

> “Personas and scenarios help designers to imagine the users and aid development of design ideas. The concept of engaging personas and narrative scenario explores personas in the light of what it is to identify with and have empathy with a character. The concept of narrative scenarios views the narrative as aid for exploration of design ideas" 
> 	Lene Nielsen - 2004

## WHAT EXACTLY IS A PERSONAS?
> «A semi-imaginary representation of the ideal customer, based on market research and real data about existing clients, originating from a deep need (PAIN). 

![](https://i.imgur.com/Lx4SYxB.png)

When consumers take decisions they use the reptilian brain and this is where the PAIN is talking to. We are talking about simple things that moves the people at deep level not brands or products.
A few seconds is the time needed to take a decision:
- What am I looking at? (Context)
- Why should I care? (Value)
- What should I do next? (Action)
### PAIN
![](https://i.imgur.com/HmptNPG.png)

The main point for the start of Erik's decisions is that he wants more time for himself.
### GAIN
![](https://i.imgur.com/QqCEPH0.png)

Mark's need is to manage the time to have some time for himself.
> Personas have represented a significant step forward in understanding the customer.

## BUT PERSONAS AND BEHAVIORAL TARGETING HAVE SOME LIMITS
![](https://i.imgur.com/l2kbDk3.png)

They are static so they do not consider the change of a variable.
They cannot be transfer between people. 
## DYNAMIC PERSONAS MODEL
![](https://i.imgur.com/Fep6pYt.png)

There is the PAIN that moves the persona but it can changes regarding the environment/variables the persona is subjected to.
### MASK
![](https://i.imgur.com/OTrTaWs.png)

In each mask the sensibility change w.r.t. some aspect of the persona. Every persona can have different masks for different situations.
### WHAT ARE THE ADVANTAGES?
![](https://i.imgur.com/FVFfWUR.png)

So you can interact with the customer in different ways w.r.t. the context or approach a customer only on specific context. The company can use specific context for specific PAIN. 
#### PERSONAS’ APPLICATION AREAS
![](https://i.imgur.com/HMpFoTC.png)

