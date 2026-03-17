# Predicting Recipe Score
Authors: Beliz Akbulut, Caroline McClung

## Introduction
Enjoying food is one of the few universal experiences nearly all living animals share. Though we all share this trait, the types of food we eat, how we cook them, and the combinations of food we eat together vary wildly. The ways we create our own recipes may be due to cultural customs, dietary restrictions, or personal preference, among other factors. But what makes a recipe good? More specifically, we want to research **what features of a recipe have the greatest influence on their rating**. To conduct this exploration, we will use a dataset scraped from food.com that contains a subset of recipes and reviews posted online since 2008. 

The first dataset, `recipe`, contains 83782 rows, 83782 of which are unique recipes, and has 10 columns recording the following information:

| Column             | Description                                                                                                                                                                                       |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `'name'`           | Recipe name                                                                                                                                                                                       |
| `'id'`             | Recipe ID                                                                                                                                                                                         |
| `'minutes'`        | Minutes to prepare recipe                                                                                                                                                                         |
| `'contributor_id'` | User ID who submitted this recipe                                                                                                                                                                 |
| `'submitted'`      | Date recipe was submitted                                                                                                                                                                         |
| `'tags'`           | Food.com tags for recipe                                                                                                                                                                          |
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`        | Number of steps in recipe                                                                                                                                                                         |
| `'steps'`          | Text for recipe steps, in order                                                                                                                                                                   |
| `'description'`    | User-provided description                                                                                                                                                                         |
| `'ingredients'`    | Text for recipe ingredients                                                                                                                                                                       |
| `'n_ingredients'`  | Number of ingredients in recipe                                                                                                                                                                   |

The second dataset, `interactions`, contains 731927 rows, each corresponding to a user's review on a given recipe. The columns included in `interactions` are:

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |

Through investigating which features of a recipe most influence rating, we gain insight into how one can improve their own recipes. Findings from this project are not only relevant on an individual level but also can be used by restaurants and/or businesses to improve their menu and overall increase customer satisfaction.

## Data Cleaning and Exploratory Data Analysis
To increase readability and overall usability for our research purposes, we have made the following modifications to the `recipe` and `interactions` datasets.  
1. Merge the two datasets
   - Left merged the recipes dataset with the ratings dataset.
   - Filled all ratings of 0 with np.nan.
   - Found the average rating per recipe, as a Series, and added this series into the       dataset as a new column `average_rating`.
2. Converted the `steps`, `ingredients`, `nutrition`, and `tags` columns into lists
   - Though these columns appear to already contain lists, they are actually strings.
     Thus, we created a helper function `string_to_list` to convert them into the list      datatype.
   - These transformations are represented in `recipes` as columns named `steps_lst`,       `ingredients_lst`, `nutrition_lst`, and `tags_lst` respectively. Furthermore, the      original columns for this data are dropped.
  
After cleaning up our dataset, we observed that 3 columns appear to have missing data, these being:
1. `name`: 1 missing value
2. `description`: 70 missing values
3. `average_rating`: 2609 missing values

Overall, our recipes dataset now has 83782 rows and 15 columns. Embedded below is the head of our dataframe, including only the relevant columns and truncated values due to the size of `recipe`. 

| name                                 |   minutes |   average_rating |   n_ingredients | ingredients_lst         | steps_lst               | tags_lst                | description             |
|:-------------------------------------|----------:|-----------------:|----------------:|:------------------------|:------------------------|:------------------------|:------------------------|
| 1 brownies in the world    best ever |        40 |                4 |               9 | bittersweet chocolat... | heat the oven to 350... | 60-minutes-or-less,...  | these are the most;...  |
| 1 in canada chocolate chip cookies   |        45 |                5 |              11 | white sugar, brown s... | pre-heat oven the 35... | 60-minutes-or-less,...  | this is the recipe t... |
| 412 broccoli casserole               |        40 |                5 |               9 | frozen broccoli cuts... | preheat oven to 350...  | 60-minutes-or-less,...  | since there are alre... |
| millionaire pound cake               |       120 |                5 |               7 | butter, sugar, eggs     | freheat the oven to...  | time-to-make, course... | why a millionaire po... |
| 2000 meatloaf                        |        90 |                5 |              13 | meatloaf mixture, un... | pan fry bacon, and s... | time-to-make, course... | ready, set, cook! sp... |

### Univariate Plot
<iframe
  src="assets/univariate_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Plot
<iframe
  src="assets/bivariate_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
