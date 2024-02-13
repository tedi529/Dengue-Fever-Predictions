# Dengue Fever Predictions in San Juan, PR and Iquitos, Peru
Using regression techniques, I predicted the number of cases of dengue fever on a weekly basis during the years 2008 - 2013 in two cities in the Western Hemisphere: San Juan, Puerto Rico and Iquitos, Peru.

The model was developed with data from a machine learning competition where it scored in the top 20% of entries based on number of cases predicted. The competition page can be found at the following URL: 
[DengAI: Predicting Disease Spread](https://www.drivendata.org/competitions/44/dengai-predicting-disease-spread).

## Background
The task was to predict the number of **total_cases** of Dengue Fever in each city for each week of the years 2008-2013 in the test data. The dataset consisted of various numeric climate data measurements such as temperature, precipitation, humidity, etc. A full list can be found on the competition page.

The performance evalution metric was [mean absolute error (MAE)](https://en.wikipedia.org/wiki/Mean_absolute_error). 

## Method
There were 20 features and 1456 observations in the training data set. The test data set on which predictions of total cases were made consisted of 416 observations. 

While examining the data more closely:
1. I noted two features which duplicated information and one of these was eliminated: *precipitation_amt_mm*.
2. The city column was encoded to numeric values in order for the model to be able to differentiate between the information for the two cities.

I examined the correlation matrix of the data in order to see the correlation coeffiecients of features. The matrix revealed there were a number of features which were highly correlated. It is customary to keep variables with correlation coefficients smaller than an absolute value of 0.8 as highly correlated features can introduce noise into the data. 
![image](https://github.com/tedi529/Dengue-Fever-Predictions/assets/22670096/06777a1f-a090-4d51-b9b2-3ee5a5e7b07e)

### Baseline Model
For the baseline model, I removed observations which had *null* values for any of the features. This reduced the total observation count by 257 observations from 1456 to 1199. I decided this was acceptable for the baseline model.  

I kept the encoded city data combined for the baseline model, predicting total case counts for both San Juan and Iquitos at the same time.
I used a number of common regression techniques to determine the one which yielded the best results for this dataset. 

- Baseline LinearRegression() MAE: **16.980**  
- Baseline Lasso(alpha=0.04) MAE: **16.967**  
- Baseline ElasticNet(alpha=0.01, l1_ratio=0.49) MAE: **16.978**  
- Baseline Ridge() MAE: **16.949**  
- Baseline GradientBoostingRegressor() MAE: **17.044**  

Based on the MAE above, the model which yielded the best results for the combined cities data was **Ridge Regression**.

### Paremeter Tuning & Feature Selection
I experimented with various parameters for the Lasso, Elastic Net and Ridge models above in order to determine the values of the parameters which yielded the best results and chose those values. 
To examine which features yielded the most accurate predictions, I also ran models with different combinations of features. I decided to keep features with a correlation coefficient less than 0.8 to each other and drop the features containing the bigger number of null observations. 

This experimentation yielded the following results:

- Dropped Features LinearRegression() MAE: **16.352**
- Dropped Features Lasso(alpha=0.04) MAE: **16.316**
- Dropped Features ElasticNet(alpha=0.01, l1_ratio=0.49) MAE: **16.353**
- Dropped Features Ridge() MAE: **16.330**
- Dropped Features GradientBoostingRegressor() MAE: **17.237**

Based on the MAE above, the model which yielded the best results when feature selection techniques were applied was **Lasso Regression**. 

### Separate Models
Upon further examination of the data, I could see that San Juan and Iquitos have a pretty well delineated separation among features. The scatterplot below shows the different values in the feature variables between the two cities.
![image](https://github.com/tedi529/Dengue-Fever-Predictions/assets/22670096/385e810e-a759-46c5-a859-4e953667191b)

As such, I decided to separate the data for the two cities and run the above regression models on each city individually. This technique resulted in the following MAEs. 

- Dropped Features SJ LinearRegression() MAE: **17.877**
- Dropped Features SJ Lasso(alpha=0.04) MAE: **17.961**  
- Dropped Features SJ ElasticNet(alpha=0.01, l1_ratio=0.49) MAE: **18.096**  
- Dropped Features SJ Ridge() MAE: **17.970**  
- Dropped Features SJ GradientBoostingRegressor() MAE: **18.668**  
-------------------------------------------------------------------------
- Dropped Features IQ LinearRegression() MAE: **4.913**  
- Dropped Features IQ Lasso(alpha=0.04) MAE: **5.007**  
- Dropped Features IQ ElasticNet(alpha=0.01, l1_ratio=0.49) MAE: **4.959**  
- Dropped Features IQ Ridge() MAE: **4.947**  
- Dropped Features IQ GradientBoostingRegressor() MAE: **5.779**  

## Results
I submitted the results of my models to the competition website iteratively while experimenting with different parameters, features, and data cleaning techniques.
While my test results for the baseline and feature engineered models hovered around the MAE of **16**, upon submission, the results were closer to **26**.

The smallest MAE which I achieved for my model was using the **Lasso Regression** models on the two cities separately while keeping the majority of features.  

This resulted in a competition MAE of **26.2548** which put me in the **top 20%** of submissions. 




