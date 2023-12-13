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

The metric we are using to evaluate our model is the precision score. We chose this score because the distribution of classes was not very balanced. In this scenario, making a wrong prediction could be pretty costly because it involves people's likes and dislikes, which is why we prioritized precision. Finally, our main interest is in the performance of the model on how correctly it identifies positive instances. Good ratings are more important to reccommend because the subjective nature of bad reviews makes it crucial to avoid overlooking potentially good options.

## Baseline Model
**Model Description**
Our baseline model is a Random Forest Regression Model. We tried to predict the rating of a recipe. We utilized a train-test-split model and used a test size of 0.25.

**Features in the Model**
In our baseline model, there are a few features we are examining. They are: `'minutes'` (quantitative continuous) , `'n_steps'` (ordinal discrete)  and `'n_ingredients'` (ordinal discrete). 

**Encoding and Transformation**
None of our features were qualititave. Therefore, we did not have to use any encoding techniques such as One Hot encoding. However, we employed a Binarizer to help classify certain recipes as having many steps or not that many steps. We applied the Binarizer to the `'n_steps'` feature in order to get an array of '1' and '0'. If the recipe had a lot of steps, it was labeled as a '1' and as a '0' otherwise. We used the Binarizer on the other columns too, `'minutes'` and `'n_ingredients'. 

**Performance of the Model**
After running our model, we can see that the test and train score were high and very similar to each other. Therefore we beleive our baseline model is good. The actual scores themselves are pretty high; both are in the 77% range, which means that it correctly predicts a rating more than 3/4 of the time, which we think is very good. Additionally, the test and train score are almost equal to each other, showing that the baseline model does generalize well to other datasets.
| Training Precision Score | Testing Score | 
----:|----:|
| 0.7750951868909223 | 0.7764207023879568 |

## Final Model
**Added Features**
For our final model we decided to include more features relating to the nutrional facts of the recipe. We added the `'calories'` and `'sugar features'` and transforomed them to represent `'high_calorie'` and `'high_sugar'` using a Binarizer. This is very similar to what we did on the other columns. We also decided to include `'avg_rating'` and `'review'` as we think adding more features will help to create a stronger model.
**Modeling Algorithm**

**Visualizaation**


## Fairness Analysis
**Group X:**
egfjaef

**Group Y:**

**Null Hypothesis:**

**Alternative Hypothesis:**

**Test Statistic:**

**Significance Level:**

**P value:**

**Conclusion:**

**Visualization:**
















### Introduction to the Dataset
In our RAW_recipes.csv dataset, there are 83782 rows from 2008 to 2018, and 10 columns.

|Column	                 |Description|
|---                     |---        |
|`'name'	`            |Recipe name|
|`'id'`	                 |Recipe ID|
|`'minutes'`	         |Minutes to prepare recipe|
|`'contributor_id'`	     |User ID who submitted this recipe|
|`'submitted'`	         | Date recipe was submitted|
|`'tags'`	             |Food.com tags for recipe|
|`'nutrition'`	         |Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein    (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value”|
|`'n_steps'`	         |Number of steps in recipe|
|`'steps'`	             |Text for recipe steps, in order|
|`'description'`	     | User-provided description|
|`'ingredients'	`            |Recipe ingredients|
|`'n_ingredients'	`            |number of ingredients|

The columns that are relevant to our study are: `'name' ``'minutes'` `'n_steps'` and `'n_ingredients'`

In the RAW_interactions.csv dataset, there are 731,927 rows and 5 columns.

|Column|Description|
|---|---|
|`'user_id'`	|User ID|
|`'recipe_id'`	|Recipe ID|
|`'date'`	|Date of interaction|
|`'rating'`	|Rating given|
|`'review'`	|Review text|

The columns that are relevant to our study are `'rating'`.

---

## Cleaning and EDA

### Data Cleaning
First we left-merged the two datasets on recipe ID and filled all ratings of 0 with np.nan. We chose to make this decision because it’s impossible to differentiate between genuine ratings of 0 and users who chose not to rate a recipe, so for simplicity and easier data handling, anything with a rating of 0 will be considered NaN. If we had decided to continue with 0 as a valid rating, it may have skewed any analysis or statistics involving the ratings. We also replaced all values of 0 in the minutes, n_steps, and n_ingredients columns with NaN, because it is not logical to have a value of 0 for those columns. 

Then we grouped the merged dataframe by recipe id to find average ratings for each recipe and added it as a new column.

For the minutes column, there were many values in the tens of thousands and it really skewed any box plot and histograms we were trying to make, so we created a new dataframe with the middle 50% of the minutes columns filtered out. The resulting df_filtered has 83782 rows and 6 columns. This was the dataframe we continued with and conducted all of our analysis on.


The relevant columns for our research question are avg_rating, minutes,  n_steps, and n_ingredients, so we did not need to process the other columns. Below is the head of our final cleaned dataframe. 

| name                                 |     id |   minutes |   avg_rating |   n_steps |   n_ingredients |
|:-------------------------------------|-------:|----------:|-------------:|----------:|----------------:|
| 1 brownies in the world    best ever | 333281 |        40 |            4 |        10 |               9 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |            5 |        12 |              11 |
| 412 broccoli casserole               | 306168 |        40 |            5 |         6 |               9 |
| millionaire pound cake               | 286009 |       120 |            5 |         7 |               7 |
| 2000 meatloaf                        | 475785 |        90 |            5 |        17 |              13 |

---

### Univariate Analysis
To complete the univariate analysis, we removed the outliers from the dataset.

#### Distribution of Average Ratings
The following plot displays how many recipes recieved a certain rating. Based on the plot, we can see that majortiy of the recipes received a 5 star rating, as there is a singluar peak at the 5.0 rating mark. Additionally, the data is skewed left, indicating that majority of the recipes have a higher rating.

<iframe src="assets/avg_rating_hist.html" width=800 height=600 frameBorder=0></iframe>

#### Distribution of Cooking Steps
The plot displays how many recipes have a certain number of steps. Based on the plot, we can see that there is a peak around 10 steps, and the graph is skewed right as most recipes tend to have 20 steps or less.

<iframe src="assets/steps_hist.html" width=800 height=600 frameBorder=0></iframe>

#### Distribution of Cooking Time
From the boxplot, we can see that the minutes column has a median of around 33 with the IQR laying in between about 0 and 110. The data is skewed right, indicating that most recipes have lower cooking times.

<iframe src="assets/minutes_hist.html" width=800 height=600 frameBorder=0></iframe>

#### Distribution of Number of Ingredients
The histogram shows a slightly symmetric distribution. There is a peak around 7 or 8 ingredients, and a roughly equal amount of recipes with less than 7 ingredients and more than 7 ingredients.

<iframe src="assets/ingredients_hist.html" width=800 height=600 frameBorder=0></iframe>

---

### Bivariate Analysis

#### Number of Ingredients vs. Recipe Rating
When plotting the number of ingredients vs average rating, there is a slight positive trend, with higher density of points towards higher average ratings.

<iframe src="assets/ingredients_vs_rating.html" width=800 height=600 frameBorder=0></iframe>

#### Number of Steps vs. Rating
There is a slightly positive trend. As the rating increases, the number of ingredients also is positively correlated.

<iframe src="assets/steps_vs_rating.html" width=800 height=600 frameBorder=0></iframe>

---

### Interesting Aggregates
We were particularly interested in conducting a multidimensional analysis of this dataset to gain insights into the relationship between the number of ingredients, the number of steps, and their combined impact on the average rating of recipes. 

#### Recipe Count by Number of Ingredients and Number of Steps

From the pivot table, we can see that generally, recipes with more ingredients and more steps were more common.

|   1 |   2 |   3 |   4 |   5 |   6 |   7 |   8 |   9 |   10 |
|----:|----:|----:|----:|----:|----:|----:|----:|----:|-----:|
| nan |   2 |   1 |   3 |   1 | nan |   1 | nan |   1 |    1 |
|  61 |  87 | 113 | 101 |  79 |  70 |  42 |  43 |  35 |   26 |
| 155 | 284 | 367 | 308 | 296 | 229 | 169 | 154 |  95 |   76 |
| 209 | 466 | 591 | 556 | 523 | 473 | 409 | 289 | 218 |  180 |
| 177 | 448 | 625 | 703 | 756 | 747 | 711 | 547 | 439 |  335 |

#### Average Rating by Number of Ingredients and Number of Steps

We can see that recipes with less ingredients were more likely to have a higher rating. Majority of these recieps received a 5 star rating.

|         1 |       2 |       3 |       4 |       5 |         6 |       7 |         8 |       9 |      10 |
|----------:|--------:|--------:|--------:|--------:|----------:|--------:|----------:|--------:|--------:|
| nan       | 5       | 5       | 4.83333 | 4.25    | nan       | 4.95    | nan       | 5       | 5       |
|   4.64071 | 4.72059 | 4.71517 | 4.72326 | 4.58198 |   4.75968 | 4.73417 |   4.75043 | 4.58663 | 4.56398 |
|   4.7733  | 4.68695 | 4.66226 | 4.6613  | 4.64923 |   4.68736 | 4.62882 |   4.64639 | 4.67586 | 4.61296 |
|   4.69127 | 4.68823 | 4.64924 | 4.63692 | 4.64822 |   4.59635 | 4.6274  |   4.62915 | 4.57043 | 4.62855 |
|   4.67418 | 4.65714 | 4.6654  | 4.67729 | 4.64328 |   4.59967 | 4.64362 |   4.6591  | 4.63867 | 4.66933 |

---

## Assessment of Missingness

### NMAR Analysis
We think that the avg_rating column is NMAR because it is possible that those who weren't as satisfied with the recipes choose not to rate it aand add their opinion to the website.

---

### Missingness Dependency

We decided to examine the `avg_rating` column. We constructed permutation tests to determine the relationship. We have decided to test the dependency between the missingness of `avg_rating` with `minutes`.

We compare two distributions:

1. The distribution of 'minutes' when 'avg_rating' is missing.  
2. The distribution of 'minutes' when 'avg_rating' is not missing.
   
Null Hypothesis: The missingness of 'avg_rating' is independent of other columns.

Alternative Hypothesis: The missingness of 'avg_rating' is dependent on minutes column.

We created a new column indicating the missingness status of the `avg_rating`, and shuffled this column for our permutation. Since `minutes` is numeric, we used the absolute mean difference of `minutes` when `avg_rating` is and is not missing as our test statistics.

Below are distributions of minutes with and without rating. From the histogram below, we notice there distribution is very similar. But we will continue to conduct a permutation test with mean as test statistic.

<iframe src="assets/minutes_by_missingness_rating_hist_box.html" width=800 height=600 frameBorder=0></iframe>

Below shows the empirical distribution of our test statistics in 1000 permutations, the red line indicates the observed test statistics.

<iframe src="assets/missingness_tvd_distribution.html" width=800 height=600 frameBorder=0></iframe>

From the graph above and out permutation test, we get a p-value that's lower than our significance level of 5%, so we reject the null hypothesis.

**Therefore, we conclude that it is highly possible that the missingness of `avg_rating` *<u>does</u>* depend on `minutes` column.**

---
## Hypothesis Testing
### Permutation Test
We determined that a good way to test the truthfulness of the trend is to take the distribution at the start and end of the decade for comparison. Hence, we are going to conduct a permutation test on the difference between the ratings for recipes that are "short" and "long." We define "long" recipes to be greater than the found median, 33 minutes, and anything below 33 minutes will be considered "short."

**Null Hypothesis**: All recipes are rated the same.

**Alternative Hypotheses**: Long recipes are rated lower on average compared to short recipes. Recipes with more steps are rated lower on average compared to recipes with less steps. Recipes with more ingredients are rated lower on average compared to recipes with less ingredients.

**Test Statistics**: We are going to use <u>Mean Difference</u> as our test statistic.

**Significance Level**: To ensure the accuracy of our conclusion on the existence of changes in preference, we determined to use a significance level of 5% to increase accuracy of our randomized test result.

The plot below shows the empirical distribution of each of our test statistics in 500 permutations, the red line indicates the observed test statistics.

<iframe src="assets/minutes_permutation_hist.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/n_steps_permutation_hist.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/n_ingredients_permutation_hist.html" width=800 height=600 frameBorder=0></iframe>


The observed difference in average ratings between long and short recipes is 0.0296. However, the calculated p-value of 1.0 indicates that, under the null hypothesis of no difference in ratings between long and short recipes, the observed difference is not statistically significant. Therefore, we fail to reject the null hypothesis, suggesting that there is insufficient evidence to conclude that long recipes are rated lower on average compared to short recipes based on the given data. The results do not support the alternative hypothesis of a significant difference in ratings between long and short recipes. Hence, we **fail to reject our null hypothesis**.

## Conclusion:
We asked ourselves whether recipe length (in minutes), step count, and ingredient count affected the average rating of a recipe. Exploring the relationship between average ratings alongside other factors enabled us to understand what can affect an individual’s overall satisfaction of a meal. Here is a summary of the trends we found:

Recipe Length and Average Rating:
Our data analysis does not provide adequate evidence to support the notion that the length of a recipe significantly influences its average rating.

Step Count and Average Rating:
The results of our analysis indicate a meaningful relationship between the number of steps in a recipe and its average rating, suggesting that step count does impact the perceived quality of a recipe.

Number of Ingredients and Average Rating:
Conversely, our findings do not offer sufficient support for the idea that the number of ingredients is a significant predictor of a recipe's average rating.

