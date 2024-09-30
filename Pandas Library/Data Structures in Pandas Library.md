# [[Index]]

---
# Series
A Series is nothing more than a sequence of data. Other than the values it contains an additional array with label values corresponding to the values array,  used to identify the values in the series.
### Building a Series
Using the pandas constructor *__Series(iterable)__* we can create a series containing the values in iterable. We can also specify the indexes in the parameters of the constructor: *__Series(iterable, index = iterable2)__*, iterable and iterable2 must have the same length 
The constructor can present others values than data:
- data: correspond to the data in the series
- index: can be None, correspond to the index of the series and has to be the same length of the data
- dtype: can be None. If specified forces the type in the series
- name: represent the name of the series, can be None
- copy: default to False
- fastpath: default to None
We can create a series using a NumPy array as the data
We can also use the *__random,randn(number)__* as the data input to create a series with random numbers or we can use the function *__numpy.arange(start, finish)__* to create a series with sequences of numbers. 
We can create a series from a scalar number using this number as the data input and specifying the index.

### Series values
We can obtain the value in a series using the method *__values__*
### Series index
We can obtain the indexes in a series using the method *__index__*
We can override the series indexes at any time: *__index =  iterable__*
We can define separately the index with the constructor *__Index(iterable)__* which has to be included in the parameters of the series initialization
### Accessing series values
You can access an element using is index values as you do with a list: *__series\[index]__*
In this way we obtain all the values corresponding to the index
### Name attributes
We can assign a name to the series and to its index with the method *__name = "NAME"__* for the series and *__index.name = "NAME"__* for the index

# DataFrame
It is basically a set of series

![](https://i.imgur.com/erp1W71.png)

It also added a new index array corresponding to the column
# Building a DataFrame
We can use the constructor *__DataFrame()__* to create a new DataFrame.
The parameters of the constructor are(all of them can be None):
- data: represent the various data in the DataFrame
- index: represent an array which values identify the data
- column: represent an array which values identifying the collection of the data
- dtype: represent the type of the elements in the DaatFrame, forcing a conversion if necessary
- copy: indicates if the data is passed by value or refererence. It is False by default and it indicates that the pass is made by reference.
### DataFrame values
We can obtain the value in a dataframe using the method *__values__*
### Series index
We can obtain the indexes in a dataframe using the method *__index__*
We can override the dataframe index at any time: *__index =  iterable__*
We can define separately the index with the constructor *__Index(iterable)__* which has to be included in the parameters of the datafram initialization
Same thing can be done for the columns only using the command *__columns__* instead of index
### Accessing DataFrame values
To access certain subsets of values contained in the dataframe we can use two selector: *__iloc__* and *__loc__*.
With iloc we can specify the row and the column of the desired element or we can select a specific row(\[index]) or column(\[:, index]).
The method loc works the same way but accepts the indexes and columns as parameters
### MultiIndex DataFrame
Used when the structure used has a multilevel hierarchical. The various dimension can be isolated using iloc\[] and loc\[]
Usually is preferred to extract a simple DataFrame containing the values needed instead of using the complete DataFrame
