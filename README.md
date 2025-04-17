# Predicting Cooking Time Based on Different Recipe Features 
## By: Annice Chang and Courtney Selle
## Introduction 
We chose to explore the Recipes and Ratings dataset from Food.com, which contains thousands of recipes and all of the data pertaining to the recipes. As students, we prioritize cooking time when it comes to our meals, so we were particularly interested in what factors affect the amount of time a recipe takes to make. Our central question is as follows:
> How accurately can we predict a recipie's cooking time based on its ingridients, steps, and nutritional content?

This question is valuable for both meal planning and time management, and can also help users find recipes based on preferred cooking time. The dataset contains 731,927 rows, and relevant columns to our study include: minutes, nutrition, n_steps, and n_ingredients. We utilized these columns in order to transform the data into individual features such as calories, protein, and sugar. By building this predictive model, we strive to capture the complexity of real-world recipes and investigate the factors affecting cooking time.

## Data Cleaning and Exploratory Data Analysis 
### Cleaning 
We first left-merged the recipes and interactions datasets to ensure our dataset included all recipes. We filled all ratings of 0 with NaN to represent missing values. Next, we calculated the average rating for each recipe and added the series as a new column in the merged dataset. We converted string columns, such as tags, nutrition, and ingredients, into lists for feature extraction. For example, we split the ‘nutrition’ column into separate attributes (e.g., calories, protein, sugar). To address missing values in the minutes column, we implemented probabilistic imputation. Specifically, we sampled from the distribution of observed minutes values and randomly assigned values to missing entries based on that distribution. This approach preserves the shape of the data and avoids biasing the model toward mean or median imputation. Finally, we removed outliers by filtering out any recipes with a preparation time of 1,000 minutes or more, ensuring that our modeling and visualization focused on more realistic cooking times.

Head of our cleaned data frame:

|     id |   minutes |   n_steps |   n_ingredients |   calories |   sugar |   protein |
|-------:|----------:|----------:|----------------:|-----------:|--------:|----------:|
| 306785 |        40 |         4 |               8 |       95.3 |      50 |         5 |
| 310237 |        30 |         9 |              10 |      143.5 |      25 |        10 |
| 321038 |        22 |        14 |              14 |      182.4 |      50 |        11 |
| 321038 |        22 |        14 |              14 |      182.4 |      50 |        11 |
| 342209 |        40 |         7 |              12 |      658.2 |     151 |        24 |


### Exploratory Data Analysis
#### Univariate Analysis

<iframe
 src="univariate1.html"
 width="600"
 height="400"
 frameborder="0"
 ></iframe>
 Box Plot of Recipe Minutes explores the distribution of average minutes each recipe takes. The box plot shows that most recipes take between about 56–106 minutes, with a mean of around 80 minutes. There are also a few outliers around 240 minutes, so there is a wide range of recipe times.
 <iframe
 src="univariate2.html"
 width="600"
 height="400"
 frameborder="0"
 ></iframe>
 Box Plot of Recipe Steps explores the distribution of the average number of steps each recipe takes. The box plot shows that most recipes are completed in 8–19 steps, with a mean of about 15 steps. However, some recipes take up to 37 steps, indicating that most recipes take a moderate number of steps. 

#### Bivariate Analysis
 
 <iframe
 src="bivariate1.html"
 width="600"
 height="400"
 frameborder="0"
 ></iframe>
 Average Minutes by Number of Ingredients explores how the number of ingredients affects the cooking time of a recipe. The chart shows a general upward trend, where as the number of ingredients increases, the average number of minutes the recipes take also increases, showing a direct relationship between the number of ingredients and cooking time.
 <iframe
 src="bivariate2.html"
 width="600"
 height="400"
 frameborder="0"
 ></iframe>
 Average Number of Steps by Number of Ingredients explores how the number of ingredients affects the number of steps in a recipe. The chart shows a general upward trend where recipes with more ingredients tend to have more steps, indicating a direct relationship between the number of ingredients and number of steps.


