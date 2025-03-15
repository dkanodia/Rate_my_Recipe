# Rate My Recipe

**Author**: Divyansh Kanodia

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

This project investigates how recipe characteristics, such as nutritional content, preparation time, and ingredient count, influence a recipe’s average rating. The central question is: What factors most significantly influence a recipe's average rating?

## Data Cleaning and Analysis

1. **Merging Recipe Data with User Interactions:**
   - The `recipe` dataset is joined with the `interactions` dataset based on the `id` and `recipe_id` columns to combine recipe details with their corresponding ratings.

2. **Handling Missing Ratings:**
   - Ratings of 0 are replaced with NaN to treat them as missing or invalid ratings for better accuracy.

3. **Calculating Average Ratings:**
   - The average rating for each recipe is calculated by grouping the merged dataset by `id` and computing the mean of all ratings for each recipe.

4. **Merging Average Ratings:**
   - The calculated average ratings are merged back into the original recipes dataset to provide each recipe with its corresponding average rating.

5. **Dropping Unwanted Columns:**
   - Irrelevant or non-numeric columns are removed to streamline the analysis.

6. **Handling Missing Values in `average_rating`:**
   - The `nutrition` column is split into separate nutrient columns, and missing values in `average_rating` are handled appropriately.

7. **Adding Simple Recipes Column:**
   - A binary column `simple recipes` is created to classify recipes based on their complexity (simple with ≤9 steps, complex with >9 steps).

8. **DateTime Conversion:**
   - The `submitted` column is converted to DateTime format and the year is extracted to enable easier handling of time-based analysis.

9. **Adding `description_len`:**
   - A `description_len` column is added to the relationship between description and average_rating better.

The table shows the first 5 rows of the dataset after cleaning

| `minutes` | `submitted` | `n_steps` |`n_ingredients` | `average_rating` | `calories` | `total fat (PDV)` | `sugar (PDV)` | `sodium (PDV)` | `protein (PDV)` | `saturated fat (PDV)` | `carbohydrates (PDV)` | `simple recipes` | `description_len` |
|-----------|-------------|-----------|----------------------------------------------|-----------------|------------------|------------|-------------------|----------------|-----------------|------------------|-----------------------|-----------------------|------------------|-------------------|------------------|
| 40        | 2008        | 10        | 9               | 4                | 138.4      | 10.0              | 50.0           | 3.0             | 3.0              | 19.0                  | 6.0                   | 1                | 260                              |
| 45        | 2011        | 12         | 11              | 5                | 595.1      | 46.0              | 211.0          | 22.0            | 13.0             | 51.0                  | 26.0                  | 0                | 230                             |
| 40        | 2008        | 6         | 9               | 5                | 194.8      | 20.0              | 6.0            | 32.0            | 22.0             | 36.0                  | 3.0                   | 1                | 369                             |
| 120       | 2008        | 7         | 7               | 5                | 878.3      | 63.0              | 326.0          | 13.0            | 20.0             | 123.0                 | 39.0                  | 1                | 210                             |
| 90        | 2012        | 17        | 13              | 5                | 267.0      | 30.0              | 12.0           | 12.0            | 29.0             | 48.0                  | 2.0                   | 0                | 201          |



## Univariate Analysis

- **Number of Ingredients Distribution:**
   - The histogram reveals a right-skewed distribution of the number of ingredients, with most recipes containing between 5 to 12 ingredients. This suggests simpler recipes with fewer ingredients are more common.

<iframe
  src="n_ingredients_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

- **Distribution of Average Ratings:**
   - The pie chart displays that most recipes receive a 5-star rating (57%), followed by 4 stars (33.5%), and lower ratings account for a small percentage. This suggests users generally rate recipes positively.

<iframe
  src="pie_chart_ratings.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Bivariate Analysis

- **Number of Ingredients vs Average Rating:**
   - A box plot visualizes the relationship between the number of ingredients and average ratings, revealing that ingredient count doesn’t strongly impact ratings. Both simple and complex recipes can receive high or low ratings.
 
<iframe
  src="box_ingredients_average.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

- **Description Length vs Average Rating:**
   - The relationship between recipe description length and ratings shows that longer descriptions are slightly associated with higher average ratings.

<iframe
  src="box_description_average.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Interesting Aggregates


| Rating | Calories Mean | Calories Median | Calories Std | Description Length Mean | Description Length Median | Description Length Std |
|--------|---------------|-----------------|--------------|-------------------------|---------------------------|-------------------------|
| 0      | 515.05        | 338.1           | 776.16       | 235.67                  | 174.0                     | 246.64                  |
| 1      | 446.13        | 276.9           | 612.53       | 224.62                  | 168.5                     | 195.68                  |
| 2      | 442.99        | 307.0           | 672.84       | 221.76                  | 175.5                     | 181.24                  |
| 3      | 444.71        | 311.6           | 658.71       | 221.57                  | 172.0                     | 208.67                  |
| 4      | 411.91        | 305.6           | 504.49       | 227.03                  | 177.0                     | 195.77                  |
| 5      | 434.21        | 303.6           | 692.57       | 222.94                  | 174.0                     | 194.65                  |

This table summarizes the calorie content and description length for recipes grouped by their rating categories, ranging from 0 to 5.

1. **Caloric Trends & Ratings:**
   - Recipes with 0-star ratings have the highest average calorie count (514.55 kcal), while 4-star rated recipes have the lowest average calories (412.05 kcal), suggesting users might prefer moderately healthy recipes.

2. **Recipe Description Length & Ratings:**
   - The average description length does not vary significantly across ratings, but 0-star rated recipes tend to have the longest descriptions (236.03 characters), suggesting longer descriptions do not guarantee better ratings.

## NMAR (Not Missing at Random) Analysis

