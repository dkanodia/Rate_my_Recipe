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

## Prediction Problem Statement:

I aim to predict the ratings of recipes based on various features such as ingredients, cooking time, number of steps, and user reviews. This is a multiclass classification problem, as the ratings fall into five discrete categories: [1, 2, 3, 4, 5].

Response Variable:
 - The response variable is the recipe rating, which represents user satisfaction with a given recipe. We chose this because ratings are a key factor in determining the popularity and perceived quality of recipes.

Evaluation Metric:
- We will use the F1-score as the primary evaluation metric instead of accuracy because:

Since Ratings are unbalanced i.e higher concentration of 4s and 5s, we can use F1-score balances precision and recall, making it a better choice in imbalanced scenarios.
Accuracy could be misleading if the model simply predicts the most common rating without learning meaningful patterns.

Justification for Feature Selection:
- At the time of prediction, we have access to all the columns in the rating dataset, as listed in the introduction section. These features describe the recipes themselves—such as ingredients, cooking time, number of steps, and other attributes—ensuring that our model only uses information that would be available even if no users have rated or reviewed the recipe yet. This guarantees that our predictions are based solely on intrinsic recipe characteristics rather than future user interactions.

## Baseline Model

Model Description
 - We used a Random Forest Classifier for this classification problem. A Random Forest is an ensemble model that builds multiple decision trees and combines their outputs to make a final prediction, providing a robust and reliable model that can handle both numerical and categorical features.

Features in the Model
We selected the following features for the baseline model:

 - `Calories` (Quantitative): This is a continuous variable representing the caloric content of the recipe.
 - `Simple Recipes` (Nominal): This feature indicates whether a recipe is considered "simple" (e.g., a binary flag, 1 for "simple," 0 for "not simple").

Feature Types and Encoding:

Quantitative Features:
 - calories: This is already a numerical feature and used directly without any encoding.
Nominal Features:
- simple recipes: This feature was encoded using binary encoding, where "simple" is represented as 1 and "not simple" is represented as 0.

Model Performance
- After training the model on 80% of the dataset and evaluating it on the remaining 20%, we used the following metrics:

Classification Report (Performance Metrics):
- The model's performance is summarized below:

Precision: The proportion of true positive predictions relative to all positive predictions for each class.
Recall: The proportion of true positive predictions relative to all actual positives for each class.
F1-score: The harmonic mean of precision and recall, which balances both metrics.
Support: The number of instances for each class in the test set.

              precision    recall  f1-score   support
           1       0.01      0.04      0.01       125
           2       0.01      0.05      0.02       135
           3       0.05      0.14      0.07       768
           4       0.35      0.32      0.33      5667
           5       0.59      0.45      0.51      9527
    accuracy                           0.39     16222
   macro avg       0.20      0.20      0.19     16222
weighted avg       0.47      0.39      0.42     16222
Accuracy: 38.53%

Analysis of Model Performance:

 - Class Imbalance: The model performs well on the more frequent ratings (4 and 5), but it significantly underperforms on ratings 1, 2, and 3. This suggests the model is biased towards predicting higher ratings due to the class imbalance.

 - F1-scores: The F1-scores for the lower ratings (1, 2, 3) are very low, indicating that the model struggles to predict these classes accurately.

 - Macro vs. Weighted Average: The macro average F1-score is quite low (0.19), which shows that the model is not handling the minority classes well.
However, the weighted average F1-score (0.42) is slightly better, reflecting better performance on the majority class (rating 5).

The model is not ideal for the following reasons:

- Low Performance on Minority Classes: The model does not generalize well across all ratings. It struggles particularly with predicting lower ratings (1, 2, and 3).

- Class Imbalance: The model's high accuracy is largely driven by the prevalence of higher ratings (4 and 5). This suggests that the model may simply be predicting the majority class, which is not ideal for this task.

- Room for Improvement: The accuracy is low (38.53%), which indicates that the model could benefit from improvements in feature selection, handling class imbalance, or trying different models.

Next Steps for Improvement:
- Feature Engineering: We can add more relevant features such as ingredient count, preparation time, or user reviews to provide the model with more information.

- HyperParameter tuning: Tuning all hyperparameters using gridSearchCV can give better results.

## Modeling and Feature Engineering for Recipe Rating Prediction

1. Features Added and Their Relevance
The following features were selected based on their potential to provide meaningful insights into the recipe's characteristics, which would likely influence the user ratings:

1.1 Calories (Quantitative)
Reasoning: The number of calories in a recipe is an important factor when predicting ratings. Recipes with higher caloric content might appeal to different user segments (e.g., those who are calorie-conscious vs. those who are not). This feature provides essential information about the recipe's complexity and dietary appeal.
1.2 Preparation Time in Minutes (Quantitative)
Reasoning: The amount of time required to prepare a recipe is often correlated with the complexity and effort involved. Recipes that take longer to prepare may be perceived as more sophisticated, which could influence their rating. Users often rate based on the effort required and the perceived value of that effort.
1.3 Submission Time (Quantitative)
Reasoning: The age of a recipe could impact its rating. Older recipes might have been rated based on outdated tastes or preferences, while newer recipes might reflect current trends or seasonal preferences. This feature provides a temporal perspective, showing whether newer recipes tend to receive different ratings.
1.4 Description Length (Quantitative)
Reasoning: A longer recipe description might indicate a more detailed and potentially more sophisticated recipe. It may also correlate with the clarity and thoroughness of the recipe, both of which can influence user ratings.
1.5 Simple Recipes (Nominal)
Reasoning: This binary feature indicates whether the recipe is considered "simple" or not. Simpler recipes may appeal to users who prefer quick and easy meals, while more complex recipes might attract users who enjoy culinary challenges. This feature can help differentiate between user preferences for easy vs. complex dishes.
These features were selected based on the data generation process. Recipes that are more detailed, fresh, complex, or with higher caloric content might receive different ratings from users, and including them in the model helps the model learn these patterns. These features are aligned with user behavior and preferences in the context of recipe ratings.

