# Recipes & Ratings 

### Introduction

In this project, I used two datasets (*recipes* and *interactions*) that contain all recipes and ratings records from 2008 to 2018. The datasets were directly scraped from [Food.com](https://www.food.com/), a website that ranks in the [top 30](https://www.similarweb.com/top-websites/food-and-drink/cooking-and-recipes/) of the world's most-visited cooking sites. The *recipes* dataset contains 83782 rows and 12 columns, each row corresponding to recipe. The *interactions* dataset contains 731927 rows and 5 columns, each row corresponding to a viewer-submitted review about a recipe.

My analysis is centered around the question: <mark>What are the characteristics of high rating recipes?</mark> Specifically, I analyzed how cooking time, cooking complexity, preparation difficulty and calorie level may affect the average ratings of recipes. These four characteristics are related to these columns: (in *recipes* ,) `'minutes'`, `'nutriton'`, `'n_steps'`, `'n_ingredients'`, and (in *ingredients* ,) `'rating'`. Below are a detailed description of each relative column:

| Column Name | Description |
| --- | --- |
| `'minutes'`| Minutes to prepare recipe |
| `'nutrition'` | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'` | Number of steps in a recipe |
| `'n_ingredients'` | Number of ingredients in a recipe |
| `'rating'` | Rating given by a viewer |


Understanding the question could help a potential submitter to better predict future ratings of his/her recipes. Also, high rating corresponds to a higher public preference. When building a recommender system, researchers may reference to this analysis to have a general sense of what recipes types might have a higher popularity. 

# Analysis 

## Cleaning and EDA

### Data Cleaning

- **Merging**: In order to calculate the average ratings per recipe, I left merged the *recipe* dataset and the *interaction* dataset. Since one can only rate between 1-5 on Food.com, value 0 in the `'rating'` column acutually means 'missing', so I replaced it with `np.NaN`.

- **Data type Ccnversion**: Since all the data was scraped from the website, list-like variables like `'nutrition'` and `'tags'` were represented as strings. I converted these variables to lists.

- **Adding additonal columns**: I splitted the `'nutriton'` column and added a new column `'calories'` to the dataset since it's key variable in my analysis.

- **Consistently "incorect" values**: I noticed that the `'tags'` column contained values like '' and 'less_thansql:name_topics_of_recipegreater_than' which were equivalent to missing values, similiar to the `'description'` column. I replaced them with `np.NaN`.

- **Unreasonable outliers**: I noticed some unreasonable data in `'calories'` and `'minutes'`. Specifically, there were recipes that were more than 10000 calories. There were also recipes that took more than 43200 minutes (a month) but didn't explicitly mention the word "month" in its detailed steps (for example, "*after 6 months you can...*" would be a valid input). I deleted these rows from the dataset as they account for less than 0.05% of all recipes.

Below is the `head` of my cleaned Dataframe:

<iframe src="assets/recipes_head.html" width=700 height=500 frameBorder=0></iframe>

### Univariate Analysis
<iframe src="assets/uni_rating.html" width=800 height=600 frameBorder=0></iframe>
Above is a probabaility density histogram of average ratings for all recipes. Highly skewed to the left, the plot shows that most of the recipes were highly-rated. Specifically, around 75% of the recipes were rated between 4.5 ~ 5.

### Bivariate Analysis
<iframe src="assets/bi_time_rating.html" width=800 height=600 frameBorder=0></iframe>
Above is a plot showing the distribution of cooking time acorss different average rating groups. Based on the shapes of the histogram we can tell that the time distributions are not similiar across groups. The box plot above further shows that the cooking time of recipes rated between 0 ~ 1 ranges wider while the cooking time of recipes between 1 ~ 2, 2 ~ 3, 3 ~ 4 share similiar range.

### Interesting Aggregates

|   rating_bin |   minutes |   n_steps |   n_ingredients |   calories |
|-------------:|----------:|----------:|----------------:|-----------:|
|            1 |   95.9151 |  10.489   |         9.04924 |    445.615 |
|            2 |   92.6442 |  10.8072  |         9.23824 |    442.597 |
|            3 |   97.5383 |   9.94796 |         9.12602 |    426.79  |
|            4 |  103.584  |   9.99283 |         9.3469  |    423.95  |
|            5 |   91.951  |  10.0604  |         9.17896 |    418.8   |


Above is a table grouped by rating bins (1, 2, 3, 4, 5). Each column represents the mean value of the distribution. Based on the data, we can see that the means of cooking time (`'minutes'`), cooking complexity (`'n_steps'`), and preparation difficulty (`'n_ingredients'`) don't vary much across different groups. However, for the calorie level, it seems that lower-rated recipes tend to have more calories then high-rated ones.


## Assesment of Missingness

### NMAR Analysis

Three columns in the dataset have missing values: `'average_rating'`, `'description'`, `'tags'`. I believe the column `'description'` is NMAR (Not Missing At Random).

Submitters often include the background information of the recipe or their experiences of desgning the recipe. If a submitter does not have anything particular to say about the recipe, he/she may leave it blank. Therefore, the chance that a value in `'description'` is missing depends on the value itself (e.g. nothing specific to put in the description).

### Missingness Dependency

To test whether the missingness in `'average_rating'` depends on `'minutes'`, I ran a permutation testing. Since the distributions of cooking time (`'minutes'`) when values in `'average_rating'` are missing and when values in `'average_rating'` are not missing have similiar means but different shapes, I used the Kolmogorov-Simrnov test statistic. 

The resulted p-value is 0.0, which is less 0.5 (significance level). Therefore, I reject the null hypothesis, which suggests that the missingness in `'average_rating'` **does** depend on `'minutes'`. And based on the distribution of test statistic, the `'average_rating'` tends to be missing more when the cooking time is lower. One possible explanation could be that recipes that require less cooking time tend to be easier. People may not bother to rate an easy recipe they don't put much effort in. 

Below is a plot visualizing the permutation testing:
<iframe src="assets/missing_p.html" width=800 height=600 frameBorder=0></iframe>


## Hypothesis testing

Recalling, my research question is **What are the characteristics of high rating recipes?** 

Specifically, I tested on 4 hypothesis with permutation testing: 

1. Is there a relationship between the cooking time and average rating of recipes?
    (I define cooking time <= 45 minutes to be normal, cooking time > 45 minutes to be long.)
    - Null hypothesis: Cooking time doesn't affect average rating of the recipes.
    - Alternative hypothesis: Longer cooking time is related to lower average ratings.
2. Is there a relationship between the number of steps and average rating of recipes?
    (I define number of steps <= 12 to be normal, steps > 12 to be many.)
    - Null hypothesis: Number of steps doesn't affect average rating of the recipes.
    - Alternative hypothesis: More steps is related to lower average ratings.
3. Is there a relationship between the number of ingredients and average rating of recipes?
    (I define number of ingredients <= 9 to be normal, ingredients > 9 to be  many.)
    - Null hypothesis: Number of ingredients doesn't affect average rating of the recipes.
    - Alternative hypothesis: More ingredients is related to lower average ratings.
4. Is there a relationship between the calories and average rating of recipes?
    (I define calories <= 700 to be normal, calories > 700 to be  many.)
    - Null hypothesis: Calories doesn't affect average rating of the recipes.
    - Alternative hypothesis: More calories is related to lower average ratings.


Since the dependent variable - average rating - is a numeric variable, I used difference in group means as test statistic. THe significance level is 5%.

Below are the results for each of the hypothesis:
1. Cooking time vs Average time
	- p value: 0.0
	- conclusion: Since the p value is less than 0.05, I reject the null hypoethesis. The result suggests that recipes that require longer cooking time tend to be associated with lower average ratings.
2. Number of steps vs Average time
	- p value: 0.177
	- conclusion: Since the p value is larger than 0.05, I fail to reject the null hypoethesis. The result suggests that recipe complexity doesn't have an effect on the recipe's average rating.
3. Number of ingredients vs Average time
	- p value: 0.918
	- conclusion: Since the p value is larger than 0.05, I fail to reject the null hypoethesis. The result suggests that preparation difficulty doesn't have an effect on the recipe's average rating.
4. Calories vs Average time
	- p value: 0.383
	- conclusion: Since the p value is larger than 0.05, I fail to reject the null hypoethesis. The result suggests that calorie level doesn't have an effect on the recipe's average rating.

# Model building

Based on the analysis abolve, I built a regressor model to **predict the average rating of a recipe**

### Breaking Down

The response variable for my prediction problem is simply what I want to predict - the average rating calculated from all reviews, which is a continuous, quantitative variable. 

The metric I would use to evaluate my model's performance would be RMSE(root mean squared error). I chose this metric because it is commonly used to evaluate how well a regressor model is able to predict accurately. Specifically, RMSE measures the average differences between the predicted values and the target values, so my goal in this project is to build a regressor model that minimizes RMSE.

The goal of the project is to help submitters to estimate the ratings of their recipes before the recipes are posted, so I won't use information from the reviews as they are not yet available to the submitters at that moment.

## Baseline Model

First of all, I created a baseline model using <mark>Decision Tree regressor</mark>. The features I used are the recipe's cooking time (`minutes`), the number of steps in the recipe (`n_steps`), the number of ingredients to prepare (`n_ingredients`), and the number of calories in the recipe (`calories`). All four features are quantitative, discrete variables. For the baseline model, I didn't perform any encodings.

*(Note: I chose the Decision Tree regressor because based on my previous exploratory data analysis, the features don't quite share a linear relationship with the response variable, which is average rating. Also, there are large outliers in some of the features like `minutes` which would strongly influence the best fitting line of a regressor model. Therefore, I used a nonlinear regressor model that's robust to outliers in this problem, which is the Decision Tree regressor)*

### Model Performance

The performance of the baseline model, in terms of RMSE, on the training set is 0.027 (rounded to 3 decimal places). On the test data, the performance, in terms of RMSE, is around **0.944** (rounded to 3 decimal spaces).

Overall, the baseline model is not too bad because the RMSE values for both the training set and test set are below 1, which is quite small. However, the large difference in the model's performance on the training set and test set shows that the model is overfitting to the training data and generalizes poorly to unseen data.

In future steps, my primary goal would be to improve the model's performance on unseen data.

## Final Model

### Feature Encoding

**Quantile Transformation:**

The first thing I did was to transform all the quantitative variables (`n_steps`, `minutes`, `n_ingredients`, and `calories`) with quantile information. I did this for two reasons. 
- First, the quantitative variables have very different ranges. For example, the values in `calories` range from 0 to 45609 while the values in `n_ingredients` only range from  1 to 37. In this case, the `calories` will automatically carry more weightage than the `n_steps`. Using quantile transformation will bring all quantitative variables to the same scale so they will contribute equally to the analysis. 
- Secondly, both`calories` and `minutes` have very large outliers which would stay large even after z-score standardization. Quantile transformation can solve this problem because it performs a nonlinear transformation by squeezing all values into 0 to 1.

**New Categorical Feature:**

The second thing I did was encode a new feature about the recipe's course category from the `tag` column in the dataset. I added this feature because users may have different rating standards for different types of recipes. For example, users may value the number of calories most when rating a salad recipe. When it comes to rating a breakfast recipe, users may focus more on the number of steps instead.

Since each recipe may be labeled with multiple course categories, I used a MultiLaber Binarizer to encode the feature.

### Model Fine-tuning

I decided to use the model I used in the baseline model for my final model due to the reasons stated in the Baseline section. 

To fine-tune the model, I used 5-fold cross-validation and performed a grid search to find the combination of hyperparameters that maximizes average validation accuracy. The hyperparameters I chose to fine-tune were:
- *splitter*: strategy used to choose the split at each node
- *max_features*: the number of features to consider when looking for the best split
- *max_depth*: the maximum depth of the tree
- *min_samples_split*: the minimum number of samples required to split an internal node
All of the hyperparameters manipulate the complexity of the model and therefore affect the model's generalizability to unseen data. 

The best combinations of hyperparameters are: `splitter='random', max_features=None, max_depth=100, min_samples_split=0.5`. Using those hyperparameters, the performance of my final model, in terms of RMSE, on the training set is 0.638 (rounded to 3 decimal places). On the test data, the performance, in terms of RMSE, is around **0.641** (rounded to 3 decimal spaces). 

To conclude, though the final model performs worse on training sets as compared to the baseline model, its performance on the test set increases. And more importantly, the difference in the model's performance on the training set and test set shrinks from 0.917 to 003. This is a large improvement because, unlike the baseline model, my final model performs nearly equally well on the test set and training set, showing its good generalizability to unseen data.

## Fairness Analysis

Here I performed a fairness analysis of my final model's performance, in terms of RMSE, in recipes with different calorie levels. I artificially defined the threshold for being a high-calorie recipe to be 780. Specifically, my hypothesis is as follows:

- **Null Hypothesis**: My model is fair. Its RMSE for high-calorie recipes (#calorie > 800) and normal (#calorie <= 800) recipes are roughly the same, and any differences are due to random chance.
- **Alternative Hypothesis**: My model is unfair. Its RMSE for high-calorie recipes (#calorie > 800) is different from its RMSE for normal recipes (#calorie <= 800).

My choice of test statistic was the absolute RMSE difference between the two groups. And my significance level is 0.05.

### Conclusion

The resulting p-value is 0.011. A visualization of the permutation test is as follows: 
<iframe src="assets/test.html" width=800 height=600 frameBorder=0></iframe>
To conclude, since the p-value is less than the significance level, I reject the null hypothesis. The result suggests that my final model is not fair. It appears to be performing differently for high-calorie recipes and normal recipes. 









