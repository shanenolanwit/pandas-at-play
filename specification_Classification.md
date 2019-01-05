### Task - Classification 

#### Aim

Multi-label classification with numerical variables (of your slice) of the
Instacart dataset, predicting the product mix in the next order for a user.

#### Detail

The problem to solve takes the form of _Classification with numerical
independent variables_.

Given the history of __order__s per user, Instacart would like to predict the
mix of __product__s that would be ordered next. From a business perspective,
there is little to be gained from promoting a product that would be purchased
in any case, but much to be gained from promoting a related product. Therefore,
knowing the baseline (i.e., without marketing intervention) could improve the
efficiency of product marketing efforts.

As with the regression task, we recommend that you choose a _relevant_ subset
of users for analysis, where _relevant_ implies an unbiased subset chosen to
make the problem tractable with the computing resources available to each
student.

There are two subtasks that need to be considered:
1. taking historical order data from (a subset of) users and using this data to
   predict the next order for a typical user, given that user's order history
2. taking historical order data from a _single_ user and using this data to
   predict the next order for that user.

The basic hypothesis in the first of these is that there is an underlying
consistency across users in terms of their order behaviour, so the more data
that is available, the better the prediction of a typical user's next order.

By contrast, the hypothesis in the second of these is that the order behaviour
of different differs from each other, so it is better to tailor next order
predictions to individual users.

We recommend the following three features (derived independent variables) for
each __product__:

1. relative quantity across all orders for that user (computed as the number of
   times it is ordered divided by the number of all products ordered)
2. relative frequency of reorder (so if ordered twice in 4 orders, the reorder
   rate is (2-1)/(4-1) = 1/3). If not reordered, it takes the value 0.
3. (some function of) "add to cart order", across all orders for a given user.
   For example, if it was ordered 3 times, and was the first, third and fifth
   item added to these orders, the corresponding feature would take the _mean_
   value. Optionally, students might wish to investigate different ways of
   calculating this mean: the traditional [arithmetic
   mean](https://en.wikipedia.org/wiki/Arithmetic_mean) (3 in this case) or the
   [harmonic mean](https://en.wikipedia.org/wiki/Harmonic_mean) (approximately
   1.96 in this case) that gives more weight to smaller values.

The 3 features above are chosen in an attempt to capture the following
predictive attributes:
* how often a product is ordered, compared to all other products for that user
* how frequently a product is reordered by a given user
* using "add to cart order" as a proxy to represent the importance of the
  product to the user

Regarding feature 3, Kieran analysed the reorder rate as a function of "add to
cart order" and found that it declined steadily as the "add to cart order"
increased. This trend stopped abruptly at "add to cart order" 35
(approximately) when the relationship broke down, by exhibiting large
oscillations.

Therefore, we recommend that you, as part of the Exploratory Data Analysis
phase, plot reorder rate (across all _relevant_ products and users) against
"add to cart order", and use this to determine when the simple relationship
breaks down, say at n=35.

You can then use linear regression to fit a low-order polynomial (linear or
quadratic, say) to reorder rate as a function of "add to cart order" for the
restricted range while the relationship shows a stable trend. Using the linear
regression you learned, you should then be able to extrapolate a reorder rate
for higher "add to cart order" values.

Indeed, it should be possible to use the results of this linear regression (and
its extrapolation to higher "add to cart order" values) to lookup a generic
reorder rate for a given (mean) "add to cart order" for a given __product__ for
a given __user__. This generic reorder rate can then be used in place of
feature 3 above, as it is derived from the same data and represents much the
same behaviour.

The _dependent_ variable is the product mix in the next order for that user in
the training set, expressed as a set of binary variables (0,1-valued: product
is included or not).

The number of __product__s could cause difficulties, because the number of
features (hence columns in the observation matrix) is 3 times the number of
__product__s considered in the model, and the dependent variable is
vector-valued, comprising indicator values for each of those products.

Therefore, you are recommended to limit your analysis as follows:
1. with multiple __user__s, take the most popular 200 products, say
2. with a single __user__, limit to the __product__s purchased by that user in
   their orders to date.

Regarding training, test data and validation, we recommend the following:

1. given order data from multiple users, the training data comprises all orders
   for an 80% subset of the _relevant_ users. The learned classifier can then
   be applied to order data from the remaining 20% subset of users. For
   validation, you need to measure how well the classifer predicts the last
   order for those "test" users.
2. given order data from a single user, the training data for that user
   excludes the last order for that user. For testing, the learned classifier
   is applied to all the data including the last order and the __product__ mix
   of the last order is predicted. For validation, the predicted last order can
   be compared with the actual last order for that user.

#### Disclaimers/Comments

* Feature engineering is a very important part of this task, because Instacart
  does not provide labels as part of the data. Therefore, they need to be
  derived.
 
* The classifiers covered in this module generally do not support as many
  options for dimensionality reduction as linear regression does, so depending
  on the scope of the learned classifier, we recommend either a user-specific
  product subset, or a "Top N" approach (of products purchased by the
  _relevant_ users) in this task.  Needless to say, it is essential to ensure
  that the same subset of __product__s is used for features and for the
  vector-valued dependent variable.

* One of the objectives in this task is to determine whether per-user or
  across-user models make better predictions. Therefore, you will need to
  compare the two model scopes and suggest which is more applicable to the
  Instacart data.

* Multilabel classification, as used here (because the "product mix" dependent
  variable is vector-valued, taking a 1 or 0 in each product "slot" depending
  on whether the product is included or not), is most commonly used in document
  classification.
  
* The relevant `sklearn` libraries for multilabel classification can be found
  [here](http://scikit-learn.org/stable/modules/multiclass.html). For a text
  mining example, we might wish to label a book with multiple non-exclusive
  attributes, such as its theme, its genre, its intended audience, and whether
  it belongs to a series or not. When used for market basket analysis, as here,
  the main difficulty with multilabel classification is that the number of such
  labels (identifying products uniquely) can be enormous.

#### Grading Outline

 * 25% deriving features 1 and 2 (will require data manipulation)
 * 25% deriving feature 3 (will require solving a linear regression
   sub-problem)
 * 20% choosing and using a classifier
 * 30% validating the results, to include analysis of confusion matrix,
   precision, recall and between-model metrics such as F1-score etc.

