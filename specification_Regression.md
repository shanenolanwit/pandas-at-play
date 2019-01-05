### Task - Linear regression 

#### Aim

Perform a linear regression analysis (of your slice) of the Instacart dataset,
predicting the number of days between orders.

#### Detail

The problem to solve takes the form of _Linear Regression with categorical
independent variables_.

Consider the sequence of orders placed by a customer. The hypothesis is that
the typical product mix for that customer can be used to predict when the next
order will be placed.

For training data, take an 80% subset of "relevant" __order__s, across all
"relevant" __user__s. From the first to the second last order, determine the
__product__ mix for that __user__, as well as the __number of days__ until the
next __order__ for that __user__.

The training data is the remaining 20% subset of "relevant" __order__s, across
all "relevant" __user__s. 

For example, if there were just 1 user and that user placed 11 orders, the
training set could include the product mixes for each of the following 8
orders: 1, 2, 4, 5, 6, 7, 9, 10, together with the "days between orders" for
each of the 8 order pairs (1,2), (2,3), (4,5), (5,6), (6,7), (7,8), (9,10),
(10,11). In this case, the test data would include the product mix for
order 3 and 8, together with the "days between orders" for each of the 2
order pairs (3,4) and (8,9).

Please note that the proposed 80:20 split is across user x order combinations. 

Also, in practice, _relevant_ implies an unbiased subset chosen to make the
problem tractable with the computing resources available to each student.

For _Independent variables_, the __product__ (mix) for __order__ `i-1` can be
used, and the _Dependent variable_ is the __number of days__ between __order__
`i-1` and __order__ `i` in that sequence for a given __user__.

Therefore the number of observations in the observation matrix is the number
of __user__s times the number of __order__s for each __user__.

Since there are 49688 distinct products, the dimensionality of the problem is
very high, so students need to take account of this using:

1. roll up to __aisle__ (134 unique) or __department__ (21 unique) and build
   model accordingly 
2. use PCA to reduce the dimensions (of __product__s, __aisle__s and/or
   departments), and solve using the reduced-dimension model
3. use regularisation and fit a constrained model.

#### Disclaimers/Comments

* It is possible to obtain either per-user predictions (by solving a separate
  regression problem for each user) or a user-agnostic prediction (by merging
  the data for many users). It would be interesting to try both and compare,
  explaining what you find. 
 
* Much of this task relates to dimensionality reduction. In traditional BI,
  this would be done using roll-ups (e.g., from __product__ to __aisle__), but
  linear regression offers other options. The BI approach is easier to
  interpret, because of the "meaningful" labels, but does it work as well? Give
  reasons for your answer!

* Validating the results requires analysis of the outputs (and not just the
  predictions) of the linear regression model, e.g., R-squared and any other
  metrics you believe are helpful here.
   
#### Grading Outline

 * 3 x 25% for each of the 3 regression model x dimensionality reduction options above, where the 25% consists of  
   * 5% Visualisation 
   * 10% formulation and solving 
   * 10% Validation/Comments/analysis.
 * 25% for comparing per-user and multi-user models, and different methods for dimensionality reduction.
