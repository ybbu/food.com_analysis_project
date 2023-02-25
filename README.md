# Recipes & Ratings Analysis

## Introduction

In this project, I used two datasets (*recipes* and *interactions*) that contain all recipes and ratings records from 2008 to 2018. The datasets were directly scraped from [Food.com](https://www.food.com/), a website that ranks in the [top 30](https://www.similarweb.com/top-websites/food-and-drink/cooking-and-recipes/) of the world's most-visited cooking sites. The *recipes* dataset contains 83782 rows and 12 columns, each row corresponding to recipe. The *interactions* dataset contains 731927 rows and 5 columns, each row corresponding to a viewer-submitted review about a recipe.

My analysis is centered around the question: **What are the characteristics of high rating recipes?** Specifically, I analyzed how cooking time, cooking complexity, preparation difficulty and calorie level may affect the average ratings of recipes. These four characteristics are related to these columns: (in *recipes* ,) `'minutes'`, `'nutriton'`, `'n_steps'`, `'n_ingredients'`, and (in *ingredients* ,) `'rating'`. Below are a detailed description of each relative column:

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

Below is the `head` of my cleanded Dataframe:

|     id | name                                 |   minutes | submitted   | tags                                                                                                                                                                                                                                                                                               | nutrition                                     |   n_steps | description                                                                                                                                                                                                                                                                                                                                                                       |   n_ingredients |   average_rating |
|-------:|:-------------------------------------|----------:|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------|----------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------------:|
| 333281 | 1 brownies in the world    best ever |        40 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings']                                                                        | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]      |        10 | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              |               9 |                4 |
| 453467 | 1 in canada chocolate chip cookies   |        45 | 2011-04-11  | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                                                                                                      | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0]  |        12 | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            |              11 |                5 |
| 306168 | 412 broccoli casserole               |        40 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                                                                                               | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]     |         6 | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 |               9 |                5 |
| 286009 | millionaire pound cake               |       120 | 2008-02-12  | ['time-to-make', 'course', 'cuisine', 'preparation', 'occasion', 'north-american', 'desserts', 'american', 'southern-united-states', 'dinner-party', 'holiday-event', 'cakes', 'dietary', 'christmas', 'thanksgiving', 'low-sodium', 'low-in-something', 'taste-mood', 'sweet', '4-hours-or-less'] | [878.3, 63.0, 326.0, 13.0, 20.0, 123.0, 39.0] |         7 | why a millionaire pound cake?  because it's super rich!  this scrumptious cake is the pride of an elderly belle from jackson, mississippi.  the recipe comes from "the glory of southern cooking" by james villas.                                                                                                                                                                |               7 |                5 |
| 475785 | 2000 meatloaf                        |        90 | 2012-03-06  | ['time-to-make', 'course', 'main-ingredient', 'preparation', 'main-dish', 'potatoes', 'vegetables', '4-hours-or-less', 'meatloaf', 'simply-potatoes2']                                                                                                                                             | [267.0, 30.0, 12.0, 12.0, 29.0, 48.0, 2.0]    |        17 | ready, set, cook! special edition contest entry: a mediterranean flavor inspired meatloaf dish. featuring: simply potatoes - shredded hash browns, egg, bacon, spinach, red bell pepper, and goat cheese.                                                                                                                                                                         |              13 |                5 |


### Univariate Analysis

<iframe src="assets/uni_rating.html" width=800 height=600 frameBorder=0></iframe>

## Assesment of Missingness

## Hypothesis testing


