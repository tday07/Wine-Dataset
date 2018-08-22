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
The data variables include fixed acidity, volatile acidity, citric acid, residual sugar, chlorides, free sulfur dioxide, total sulfur dioxide, density, pH, sulphates, alcohol, quality, good, and color.

I decided to explore the quality variable.  Below is a histogram of the quality. As shown in the graph, majority of the wine types were classified as "good quality" (6).

![Histogram_Wine_Quality](Histogram_Wine_Quality.PNG)

Below is a histogram of the quality of wine by color of wine.  There is more white wine considered "good quality" than red wine.  It also looks like there are more white wines in the dataset compared to red wine.

![Quality_Wine_By_Color](Quality_Wine_By_Color.PNG)

![Alcohol_By_Color](Alcohol_By_Color.PNG)

![Fixed_Acidity_by_Wine_Type](Fixed_Acidity_by_Wine_Type.PNG)

![Volatile_Acidity_by_Wine_Type](Volatile_Acidity_by_Wine_Type.PNG)

![Citric_Acid_by_Wine_Type](Citric_Acid_by_Wine_Type.PNG)

![pH_by_Color](pH_by_Color.PNG)

![Correlogram_of_Wine_Data](Correlogram_of_Wine_Data.PNG)

# Models
The following packages need to be loaded for each model and graphs.
library(ggplot2)                                                                                                                         
library(ggthemes)                                                                                                                       
library(GGally)                                                                                                                         
library(tidyverse)                                                                                                                       
library(magrittr)                                                                                                                       
library(dplyr)                                                                                                                           
library(hexbin)                                                                                                                         
library(randomForest)                                                                                                                   
library(caret)                                                                                                                           
library(rpart)                                                                                                                           
library(rpart.plot)                                                                                                                     
library(ggcorrplot)                                                                                                                     
library(pscl)                                                                                                                           
library(ROCR)                                                                                                                           

#### Linear Regression
I first split my data into a test and training set.  The quality variable also needs to be converted to a factor before running the model.  I then fit the model and the results showed that some of the variables like volatile acidity and residual sugar are statistically significant.

wine_adjusted_2$quality <- as.factor(wine_adjusted_2$quality)                                                                           
set.seed(123)                                                                                                                           
ind = sample(2, nrow(wine_adjusted_2), replace = TRUE, prob=c(0.7,0.3))                                                                 
trainwine = wine_adjusted_2[ind == 1,]                                                                                                   
testwine = wine_adjusted_2[ind == 2,]                                                                                                   
model <- glm(quality ~., family=binomial(link='logit'),data=trainwine)                                                                   
summary(model)                                                                                                                           

![Linear_Regression_Results](Linear_Regression_Results.PNG)

I also ran an ANOVA test.  This showed that some more variables were statistically significant.  Some of those variables include alcohol, sulphates, and pH.

anova(model, test="Chisq")                                                                                                               

![Anova_Results](Anova_Results.PNG)

#### Random Forest Model
set.seed(123)                                                                                                                           
indrf = sample(2, nrow(wine_adjusted_2), replace = TRUE, prob=c(0.7,0.3))                                                               
trainrf = wine_adjusted_2[indrf == 1,]                                                                                                   
testrf = wine_adjusted_2[indrf == 2,]                                                                                                   
loan.rf = randomForest(quality ~ ., data=trainrf, importance = T)                                                                       
loan.rf                                                                                                                                 

![loan.rf](loan.rf.PNG)

loan.prediction = predict(loan.rf, testrf)                                                                                               
confusionMatrix(table(loan.prediction, testrf$quality))                                                                                 

![confusion_matrix](confusion_matrix.PNG)

The importance variables are shown below.

![varimpplot](varimpplot.PNG)

#### Decision Tree Model
set.seed(123)                                                                                                                           
inddt = sample(2, nrow(wine_adjusted_2), replace = TRUE, prob=c(0.6,0.4))                                                               
traindt = wine_adjusted_2[inddt == 1,]                                                                                                   
testdt = wine_adjusted_2[inddt == 2,]                                                                                                   

The confusion matrix for the decision tree model is shown below.

predictions = predict(loan.rp, testdt, type= "class")                                                                                   
table(testdt$quality, predictions)                                                                                                       
confusionMatrix(table(predictions, testdt$quality))                                                                                     

![confusion_matrix_dt](confusion_matrix_dt.PNG)
![decision_tree_plot](decision_tree_plot.PNG)

prune.tree = prune(loan.rp, cp = loan.cp)                                                                                               
rpart.plot(prune.tree,tweak=1.3)                                                                                                         
predictions.pt = predict(prune.tree, testdt, type="class")                                                                               
table(testdt$quality, predictions.pt)                                                                                                   
confusionMatrix(table(predictions.pt, testdt$quality))                                                                                   

I pruned the decision tree but it didn't make much of a difference as shown by the confusion matrix below.

![pruned_tree_confusion_matrix](pruned_tree_confusion_matrix.PNG)

# Analysis


# Conclusion



# References
