### Task - Association Rule Mining 

#### Aim

Perform an association rule mining (ARM) analysis (of your slice) of the Instacart dataset.

#### Detail

Perform ARM using __product__, __aisle__, and __department__ as the item.

To perform an ARM with item=product we need to reduce the number of products (from the initial 50K products). A crude approach is to simply drop all product which appear less than some minimum frequency threshold (say __PRODUCT_KEEP_MIN_FREQ=500__).

The table __orders_products__ needs to be one-hot encoded. Simplest way to do this is to first convert the order-product pairs to a transaction list and then (as in Practical B) to a one-hot encoding.  

~~~~.python
# convert table of (order,product) pairs to list of transactions
transactions = orders_products.groupby('order_id').apply(lambda order: order['product_id'].tolist())
~~~~

For each ARM with item (product, aisle, department) you should at least do the following subtasks:

 * Visualise the first 100 transactions vs items and comment (see Practical B).
 * Visualise the distribution of transaction size and comment (see Practical B).
 * Generate a few hundred frequent itemsets. You need to experiment (as discussed in class) to estimate a suitable value for the support threshold.
 * Generate a hundred+ rules and see if your can identify any interesting ones.

#### Disclaimers/Comments

 * When using __aisle__ and __department__ for item in the ARM model, we really should be using more advanced models than just (0=absent, 1=present).  Feel free to explore the more advanced models, but we would be happy with just the one-hot analysis.
 
 * When you perform ARM with __item=product__ we will find the your rule list will swamped by the organic fanboy/girl customers (my term not Bernard's) and I realy hope those customers buying the $\{lemon, lime\}$ itemset were doing it for non-healthy reasons like drinking gin! 
 
 * A more sophisticated analysis with __item=product__ would be to roll up (merge) essentially similar products. So 
   * All the organic fruit (${Organic\ Strawberries, Organic\ Raspberries, Bag\ of\ Organic\ Bananas, \ldots looong\ list\ldots }$) would just go to "organic fruit"
   * Merge products which differ only by brand name. 
   
#### Grading Outline

 * 3 x 25% for each of the separate basic ARM with item = (product, aisle, department) where the 25% consists of  
   * 10% Visualisation 
   * 5% Rule generation 
   * 10% Comments/analysis and identification of interesting rules.
 * 25% for performing some roll-up of products and a more sophisticated analysis.





 




