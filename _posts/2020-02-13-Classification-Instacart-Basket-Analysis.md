---
layout: post
title: Classification: Instacart Basket Analysis

---

*an overview of my classification project predicting user product purchases.

-----

## Project Scope

My third project was to use supervised learning techniques to create a model. I chose to look at the Instacart Basket Analysis dataset from competition that Instacart ran on Kaggle in 2017. We learned several classification models such as Logistic Regression, K-Nearest Neighbors, Random Forest. I utilized these and several more to try and predict user purchases.

## Data Cleaning

Instacart's original dataset came with several csv files.

* Orders - user id, order id, day of week, order hour, order number
* Orders Train - order id, product id, add to cart order, reordered (boolean)
* Orders Prior - same as above, for different set of previous orders.

I joined each of the Train and Prior files with the main Orders file. Then split the Train data.
I removed the items with N/A in the days_since_prior_order for the Train set since these products had no order history.

* Products
* Aisles
* Departments

I used these documents only for the Exploratory Data Analysis stage of the project.

## Methodology

### Feature Engineering

The Kaggle dataset I used was pretty raw in that the features are very basic. A model ran with only columns such as user_id, order_id, and product_id do not have much importance in predicting individually. With feature engineering, I was able to create better features. This was done with SQL on AWS as well as Pandas. I used group by aggregation on both SQL and Pandas.

* User Features
* Product Features
* User-Product Features

### Modeling

For my project, I focused on the F1 Score for my model optimization.
**Why F1 Score?** This is due to a few factors. First, the data had a high class imbalance. Far more people did not order a product in their next order than did. F1 Score doesn't punish you for have a high amount of True Negatives. It also creates a balance between Precision and Recall. This was important for my model because Instacart wants high scores for both. A balance between these metrics would translate to more products being recommended to users and being careful not to recommend poor products that users won't convert on.

I used Logistic Regression, KNN, Random Forest, ADA Boost and XGBoost. My initial comparison of these models looked at the F1 score. I then took the top models (everything except KNN) to the next stage where I plotted their ROC curves on the same graph.

** ROC CURVE IMAGE **

I then used GridSearchCV to find the optimal parameters for XGBoost and Random Forest to get these models at their highest F1 scores. This involved looking for the best learning rate, number of trees, and max depth of the trees.

After pulling the parameters from my highest scores, I ran the models on the test set of data. Next, I performed F1 Threshold Optimization and adjusted the F1 threshold to help the model best classify it's predictions.


## Results

My final scores for the model were:

Metric | Score
-------|--------
F1 | 0.4149
Accuracy | 0.7905
Recall | 0.5139


### Future Work

I would to create more features with aisle and departments as I did not use those columns in my model.
A significant hurdle I had to overcome in this project was the size of the data. To be able to better train my model, I would like to have more computational power so that I can include all data. Lastly, my most important feature was a User-Product aggregated feature that showed the frequency of a product in the user's last 5 orders. After seeing the huge value in the most recent orders when compared to the entire order history, I would like to create more features that focus on most recent orders.