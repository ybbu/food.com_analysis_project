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

## Assesment of Missingness

## Hypothesis testing


