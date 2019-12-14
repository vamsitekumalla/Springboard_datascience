  
## Capstone Project 2 - Instacart : What's in the next order? 

------

### Background
-----
In the era of online shopping, we can buy almost anything in the span of a click - from electronics to clothing and groceries is also added to the list. Companies like Instacart are grabbing much clientele, helping the customers with grocery shopping needs with the convenience of being able to place an order from anywhere.

Instacart is a technology company which provides same day grocery delivery and pick-up service in the US and Canada. Customers shop for groceries through the Instacart mobile app or Instacart.com from the company's more than 300 national, regional and local retailer partners. The order is then shopped and delivered by an Instacart personal shopper.

Instacart's customers can access the service via the Instacart mobile app or website and begin shopping by selecting their city and store, then adding groceries to their digital cart. Customers can pay with personal debit or credit cards, Android/Google Pay and Apple Pay.

### Problem Description
------
Physical stores aid in visually introducing other items to the shopper which might potentially become an item in the purchase. In comparison with brick and mortar stores, the online stores give less exposure to other products (smaller visual field) and therefore the choice of product display on the page is of critical importance to ensure a fulfilling shopping experience.

Online grocery shopping, although being a great convenience, poses a particular problem - stickiness and customer retention. Creating a good shopping experience with increasing sales and seamless experience is becoming increasingly difficult for e-commerce websites. As a part of enhancing user experience, the project tries to predict which previously purchased products will be in a user’s next order. This is done by splitting the general reordering ( or not) prediction and no-reorder prediction and combining the above models to arrive at a more generalized model taking into consideration the User as well as Product features and combining both in the final model.

### Stakeholders and Impact
----------
Instacart’s ability to predict customer’s buy and be able to recommend products based on buying pattern and adaptation to their changing needs expectations - improves stickiness, helps in creating a seamless experience to and eventually promote user base for Instacart and maximize revenue.

Reordering prediction and thereby making recommendations to customer will help in making the shopping experience very personalized and will help users in having a smooth and seamless shopping experience. It will also help in faster checkout in comparison with having to search through the catalog of products offered on the app/website.

In addition to the above, the above prediction (and recommendation to customers) will help retail partners to have increased and continued sales through Instacart.


### Instacart dataset
_______________________________

`orders` (3.4m rows, 206k users):
* `order_id`: order identifier
* `user_id`: customer identifier
* `eval_set`: which evaluation set this order belongs in (see `SET` described below)
* `order_number`: the order sequence number for this user (1 = first, n = nth)
* `order_dow`: the day of the week the order was placed on
* `order_hour_of_day`: the hour of the day the order was placed on
* `days_since_prior`: days since the last order, capped at 30 (with NAs for `order_number` = 1)

`products` (50k rows):
* `product_id`: product identifier
* `product_name`: name of the product
* `aisle_id`: foreign key
* `department_id`: foreign key

`aisles` (134 rows):
* `aisle_id`: aisle identifier
* `aisle`: the name of the aisle

`deptartments` (21 rows):
* `department_id`: department identifier
* `department`: the name of the department

`order_products__SET` (30m+ rows):
* `order_id`: foreign key
* `product_id`: foreign key
* `add_to_cart_order`: order in which each product was added to cart
* `reordered`: 1 if this product has been ordered by this user in the past, 0 otherwise

where `SET` is one of the four following evaluation sets (`eval_set` in `orders`):
* `"prior"`: orders prior to that users most recent order (~3.2m orders)
* `"train"`: training data supplied to participants (~131k orders)
* `"test"`: test data reserved for machine learning competitions (~75k orders)

### EXPLORATORY DATA ANALYSIS

With the available data, following EDA was carried out :

- Which day has the most orders?

- What time of the day are most orders placed?

- Lull period (between reorders) - frequency of reorder

- Which department do most reorders belong to?

- Which aisle do most reorders belong to ?

- Average number of items in a reorder

- Number of organic products in reorders

- Most reordered products


### Machine Learning

Creating User and product level metrics and merging them with the initial dataframes, two separate models were generated predicting the 'Reorders' and 'NONE' and both were combined to arrive at the final Model. Process is detailed in the notebook. 



### LIMITATIONS
Limitations of the above model :
1.  Only limited features were extracted using the user x product combination. Inclusion of several other features pertaining to date/day/time of purchase/Department/Aisle etc could probably enhance the dataset and give out better accuracy in the final model but it was not tested due to time and computational constraints.
    
2.  XGBoost Classifier was used which is considered robust and fast among other Classification algorithms. We took F1 score as metric as it takes into consideration precision as well as recall. Other ensemble models were not tested which may or may not perform better than the ones used here.
    
3.  GridSearch Cross Validation and parameter tuning for the ‘reorder’ model and no fine tuning of parameters for ‘NONE’ reorder model : These were not carried out due to time and computational constraints, carrying out the same might affect the F1 score values of the final outcome.
    
4.  While calculating y_pred values for Iteration 2 of our first model, we have set a threshold of 0.22, tweaking that value might affect the final outcome of our model, which was not tested in this project.
    
5.  Only two models were considered in this project. In an ideal scenario, multiple models with different feature sets and combining their values using weighted averages will result in a better final outcome (F1 score of final model) but was not carried out due to time and computational constraints.