- **Missingness in `average_rating`:**
   - Missing ratings for certain recipes are linked to user engagement and are likely NMAR (Not Missing at Random). Recipes with no ratings are likely new or unpopular.

- **Analysis of Missingness Based on Recipe Characteristics:**
   - **For `minutes`:**
     - The observed difference in means was 117.34 minutes, with a p-value of 0.0520. Since the p-value is slightly above 0.05, we fail to reject the null hypothesis. Missingness in `average_rating` does not appear to be significantly dependent on the recipe’s preparation time.
    
<iframe
  src="permutation_test_minutes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
   
   - **For `n_ingredients`:**
     - The observed difference in means was 0.2542, with a p-value of 0.0020. The p-value is below 0.05, so we reject the null hypothesis, indicating that missingness in `average_rating` is significantly related to the number of ingredients.
    
 <iframe
  src="permutation_test_n_ingredients.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Test: Effect of Recipe Complexity on Average Rating

- **Null Hypothesis (H₀):**
   - The number of steps in a recipe does not affect its average rating.

- **Alternative Hypothesis (H₁):**
   - Recipes with more steps have a different average rating than simpler recipes.

- **Test Statistic:**
   - The absolute difference in means between simple recipes (fewer than 9 steps) and complex recipes (9 or more steps).

- **Significance Level (α):**
   - 0.05, a standard threshold for statistical significance.

- **Results:**
   - The observed test statistic was calculated, and the permutation test p-value was 0.0010.

- **Conclusion:**
   - Since the p-value is much lower than 0.05, we reject the null hypothesis, suggesting that the number of steps in a recipe is associated with differences in average ratings.

Justification for Choices:

- Permutation Test: This non-parametric approach is appropriate because it does not assume normality and allows for a direct comparison of observed differences under random permutations.
- Absolute Difference in Means: Captures deviations in either direction, making the test more flexible.
- Significance Level (0.05): A standard threshold that balances Type I and Type II errors.

# Recipe Rating Prediction

This project aims to predict user ratings for recipes based on various features, such as ingredients, preparation time, number of steps, and other recipe characteristics. The goal is to classify recipes into five discrete rating categories: [1, 2, 3, 4, 5].

## Problem Statement

We aim to predict the ratings of recipes based on features like calories, preparation time, simplicity, and description length. This is a multiclass classification problem, as the ratings fall into five discrete categories: [1, 2, 3, 4, 5].

### Response Variable
- The response variable is the recipe rating, representing user satisfaction with the recipe. Ratings are crucial in determining the popularity and perceived quality of recipes.

### Evaluation Metric
- **F1-score** is used as the primary evaluation metric, due to the class imbalance in the dataset. F1-score balances precision and recall, making it more appropriate than accuracy for imbalanced classes.

### Justification for Feature Selection
- The features used for prediction are intrinsic characteristics of the recipe itself, such as calories, preparation time, number of steps, and whether the recipe is considered simple or not. These features are accessible before user interactions with the recipe.

## Data Overview

The dataset contains two main tables:
1. **Recipe Dataset** (83782 rows): Contains features such as ingredients, preparation time, calories, etc.
2. **Interactions Dataset** (731927 rows): Contains user ratings and interactions for recipes.

## Baseline Model

### Model: Random Forest Classifier
- **Random Forest** is an ensemble model that builds multiple decision trees and combines their outputs to make a final prediction. It can handle both numerical and categorical features and is resistant to overfitting.

### Features Used in the Model:
- **Calories (Quantitative)**: Represents the caloric content of the recipe.
- **Simple Recipes (Nominal)**: A binary feature indicating whether the recipe is simple or not.

### Model Performance:
- **Accuracy**: 38.53%
- **F1-Score**: 
  - Rating 1: 0.01
  - Rating 2: 0.02
  - Rating 3: 0.07
  - Rating 4: 0.33
  - Rating 5: 0.51

**Key Insights**:
- The model performs well for higher ratings (4, 5), but underperforms for lower ratings (1, 2, 3) due to class imbalance.
- The model’s performance is better for the majority class (rating 5), but it struggles with predicting minority classes.

## Improvements and Model Tuning

### Hyperparameter Tuning:
- **GridSearchCV** was used to tune the hyperparameters, including:
  - **Criterion**: Gini impurity vs. entropy
  - **Max Depth**: None, 10, 20
  - **Min Samples Split**: 2, 5, 10

### Final Model Performance:
- **Accuracy**: 55.34%
- **Precision for Rating 5**: 0.60
- **Recall for Rating 5**: 0.80

### Model Improvements:
- **Feature Engineering**: New features like preparation time, submission time, and description length were added to improve model performance.
- **Hyperparameter Tuning**: Optimized model parameters to achieve better performance.

### Future Improvements:
- **Class Imbalance**: Techniques like SMOTE or adjusting class weights during training could improve model performance for minority classes.
- **Advanced Models**: Experimenting with models like XGBoost or Neural Networks could help improve prediction accuracy.
- **Feature Expansion**: Adding features like user reviews or ingredient lists could further improve the model’s predictive power.

## Fairness Analysis

### Groups:
- **Group X**: Recipes labeled as "simple recipes" (simple = 0).
- **Group Y**: Recipes labeled as "simple recipes" (simple = 1).

### Hypothesis Testing:
- **Null Hypothesis (H₀)**: No difference in precision between Group X and Group Y.
- **Alternative Hypothesis (H₁)**: There is a difference in precision between Group X and Group Y.
- **Test Statistic**: Absolute difference in precision between the two groups.
- **p-value**: 0.996 (greater than 0.05).

### Conclusion:
- We fail to reject the null hypothesis, indicating no significant difference in precision between "simple" and "non-simple" recipes. The precision difference is minimal.
