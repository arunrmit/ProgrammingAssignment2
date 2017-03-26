
Assignment Instructions
_________________________________________________
### Introduction

This second programming assignment will require you to write an R function that is able to cache potentially time-consuming computations. 
For example, taking the mean of a numeric vector is typically a fast operation. 
However, for a very long vector, it may take too long to compute the mean, especially if it has to be computed repeatedly (e.g. in a loop). 
If the contents of a vector are not changing, it may make sense to cache the value of the mean so that when we need it again, it can be looked up in the cache rather than recomputed. 
In this Programming Assignment you will take advantage of the scoping rules of the R language and how they can be manipulated to preserve state inside of an R object.

### Example: Caching the Mean of a Vector

In this example we introduce the `<<-` operator which can be used to assign a value to an object in an environment that is different from the current environment. Below are two functions that are used to create a special object that stores a numeric vector and caches its mean.

The first function, `makeVector` creates a special "vector", which is really a list containing a function to

1.  set the value of the vector
2.  get the value of the vector
3.  set the value of the mean
4.  get the value of the mean

<br />__________________________________________________________________________________________________
<br /># Creating makeCacheMatrix function to set and get the value of vector.<br />
#Also, to set and get the mean of vector<br /><br />
makeCacheMatrix &lt;- function(x = matrix()) {
<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; invData &lt;- NULL
<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; set &lt;- function(y){
<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; x &lt;&lt;- y
<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; invData &lt;&lt;- NULL
<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; get &lt;- function() x
<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; setInv &lt;- function(mean) invData &lt;&lt;- mean
<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; getInv &lt;- function() invData
<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; list(set = set,
<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; get = get,
<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; setInv = setInv, 
<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; getInv = getInv)
<br />}<br /><br /># The below function help to compute inverse matrix retuned by makeCacheMatric
<br /># Actually this function check whether the mean is already is already created or not
<br />#If it find out mean is already created, it skips the computation and provide the output
<br /><br />cacheSolve &lt;- function(x, ...) {<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ## Return a matrix that is the inverse of 'x'<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; invData &lt;- x$getInv()<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if(!is.null(invData)){<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; message("getting cached data")<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return(invData)<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; data &lt;- x$get()<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; invData &lt;- solve(data,...)<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; x$setInv(invData)<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; invData&nbsp;&nbsp;&nbsp; &nbsp;<br />}<br />---------------------------------------------------------------------------------------------<br />#Testing my function<br /><br />#Creating 2*2 matrix and assigning it to the makeCacheMatrix<br />&gt; MyData &lt;- makeCacheMatrix(matrix(c(1,2,3,4),2,2))<br /><br />#Calling cacheSolve function for the first time<br />#Function is computing the value and returning the inverse matrix first time<br /><br />&gt; cacheSolve(MyData)<br />&nbsp;&nbsp;&nbsp;&nbsp; [,1] [,2]<br />[1,]&nbsp;&nbsp; -2&nbsp; 1.5<br />[2,]&nbsp;&nbsp;&nbsp; 1 -0.5<br /><br />#Calling the cacheSolve function again and this time it returning the value from the cache<br />#with the message, instead of computing again<br /><br />&gt; cacheSolve(MyData)<br />getting cached data<br />&nbsp;&nbsp;&nbsp;&nbsp; [,1] [,2]<br />[1,]&nbsp;&nbsp; -2&nbsp; 1.5<br />[2,]&nbsp;&nbsp;&nbsp; 1 -0.5<br /><br /></p>
