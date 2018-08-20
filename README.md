# Wine-Dataset
I will use the wine dataset to determine the quality of the wine.  It contains characteristics about both white and red wines.  It contains 6,497 observations and 14 variables.  
# Description



# Data Observations


# Data Cleaning
The dataset didn't require a lot of cleaning. The quality variable was either converted to a numeric value or a factor depending on the need.  For example, to run the model I converted quality to a factor.  The numeric version of quality was used for graphing purposes.

r.w.wine$quality <- as.numeric(r.w.wine$quality)
r.w.wine$good <- as.factor(r.w.wine$good)

There were no missing values in the dataset.

# EDA


# Models



#### Linear Regression




#### Random Forest Model




#### Decision Tree Model



# Analysis


# Conclusion



# References
