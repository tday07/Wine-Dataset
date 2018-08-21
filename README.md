# Wine-Dataset
I will use the wine dataset to determine the quality of the wine.  It contains characteristics about both white and red wines.  It contains 6,497 observations and 14 variables.  

# Description
This projected stemmed from the curiosity in each variable and it's impact on the quality of the wine.  I don't know a lot about wine and didn't know that characteristics like pH, density, chlorides, etc. had such a big impact on the quality of wine.  I used a supervised learning technique in R to analyze this data.  The dataset came from the following site: https://www.kaggle.com/aleixdorca/wine-quality. 

# Data Observations
Questions on original review of the dataset include:

* Will Alcohol percentage and citric acid be the most important variables when it comes to determining the quality of the wine?
* What variables are the highest correlated variables to quality?
* Will chlorides be the least important variable in the dataset?

# Data Cleaning
The dataset didn't require a lot of cleaning. The quality variable was either converted to a numeric value or a factor depending on the need.  For example, to run the model I converted quality to a factor.  The numeric version of quality was used for graphing purposes.

r.w.wine$quality <- as.numeric(r.w.wine$quality)                                                                                         
r.w.wine$good <- as.factor(r.w.wine$good)

There were no missing values in the dataset.  The quality variable contained 9 different values.  These values were changed to either a 0 or a 1.  Anything below a 5 was changed to a 0 (poor quality) and anything 5 and up was changed to a 1 (good quality).

r.w.wine$quality[r.w.wine$quality == "1"] <- 0                                                                                           
r.w.wine$quality[r.w.wine$quality == "2"] <- 0                                                                                           
r.w.wine$quality[r.w.wine$quality == "3"] <- 0                                                                                           
r.w.wine$quality[r.w.wine$quality == "4"] <- 0                                                                                           
r.w.wine$quality[r.w.wine$quality == "5"] <- 1                                                                                           
r.w.wine$quality[r.w.wine$quality == "6"] <- 1                                                                                           
r.w.wine$quality[r.w.wine$quality == "7"] <- 1                                                                                           
r.w.wine$quality[r.w.wine$quality == "8"] <- 1                                                                                           
r.w.wine$quality[r.w.wine$quality == "9"] <- 1                                                                                           

I chose to scale the data using the scale() package.  I converted the scaled data into a data frame and then added back in the quality, good, and color variables since these did not need to be scaled.

wine = subset(r.w.wine, select = -c(13,14) )                                                                                             
wine_scaled <- scale(wine)                                                                                                               
wine_scaled_df <- as.data.frame(wine_scaled)                                                                                             
wine_adjusted = subset(wine_scaled_df, select = -c(12))                                                                                 
wine_adjusted_2 <- cbind(wine_adjusted, quality = r.w.wine$quality, good = r.w.wine$good, color = r.w.wine$color)                       

I reviewed the adjusted dataset using the packages below.

summary(wine_adjusted_2)                                                                                                                 
str(wine_adjusted_2)                                                                                                                     

# EDA


# Models



#### Linear Regression




#### Random Forest Model




#### Decision Tree Model



# Analysis


# Conclusion



# References
