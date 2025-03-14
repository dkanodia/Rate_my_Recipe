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
