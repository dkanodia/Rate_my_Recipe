# Rate My Recipe
Authors: Divyansh Kanodia

## Introduction

Food plays a pivotal role in our daily lives, influencing our health, culture, and emotions. Cooking, for many, is not only a necessity but a fulfilling hobby that brings creativity and joy to the kitchen. However, as much as we enjoy flavorful meals, we also need to consider the nutritional impact of the food we consume. With the rising concerns about health and diet, understanding how specific ingredients or nutrients affect our food preferences has become more important than ever. This is particularly true for recipes that vary in their nutritional content, such as sugar, fat, and protein.

The first dataset, `recipe`, contains 83782 rows, indicating 83782 unique recipes, with 10 columns recording the following information:

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

The second dataset, `interactions`, contains 731927 rows and each row contains a review from the user on a specific recipe. The columns it includes are:

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |

In this project, I am investigating how recipe characteristics such as nutritional content, preparation time, and ingredient count influence a recipe’s average rating. My central question is: What factors most significantly influence a recipe's average rating? This question is important as it provides insights into the preferences of users on recipe platforms and could help food creators, nutritionists, and chefs design recipes that meet popular demand.

## Data cleaning and analysis

1. To combine the recipe information with user interactions (ratings), I performed a left join on the id column from the recipes dataset and the recipe_id column from the interactions dataset. This ensures that all recipe data is preserved while adding the corresponding ratings.
- This merging step ensures that we have both recipe details and their associated ratings in a single dataframe, which is necessary for analyzing the relationship between recipe characteristics and user ratings.

2. Handling Missing Ratings

- Ratings of 0 are replaced with NaN, as they likely represent missing or invalid ratings. This allows for more accurate analysis since we are not treating these zeros as valid ratings. Replacing 0 with NaN ensures that missing ratings are properly handled, preventing misleading results from invalid values. We can now focus on actual user ratings.
  
3. Calculating Average Ratings

- To calculate the average rating for each recipe, I grouped the merged dataset by id (the recipe identifier) and computed the mean of all the ratings for each recipe. The average rating is important for understanding the overall reception of a recipe. It helps in assessing the general quality or popularity of a recipe.

4. Merging Average Ratings with the recipes Dataset

- I merged the calculated average ratings back into the original recipes dataset so that each recipe contains its respective average rating. The column was renamed from rating to average_rating for clarity. This ensures that the recipes dataset now includes a column for average_rating, which is essential for analyzing the relationship between the recipe's characteristics (such as nutritional content and complexity) and user preferences.
Dropping Unwanted Columns

5. Several columns were dropped from the recipes dataset as they were either irrelevant for the analysis or non-numeric features that could add unnecessary complexity.
   - Removing these columns made the dataset more focused and relevant for the analysis. This allows us to work with the essential features such as nutritional information, number of ingredients, and user ratings.

6. Filling Missing Values in the average_rating Column

- The nutrition column contained a single string with multiple nutritional values. I split this into separate columns for each nutrient, such as calories, sugar, and protein, for easier analysis. This allows each nutritional value to be treated as a separate feature, which is essential for analyzing the influence of specific nutrients on user ratings.

7. Adding the simple recipes Column
- I created a binary column, simple recipes, to categorize recipes as either simple (with 9 or fewer steps) or complex (with more than 9 steps).
Impact: This new feature provides insight into whether recipe complexity (measured by the number of steps) influences user ratings, which is important for understanding user preferences.

8. Converting the submitted Column to DateTime Format

- The submitted column contains dates in string format, so I converted it to a datetime object for easier handling in any time-based analysis. This allows us to conduct time-based analyses (e.g., exploring trends in recipe ratings over time) and helps ensure consistency when working with dates.

## Univariate Analysis

The histogram below shows the distribution of the number of ingredients used in the recipes. The x-axis represents the number of ingredients, while the y-axis indicates the frequency of recipes with that specific number of ingredients.

Interpretation: The distribution is right-skewed, with most recipes containing around 5 to 12 ingredients. There are fewer recipes with a very high number of ingredients, suggesting that simpler recipes with fewer ingredients are more common in the dataset. This trend aligns with general cooking practices, where most dishes require a moderate number of ingredients rather than an excessive amount.

The pie chart below represents the distribution of average recipe ratings in the dataset. The different segments correspond to different rating values, and the percentage labels indicate the proportion of recipes that received each rating.

