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
For this plot, we examined the distribution of the number of ingredients per recipe in the dataset. As shown above, the plot is roughly bell-shaped with a slight right skew. This indicates that the most common number of ingredients per recipe is 8, while some range all the way up to 37. 

### Bivariate Plot
<iframe
  src="assets/bivariate_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This scatter plot shows how the number of ingredients in a recipe impacts its average rating. Recipes with very few ingredients (<5) appear to have slightly lower ratings, while recipes with a moderate number of ingredients (around 8–10) often have the highest average ratings. In between these two ranges, adding more ingredients does not appear to increase ratings and may even correspond to a slight decrease, forming a roughly quadratic trend.

### Interesting Aggregates
put a pivot table!

## Assessment of Missingness
As previously mentioned, the columns that contain missing data are `name`, `description`, and `average_rating`. Let's do some exploration to determine why this may be! 

### MNAR Analysis 
We believe that the `description` column is MNAR because users are likely to report a rating description when they have strong feelings about the recipe, whether that be positive or negative. However, users are likely not to report a rating explanation when they have few or mediocre thoughts on the recipe. This missingness mechanism could be turned into MAR if we collected additional information, like whether the user viewed or cooked the recipe, and also how frequently users rate recipes. 

### Missingness Dependency
Next, we chose to explore the missingness mechanism of the `average_rating` column in the recipes dataset. More specifically, we investigated whether the missingness of the `average_rating` column is dependent on the `n_ingredients` column, which keeps track of the number of ingredients, or the `day_submitted` column, which reports which day of the month a review was published. 

> `average_rating` and `n_ingredients`

**Null Hypothesis:** The missingness of average_rating does not depend on the number of ingredients in the recipe.

**Alternate Hypothesis:** The missingness of average_rating does depend on the number of ingredients in the recipe.

**Test Statistic:** The absolute difference in means between `n_ingredients` in the distribution of the group without missing `average_rating` values and the distribution of the group without missing `average_rating` values. 

**Significance Level:** 0.05

To conduct this permutation test, we shuffled the missingness of `average_rating` 1000 times to collect 1000 mean differences between the two distributions as defined in the test statistic section.

**IMPORT A GRAPH**

The **observed statistic** of these two distributions is ~0.254 as shown by the vertical line in the graph above. The p-value determined from this permutation test is 0.0. As 0.0 < 0.05 (our significance level), we **reject the null hypothesis**, concluding that the missingness of `average_rating` is dependent on the `n_ingredients` column. 


> `average_rating` and `day_submitted`

**Null Hypothesis:** The missingness of average_rating does not depend on the day of the month the review was submitted.  

**Alternate Hypothesis:** The missingness of average_rating does depend on the day of the month the review was submitted

**Test Statistic:** The absolute difference in means between `day_submitted` in the distribution of the group without missing `average_rating` values and the distribution of the group without missing `average_rating` values. 

**Significance Level:** 0.05

To conduct this permutation test, we shuffled the missingness of `average_rating` 1000 times to collect 1000 mean differences between the two distributions as defined in the test statistic section.

**IMPORT A GRAPH**

The **observed statistic** of these two distributions is -0.065 as shown by the vertical line in the graph above. The p-value determined from this permutation test is 0.725. As 0.725 > 0.05 (our significance level), we **fail to reject the null hypothesis**, concluding that the missingness of `average_rating` is not dependent on the `day_submitted` column. 

## Hypothesis Testing
We've all heard the phrase *"butter makes everything better"* but how much better does butter truly make a recipe? Does it have any effect on the rating of a recipe, or is that phrase just a catchy line we've heard over and over again? Thus, for our hypothesis test, we choose to explore just that. 

**Null Hypothesis:** Recipes that include butter have equal ratings to recipes that do not include butter

**Alternate Hypothesis:** Recipes that include butter **do not** have equal ratings to recipes that do not include butter

**Test Statistic:** The absolute difference in means between the average rating of recipes with butter and without butter 

**Significance Level:** 0.05
We're using a permutation test as it directly tests whether the difference in average ratings between recipes with and without butter could occur by chance or if there is a likely relationship. Furthermore, we set up our hypotheses so that our null hypothesis assumes no difference between the two groups. We chose the difference in means as our test statistic, as it allows us to measure the relationship between the average rating and the presence of butter in a recipe. Finally, we used a significance level of 0.05 as it is the standard threshold for significance. 

To conduct this permutation test, we split up our recipe data set into 2 groups based on whether butter was in the `ingredients_lst` column or not. From this, we got an **observed value of 0.001**, and after shuffling 1000 times and recording our test statistic, we got a **p-value of 0.854**. 

### Hypothesis Test Conclusion
As the p-value found 0.854 is greater than our chosen significance level of 0.05, we **fail to reject the null hypothesis**. Thus, we conclude that we have not found enough evidence to confidently say that the difference in average rating of recipes with and without butter is not due to random chance alone. 

## Framing a Prediction Problem
For this section, we aim to predict the **average rating of a recipe** using features extracted from our `recipe` dataset. We have decided to approach this problem using multiclass classification by treating the `average_rating` of a recipe as ordinal data by converting each value into an integer ranging from values 1 to 5. 

We chose `average_rating` as our response variable as it is a good indicator for how a recipe the quality of a given recipe. Furthermore, the F1-score is our primary metric for our model because the dataset has `average_rating` mostly in the 4-5 range, creating a class imbalance. Though we can still use accuracy, we will not rely on it to measure the performance of our model, as it can be misleading due to these imbalanced classes. F1-score, on the other hand, is a better metric for our problem as it combines precision and recall, which makes it more robust to class imbalances. 

## Baseline Model

## Final Model

## Fairness Analysis
