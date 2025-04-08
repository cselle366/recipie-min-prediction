# Predicting Cooking Time Based on Different Recipie Features 
## By: Annice Chang and Courtney Selle
### Introduction 
We chose to explore the Recipies and Ratings data set from Food.com, which contains thousands of recipies and all of the data pertaining to the recipies. As students, we prioritize cooking time when it comes to our meals, so we were particularly interested in what factors affect the amount of time a recipie takes to make. Our central question is as follows: 
> How accurately can we predict a recipie's cooking time based on its ingridients, steps, and nutritional content?

This question is valuble for both meal planning and time management, and can also help users find recipies based upon preferred cooking time. The dataset contains 731,927 rows and relevant columns to our study include: minutes, nutrition, n_steps, and n_ingridients. We utilized these columns in order to transform the data into individual features such as calories, protein, and sugar. By building this predictive model, we strive to capture the complexity of real world recipies and really investigate the factors affecting cooking time. 

### Data Cleaning and Exploratory Data Analysis
We first left merged the recipes and interactions datasets to ensure our dataset included all recipes. We filled all ratings of 0 with NaN to represent missing values. Next, we calculated the average rating for each recipe and added the series as a new column in the merged dataset. We converted string columns, such as tags, nutrition, and ingredients, into lists for feature extraction. For example, we split the ‘nutrition’ into separate attributes (e.g., calories, protein, sugar). To address missing values in the minutes column, we implemented probabilistic imputation. Specifically, we sampled from the distribution of observed minutes values and randomly assigned values to missing entries based on that distribution. This approach preserves the shape of the data and avoids biasing the model toward mean or median imputation. Finally, we removed outliers by filtering out any recipes with a preparation time of 1,000 minutes or more, ensuring that our modeling and visualization focused on more realistic cooking times.

Below are some graphs that are relevant to our study. 
Box Plot of Recipie Minutes explores the distribution of average minutes each recipie takes. 
<iframe
 src="univariate1.html"
 width="600"
 height="400"
 frameborder="0"
 ></iframe>
Box Plot of Recipie Steps explores the distribution of average number of steps each recipie takes. 
 <iframe
 src="univariate2.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
Average Minutes by Number of Ingridients explores how the number of ingridients affects the cooking time of a recipie. 
 <iframe
 src="bivariate1.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
Average Number of Steps by Number of Ingridients explores how the number of ingridients affects the number of steps of a recipie.
 <iframe
 src="bivariate2.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
 
### Framing a Prediction Problem

### Baseline Model

### Final Model 