2. Model Choice and Hyperparameter Tuning
2.1 Model Selection: Random Forest Classifier
The Random Forest Classifier was chosen for this task because:

It is robust and handles both numerical and categorical data well.
It can model complex relationships between features without requiring specific assumptions.
It is resistant to overfitting and works well with a large number of features.
2.2 Hyperparameters Tuned:
The GridSearchCV method was used to perform hyperparameter optimization. The following hyperparameters were tuned:

Criterion: gini (used for splitting nodes based on Gini impurity)
Max Depth: None (allows trees to grow until all leaves are pure or until the minimum samples split is reached)
Min Samples Split: 2 (nodes are split when at least 2 samples are present)
These hyperparameters were chosen based on the performance observed during cross-validation and experimentation. The max depth of None and min samples split of 2 allowed the model to grow large trees and capture complex relationships, improving its ability to classify recipes accurately.

2.3 Hyperparameter Selection Process:
GridSearchCV was employed with a 5-fold cross-validation approach to search through the following values:
Criterion: gini, entropy
Max Depth: None, 10, 20
Min Samples Split: 2, 5, 10
This exhaustive search ensures the best hyperparameters are selected based on model performance on the validation set.

3. Final Model vs. Baseline Model
3.1 Baseline Model Performance
Accuracy: 38.53%
Main issue: The model struggled to predict minority classes (ratings 1, 2, 3), likely due to class imbalance.
3.2 Final Model Performance
After applying hyperparameter tuning, the model achieved the following:

Accuracy: 55.34%
Precision for Rating 5: 0.60 (Significant improvement compared to baseline)
Recall for Rating 5: 0.80 (Better performance in predicting higher ratings)
The final model improves upon the baseline model's performance, particularly with higher ratings. The accuracy improvement from 38.53% to 55.34% is notable. However, the model still struggles with predicting the minority classes (ratings 1, 2, and 3), indicating the need for further improvements in handling class imbalance.

3.3 Model Improvements:
Hyperparameter Tuning: The final model’s hyperparameters (criterion, max depth, and min samples split) allow for better decision tree growth and more accurate predictions for the majority class.
Feature Engineering: The additional features, like calories, prep time, and description length, provide more relevant information, allowing the model to distinguish between different ratings more effectively.
4. Future Improvements and Next Steps
Class Imbalance: Implementing techniques such as SMOTE (Synthetic Minority Over-sampling Technique) or adjusting class weights during training could improve the model’s performance on the minority classes.
Advanced Models: Experimenting with models such as XGBoost or Neural Networks may provide better handling of the class imbalance and complex relationships between features.
Feature Expansion: Adding more features, such as user reviews or ingredient list characteristics, could provide more insights into user preferences and further improve prediction accuracy.
5. Conclusion
The final model provides a solid foundation for predicting recipe ratings, with notable improvements over the baseline model. While it performs well for high ratings (4 and 5), further efforts to address class imbalance and experiment with advanced models will be essential for improving the prediction of lower ratings.


## Fairness Analysis

1. Choice of Group X and Group Y:
Group X (0): Recipes labeled as "simple recipes" (i.e., 0).
Group Y (1): Recipes labeled as "simple recipes" (i.e., 1).
2. Evaluation Metric:
The evaluation metric is Precision. Specifically, we are using weighted precision, which accounts for class imbalances when calculating the precision for each class.
3. Null and Alternative Hypotheses:
Null Hypothesis (H₀): There is no difference in the model's precision between Group X (simple recipes = 0) and Group Y (simple recipes = 1).
Alternative Hypothesis (H₁): The model's precision is different between Group X (simple recipes = 0) and Group Y (simple recipes = 1).

4. Choice of Test Statistic:
The test statistic is the difference in precision between Group X and Group Y. This is measured by:

Observed Precision Difference=∣Precision of Group X−Precision of Group Y∣

We compare this observed difference to the distribution of differences generated by permuting the target labels (average_rating) within the data.
5. Significance Level:
The significance level (α) is typically set at 0.05, which means that we will reject the null hypothesis if the p-value is less than 0.05.
6. Resulting p-value:
After performing the permutation test with 1,000 permutations, the resulting p-value is computed. Based on this p-value, we can make a decision on whether to reject the null hypothesis.
The p-value represents the proportion of permutations where the precision difference is greater than or equal to the observed precision difference. If this p-value is less than 0.05, we reject the null hypothesis, suggesting that there is a significant difference in precision between the two groups.
7. Conclusion:
If the p-value is less than 0.05, we reject the null hypothesis, indicating that the model's precision significantly differs between recipes that are simple (Group X) and those that are not (Group Y).
If the p-value is greater than or equal to 0.05, we fail to reject the null hypothesis, suggesting that there is no significant difference in precision between the two groups.
