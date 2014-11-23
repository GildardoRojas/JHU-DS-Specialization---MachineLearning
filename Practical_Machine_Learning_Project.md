# Practical Machine Learning - Course Project
Gildardo Rojas Nandayapa  
Sunday, November 23, 2014  
**SUMMARY**

The goal of the project is to create machine learning models on a training data set, assess their performance and apply the best one to predict the manner 6 subjects performed an excercise. Data has been collected from activity monitoring devices. Each of the subjects were asked to perform barbell lifts correctly and in 5 different incorrect ways. The second part of the projecte requires the predictions on a the test set to be submitted for automated grading.

**ENVIRONMENT PREPARATION**

Here we load the libraries and set variables to be used.


```
## Warning: package 'caret' was built under R version 3.1.2
```

```
## Loading required package: lattice
## Loading required package: ggplot2
```

**LOADING DATA**

The input data consists of movement measurements including acceleration components of the arms and pitch and roll orientations of the dumbell.

More information is available from the website here: <http://groupware.les.inf.puc-rio.br/har>



**PREPROCESSING DATA & FEATURE SELECTION**


```r
#Remove near zero covariates (predictors)
nzv <- nearZeroVar(trainSet, saveMetrics = T)

# Create new toTrain data set removing 
toTrain <- trainSet[, !nzv$nzv]
# This reduces from 160 variables to 100

# Remove features with high percentage of missing values (hpmv)
hpmv <- sapply(colnames(toTrain), function(x) if(sum(is.na(toTrain[, x])) > 0.7*nrow(toTrain)) {return(T)} else {return(F)})
toTrain <- toTrain[, !hpmv]
```

**CROSSVALIDATION AND MODEL SELECTION**

In this section we generate two models, first Boosting Model then Random Forests, in both cases we leave the Crossvalidation process to be handled by caret package by using the trainControl parameters.


```r
# First fitting Boosting Model boosting algorithm and 8-fold cross validation to predict classe with all selected features.
set.seed(123)
#boostFit<-train(classe ~ ., method="gbm", data=toTrain, verbose=F, trControl = trainControl(method="cv", number=8))
#boostFit
#plot(boostFit, ylim=c(0.9,1))

# Then fitting Random Forests Model algorithm and 8-fold cross validation to predict classe with all selected features.
set.seed(123)
#rfFit <- train(classe ~ ., method="rf", data=toTrain, importance=T, trControl = trainControl(method="cv", number=8))
#rfFit
#plot(rfFit, ylim=c(0.9,1))
```

The Boosting Model generated a very accurate model with accuracy close to 1, but Random Forests algorithm generated even a more accurate model. Compared to boosting model, this Random Forests generally has better performance in terms of accuracy.

**PREDICTING USING THE DATA TEST**


```r
# final model
# rfFit$finalModel
# prediction
# (prediction <- as.character(predict(rfFit, testing)))
```

**CONCLUSION**

* Random forests model has overall better accuracy compared to Boosting model.
* Estimated out of sample error rate for the random forests model is 0.04%.

