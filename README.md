# Recipes & Ratings Analysis

## Introduction

In this project, I used two datasets (*recipes* and *interactions*) that contain all recipes and ratings records from 2008 to 2018. The datasets were directly scraped from [Food.com](https://www.food.com/), a website that ranks in the [top 30](https://www.similarweb.com/top-websites/food-and-drink/cooking-and-recipes/) of the world's most-visited cooking sites. The *recipes* dataset contains 83782 rows and 12 columns, each row corresponding to recipe. The *interactions* dataset contains 731927 rows and 5 columns, each row corresponding to a viewer-submitted review about a recipe.

My analysis is centered around the question: ==What are the characteristics of high rating recipes?== Specifically, I analyzed how cooking time, cooking complexity, preparation difficulty and calorie level may affect the average ratings of recipes. These four characteristics are related to these columns: (in *recipes* ,) `'minutes'`, `'nutriton'`, `'n_steps'`, `'n_ingredients'`, and (in *ingredients* ,) `'rating'`. Below are a detailed description of each relative column:

My analysis is centered around the question: <mark>What are the characteristics of high rating recipes?</mark> Specifically, I analyzed how cooking time, cooking complexity, preparation difficulty and calorie level may affect the average ratings of recipes. These four characteristics are related to these columns: (in *recipes* ,) `'minutes'`, `'nutriton'`, `'n_steps'`, `'n_ingredients'`, and (in *ingredients* ,) `'rating'`. Below are a detailed description of each relative column:

| Column Name | Description |
| --- | --- |
| `'minutes'`| Minutes to prepare recipe |
| `'nutrition'` | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'` | Number of steps in a recipe |
| `'n_ingredients'` | Number of ingredients in a recipe |
| `'rating'` | Rating given by a viewer |

Understanding the question could help a potential submitter to better predict future ratings of his/her recipes. Also, high rating corresponds to a higher public preference. When building a recommender system, researchers may reference to this analysis to have a general sense of what recipes types might have a higher popularity. 

## Cleaning and EDA

### Data Cleaning

- **Merging**: In order to calculate the average ratings per recipe, I left merged the *recipe* dataset and the *interaction* dataset. Since one can only rate between 1-5 on Food.com, value 0 in the `'rating'` column acutually means 'missing', so I replaced it with `np.NaN`.

- **Data type conversion**: Since all the data was scraped from the website, list-like variables like `'nutrition'` and `'tags'` were represented as strings. I converted these variables to lists.

- **Adding additonal columns**: I splitted the `'nutriton'` column and added a new column `'calories'` to the dataset since it's key variable in my analysis.

- **Consistently "incorect" values**: I noticed that the `'tags'` column contained values like '' and 'less_thansql:name_topics_of_recipegreater_than' which were equivalent to missing values, similiar to the `'description'` column. I replaced them with `np.NaN`.

- **Unreasonable outliers**: I noticed some unreasonable data in `'calories'` and `'minutes'`. Specifically, there were recipes that were more than 10000 calories. There were also recipes that took more than 43200 minutes (a month) but didn't explicitly mention the word "month" in its detailed steps (for example, "*after 6 months you can...*" would be a valid input). I deleted these rows from the dataset as they account for less than 0.05% of all recipes.

Below are the first three rows of my cleaned Dataframe: (`'steps'` column, which contains the specific steps for each recipe, is ignored here due to capacity issues)

|     id | name                                 |   minutes | tags                                                                                                                                                                                                                        | nutrition                             |   n_steps | description                                                                                                                                                                                                                                                                                                                                                                       |   n_ingredients |   average_rating |   calories | level   |   rating_bin |
|-------:|:-------------------------------------|----------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------|----------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------------:|-----------:|:--------|-------------:|
| 333281 | 1 brownies in the world    best ever |        40 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings'] | [10.0, 50.0, 3.0, 3.0, 19.0, 6.0]     |        10 | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              |               9 |                4 |      138.4 | normal  |            4 |
| 453467 | 1 in canada chocolate chip cookies   |        45 | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                               | [46.0, 211.0, 22.0, 13.0, 51.0, 26.0] |        12 | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            |              11 |                5 |      595.1 | good    |            5 |
| 306168 | 412 broccoli casserole               |        40 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | [20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 |               9 |                5 |      194.8 | good    |            5 |


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
|            2 |   94.7684 |  10.7997  |         9.22848 |    462.408 |
|            3 |   97.5438 |   9.95245 |         9.12667 |    437.183 |
|            4 |  111.997  |   9.99707 |         9.34799 |    427.n305 |
|            5 |  112.139  |  10.0653  |         9.1802  |    426.219 |


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