Interpretation:
A majority of recipes have an average rating of 5, making up 57% of the dataset, followed by 4-star ratings at 33.5%. Lower ratings (0, 1, 2, and 3) collectively account for a much smaller percentage of the dataset. This suggests that users tend to rate recipes positively, which could indicate a preference for rating only recipes they liked or potential bias in the rating system.

## Bivariate Analysis
This box plot visualizes the relationship between the number of ingredients in a recipe and its average rating. The median number of ingredients appears to be consistent across all ratings and recipes with very high or very low ratings tend to have a wider range of ingredient counts.

Interpretation:
The number of ingredients does not have a strong impact on user ratings. Moreover, both simple and complex recipes can be rated highly or poorly. There are also a significant number of outliers, especially in recipes with high ingredient counts.

This box plot visualizes the relationship between the length of description of a recipe and its average rating. The length appears to be consistent across all ratings and recipes with very high tend to have a wider range of ingredient counts.

Interpretation:
The length of description may have an effect on the average rating. Since the rating with higher average length tends to be higher. 

## Interesting Aggregates

1. Caloric Trends & Ratings:

- Recipes with 0-star ratings tend to have the highest average calorie count (514.55 kcal), suggesting that high-calorie recipes might not always be favored.
- 4-star rated recipes have the lowest average calories (412.05 kcal), implying that moderately healthy recipes might be preferred.
- 5-star recipes have slightly higher calories (434.25 kcal), indicating that people may still enjoy slightly richer dishes.

2. Recipe Description Length & Ratings:
- The average description length does not vary significantly across ratings.
- However, 0-star recipes have the longest average descriptions (236.03 characters), suggesting that long descriptions do not guarantee better ratings.
- The median description length remains around 170-177 characters, reinforcing that a concise but informative description is a common trait.

## NMAR Analysis

The dataset has missing columns of `average_ratings`, `description` and `name`. We drop the column of name since it does not have any information.

- `average_rating` is missing for some recipes. If a recipe has no ratings, it's likely because no one has reviewed it yet rather than randomness or dependency on another variable. 
- This suggests that the missingness is directly related to the column's nature: a recipe with missing ratings is likely an unpopular or newly added recipe. Thus `average_rating` is likely NMAR because its missingness is tied to the lack of user engagement.

  The goal of these tests was to determine whether the missingness in the average_rating column is dependent on the minutes and n_ingredients columns.

## Results for minutes
- The observed difference in means: 117.34 minutes
- The computed p-value: 0.0520
- Since the p-value is slightly above the conventional 0.05 threshold, we fail to reject the null hypothesis. This suggests that missingness in average_rating is not significantly dependent on minutes. However, given that the p-value is very close to 0.05, there may still be some weak association worth exploring further.

## Results for n_ingredients
- The observed difference in means: 0.2542
- The computed p-value: 0.0020
- The p-value is well below 0.05, so we reject the null hypothesis. This indicates that the missingness in average_rating is significantly dependent on n_ingredients. This suggests that recipes with a certain number of ingredients might be more likely to have missing ratings.

## Hypotheses test: Effect of Recipe Complexity on Average Rating

- Null Hypothesis (H₀): The number of steps in a recipe does not affect its average rating.
- Alternative Hypothesis (H₁): Recipes with more steps have a different average rating than simpler recipes.

Test Statistic and Significance Level:

- Test Statistic: Absolute difference in means between simple recipes (fewer than 9 steps) and complex recipes (9 or more steps). This is a good choice - because we are comparing two group means, and the absolute difference allows us to detect both increases and decreases in ratings.
- Significance Level (α): 0.05, which is a common threshold in hypothesis testing to determine statistical significance.

Results:
- Observed Test Statistic: Calculated from the dataset
- Permutation Test p-value: 0.0010

Conclusion: Since the p-value is much lower than 0.05, we reject the null hypothesis. This suggests that the number of steps in a recipe is likely associated with differences in average rating.

Justification for Choices:

- Permutation Test: This non-parametric approach is appropriate because it does not assume normality and allows for a direct comparison of observed differences under random permutations.
- Absolute Difference in Means: Captures deviations in either direction, making the test more flexible.
- Significance Level (0.05): A standard threshold that balances Type I and Type II errors.
