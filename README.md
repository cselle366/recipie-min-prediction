# Predicting Cooking Time Based on Different Recipie Features 
## By: Annice Chang and Courtney Selle
### Introduction 
We chose to explore the Recipies and Ratings data set from Food.com, which contains thousands of recipies and all of the data pertaining to the recipies. As students, we prioritize cooking time when it comes to our meals, so we were particularly interested in what factors affect the amount of time a recipie takes to make. Our central question is as follows: 
> How accurately can we predict a recipie's cooking time based on its ingridients, steps, and nutritional content?

This question is valuble for both meal planning and time management, and can also help users find recipies based upon preferred cooking time. The dataset contains 731,927 rows and relevant columns to our study include: minutes, nutrition, n_steps, and n_ingridients. We utilized these columns in order to transform the data into individual features such as calories, protein, and sugar. By building this predictive model, we strive to capture the complexity of real world recipies and really investigate the factors affecting cooking time. 

### Data Cleaning and Exploratory Data Analysis
We first left merged the recipes and interactions datasets to ensure our dataset included all recipes. We filled all ratings of 0 with NaN to represent missing values. Next, we calculated the average rating for each recipe and added the series as a new column in the merged dataset. We converted string columns, such as tags, nutrition, and ingredients, into lists for feature extraction. For example, we split the ‘nutrition’ into separate attributes (e.g., calories, protein, sugar). To address missing values in the minutes column, we implemented probabilistic imputation. Specifically, we sampled from the distribution of observed minutes values and randomly assigned values to missing entries based on that distribution. This approach preserves the shape of the data and avoids biasing the model toward mean or median imputation. Finally, we removed outliers by filtering out any recipes with a preparation time of 1,000 minutes or more, ensuring that our modeling and visualization focused on more realistic cooking times.

Below are some graphs that are relevant to our study.


<iframe
 src="univariate1.html"
 width="600"
 height="400"
 frameborder="0"
 ></iframe>
 Box Plot of Recipie Minutes explores the distribution of average minutes each recipie takes. The box plot shows that most recipies take in between about 56 - 106 minutes with a mean of around 80 minutes. There are also a few outliers around 240 minutes, so there is a wide range of recipie times. 
 <iframe
 src="univariate2.html"
 width="600"
 height="400"
 frameborder="0"
 ></iframe>
 Box Plot of Recipie Steps explores the distribution of average number of steps each recipie takes. The box plot shows that most recipies are completed between 8 - 19 steps, with a mean of about 15 steps. However, there are some recipies that take up to 37 steps, so most of the recipies take an intermediate amount of steps. 
 <iframe
 src="bivariate1.html"
 width="600"
 height="400"
 frameborder="0"
 ></iframe>
 Average Minutes by Number of Ingridients explores how the number of ingridients affects the cooking time of a recipie. The chart shows a general upwards trend where as the number of ingridients increases, the average number of minutes the recipies will take also increases showing a direct relationship between number of ingridients and cooking time. 
 <iframe
 src="bivariate2.html"
 width="600"
 height="400"
 frameborder="0"
 ></iframe>
 Average Number of Steps by Number of Ingridients explores how the number of ingridients affects the number of steps of a recipie. The chart shows a general upwards trend where recipies with more ingridients tend to have more steps, so there is a direct relationship between number of ingridients and number of steps. 
 
### Framing a Prediction Problem
Our project addresses a regression problem. We are predicting the total time (in minutes) it takes to prepare a recipe. This is a continuous variable, making regression the appropriate model type rather than classification. The response variable we are predicting is minutes, which represents the total time required to complete a recipe. We chose this target because it is a relevant and useful prediction for us, especially as students who don’t always have a lot of time, to estimate cooking time based on the recipe details. 
We used Mean Squared Error (MSE) as the primary evaluation method because it is standard for regression tasks and effectively penalizes larger errors more than smaller ones. We considered columns such as “n_ingredients” and “n_steps” to train the model, both of which are features that we would know before the time of prediction, which is important for constructing the prediction model. 


### Baseline Model
Our baseline model is a linear regression model that aims to predict a recipie's cooking time based upon two features: n_ingridients and n_steps which are both quanitative. Since both features were numerical, we did not perform any encoding for this model, but we standardized the features using StandarScalar inside of an sklearn pipeline so that the model treats the features equally during training. We used the evaluation metric Mean Squared Error because it penalizes larger prediciton errors more heavily. The MSE value we got of 7619.55 squared minutes shows that our data is off by a large margin, approximately off by 87.3 minutes per recipie. This large error is due to the fact that this is a baseline model, only using two simple features, and so there is a lot to improve upon for our final model. 

### Final Model 
