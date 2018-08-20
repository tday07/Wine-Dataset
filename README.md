# Wine-Dataset
I will use the wine dataset to determine the quality of the wine.  It contains characteristics about both white and red wines.  It contains 6,497 observations and 14 variables.  
# Description



# Data Observations


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

# EDA


# Models



#### Linear Regression




#### Random Forest Model




#### Decision Tree Model



# Analysis


# Conclusion



# References