### Grouping and Aggregates 
Pivot table showing how mean number of minutes varies by the number of ingredients:

|   n_ingredients |   mean_minutes |   recipe_count |
|----------------:|---------------:|---------------:|
|               1 |        36.375  |             96 |
|               2 |        50.0282 |           2273 |
|               3 |        36.699  |           7033 |
|               4 |        33.9626 |          12581 |
|               5 |        46.0601 |          20123 |

This table provides insight that there is no linear relationship between ingredients and time, so the number of ingredients might not be a perfect predictor of cooking time. Other factors such as step complexity or ingredient type likely play a major role. This table suggests we should explore other features to capture the complexity better.

## Framing a Prediction Problem
Our project addresses a regression problem. We are predicting the total time (in minutes) it takes to prepare a recipe. This is a continuous variable, making regression the appropriate model type rather than classification. The response variable we are predicting is minutes, which represents the total time required to complete a recipe. We chose this target because it is a relevant and useful prediction for us—especially as students who don’t always have a lot of time—to estimate cooking time based on the recipe details.


We used Mean Squared Error (MSE) as the primary evaluation method because it is standard for regression tasks and effectively penalizes larger errors more than smaller ones. We considered columns such as “n_ingredients” and “n_steps” to train the model, both of which are features that we would know before the time of prediction, which is important for constructing the prediction model.


## Baseline Model
Our baseline model is a linear regression model that aims to predict a recipe's cooking time based on two features: n_ingredients and n_steps, which are both quantitative. Since both features were numerical, we did not perform any encoding for this model, but we standardized the features using StandardScaler inside of an sklearn pipeline so that the model treats the features equally during training. We used the evaluation metric Mean Squared Error because it penalizes larger prediction errors more heavily. The MSE value we got of 7619.55 squared minutes shows that our model is off by a large margin—approximately 87.3 minutes per recipe. This large error is expected because this is a baseline model using only two simple features, so there is significant room for improvement in our final model.

## Final Model 
### New Features
To improve upon our baseline model, we added features that we thought would enhance our model's predictive power. We added three new quantitative features from the nutrition column that were chosen based on their potential influence on cooking time: 

`calories`: Recipes with higher calorie counts may result in longer cooking times because the food could be more rich or dense.

`protein`: Higher protein suggests cooking meat or fish, which could increase cooking time due to the need to cook and marinate the protein.

`sugar`: high sugar content could suggest a dessert or baked good, which might take longer due to the baking time. 

These features were added because they show how the nutritional complexity of a recipe could affect its cooking time.

### Feature Engineering 
We used a RandomForestRegressor, a non-linear method that can properly handle complex relationships between features. Our final feature set included: n_ingredients, n_steps, calories, protein, and sugar. Since there were no qualitative features, we did not use encoding. However, we did standardize the data with StandardScaler and implemented a pipeline in sklearn to ensure clean preprocessing.

### Algorithm and Hyperparameter 
Then, we used GridSearchCV to search over two important hyperparameters: n_estimators and max_depth. We tested n_estimators with values of 50 and 100, and max_depth with values of 5, 10, and None. The best combination with the highest performance was found when n_estimators = 100 and max_depth = None. The final model has a Mean Squared Error of 1180.44 squared minutes, which is a huge improvement from the baseline model. The final model is off by an average of 34.4 minutes per recipe, which is a significant improvement over the baseline model’s error. Therefore, the final model is significantly more accurate than the baseline model. This is likely due to the final model's added features that reflect real differences in recipe complexity.

### Model Performance 
To visualize our model's performance, we created a plot of the actual versus predicted cooking times. This visualization shows that most predictions fall close to the actual values, meaning that our model effectively captures the trends in the data.
  
<iframe
 src="prediction.html"
 width="600"
 height="400"
 frameborder="0"
 ></iframe>
 This scatter plot compares the actual versus the predicted cooking times, showing that while predictions generally follow the overall trend, it is less accurate for super short or super long cooking times.



