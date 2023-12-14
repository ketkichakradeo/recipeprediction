# Predicting Recipe Ratings

**Authors**: Ketki Chakradeo (kchakradeo@ucsd.edu) and Kayanne Tran (kqtran@ucsd.edu)

---

Our **exploratory data analysis** on this dataset can be found [here](https://ketkichakradeo.github.io/recipeanalysis/).

---

## Project Overview

In this project, we chose to explore a dataset composed of various recipes with their ratings and reviews to to try to classify a recipe into a rating group based on certain features.

Our dataset is a subset downloaded from [food.com](http://food.com), and it was originally scraped and used by the authors of [this](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf) recommender systems paper. The data comes in two raw csv files: RAW_recipes.csv and RAW_interactions.csv. The former dataset contains recipe names, ingredients, nutritional facts, reviews, and more information regarding the recipes themselves, while the latter dataset contains user reviews and ratings for each recipe.

This is a data science project for UCSD's DSC80 class. In this project, we will clean the data and train a classification model on the data. 

Through this exploration, we aim to contribute to a deeper comprehension of the multifaceted nature of human satisfaction and connection through the medium of food.

## Framing the Problem
**Problem Identification**
We are exploring the following question: Can we predict a recipe rating based on recipe length, number of steps, and number of ingredients?

This prediction problem is a **classification** type of problem. We are performing multi-class classification as each rating is its own class. In this problem, there are 5 possible ratings, ranging from 1 star to 5 stars, and thus each rating is a different class and there are 5 classes in total.

The **response variable** is the the recipe `'rating'`. We are trying to predict how many stars a recipe will get based on other features. At the time of prediction, the only features that we know are the recipe length, number of steps, and number of ingredients. All of this information is not dependent on the actual execution of the recipe, therefore we are able to use these features to train our model.

The **metric** we are using to evaluate our model is the precision score. We chose this score because the distribution of classes was not very balanced. In this scenario, making a wrong prediction could be pretty costly because it involves people's likes and dislikes, which is why we prioritized precision. Finally, our main interest is in the performance of the model on how correctly it identifies positive instances. Good ratings are more important to reccommend because the subjective nature of bad reviews makes it crucial to avoid overlooking potentially good options.

## Baseline Model
**Model Description**
Our baseline model is a Decision Tree Model. We tried to predict the rating of a recipe. We utilized a train-test-split model and used a test size of 0.25.

**Features in the Model**
In our baseline model, there are a few features we are examining. They are: `'minutes'` (quantitative continuous) , `'n_steps'` (ordinal discrete)  and `'n_ingredients'` (ordinal discrete). 

**Encoding and Transformation**
None of our features were qualititave. Therefore, we did not have to use any encoding techniques such as One Hot encoding. However, we employed a Binarizer to help classify certain recipes as having many steps or not that many steps. We applied the Binarizer to the `'n_steps'` feature in order to get an array of '1' and '0'. If the recipe had a lot of steps, it was labeled as a '1' and as a '0' otherwise. We used the Binarizer on the other columns too, `'minutes'` and `'n_ingredients'`. 

**Performance of the Model**
After running our model, we can see that the test and train score were high and very similar to each other. Therefore we beleive our baseline model is good. The actual scores themselves are pretty high; both are in the 77% range, which means that it correctly predicts a rating more than 3/4 of the time, which we think is very good. Additionally, the test and train score are almost equal to each other, showing that the baseline model does generalize well to other datasets.

|Training Precision Score|Testing Precision Score|
|---|---|
|0.7750951868909223	|0.7764207023879568|

## Final Model
**Added Features**
For our final model we decided to include more features relating to the nutrional facts of the recipe. We added the `'calories'` and `'sugar features'` and transforomed them to represent `'high_calorie'` and `'high_sugar'` using a Binarizer. This is very similar to what we did on the other columns. We chose to add these columns because the nutritional value of a recipe could also determine what people think of it. People might give very low ratings to recipes with high calories and high sugar content. We also decided to include `'avg_rating'` and `'review'` as we think adding more features will help to create a stronger model.

**Modeling Algorithm**
The modeling algorithm we chose was Random Forest Classifier. The benefits of using this type of model are that it is an ensemble method that combines the predictions of multiple decision trees, it is less prone to overfitting unlike a decision tree, and it can properly deal with imbalanced data.

To select the **hyperparameters**, we used a Grid Search algorithm that tested out different values for the different parameters of a Random Forest. The main parameter we focused on was max_depth, as too deep of a model can lead to overfitting. Through the grid search, we were able to find out the the best max_depth was 22 for our model.

The **performance** of our final model is a great improvement over the baseline model. The precision score of our final model is significantly higher than the baseline model, which is most likely due to the added features and hyperparamter tuning of the max_depth. 

**Visualization**
<iframe src="/graphs/cfm" width=800 height=600 frameBorder=0></iframe>


## Fairness Analysis
To determine if our model is truly fair, we will specifically examine possible bias between the difference in performance between two defined groups. We wanted to see if recipes with higher average ratings and lower average ratings were rated fairly. Here, we define “higher average ratings” as greater than or equal to 3.5, and “lower average ratings” as those less than 3.5. Our metric will be precision scores because we wish to focus on the true positive predictions our model makes.

**Null Hypothesis:**
Our model is fair. Its precision for recipes with higher average ratings and lower average ratings are roughly the same, and any differences are due to chance.

**Alternative Hypothesis:**
Our model is unfair, its precision for recipes with higher average ratings is higher than the higher average ratings.

**Test Statistic:**
We will use the signed difference between the weighted precision score for these two groups (higher_avg_precision - lower-avg_precision). We are using weighted average to account for the class imbalance in the dataset since there are a lot more instances of higher ratings than lower ratings in both rating and avg_rating columns.

**Significance Level:**
The significance level is α = 0.05.

**P value:**
The p-value was 0.0.

**Conclusion:**
We ran permutation tests over 1000 iterations, shuffling the rating column and calculating the precision scores for the two respective groups defined above. Our computed p value was 0.0, which is less than our significance level, so we reject the null hypothesis. This implies that our model is biased towards higher average ratings.

**Visualization:**
<iframe src="/graphs/differences_dist.html" width=800 height=600 frameBorder=0></iframe>


