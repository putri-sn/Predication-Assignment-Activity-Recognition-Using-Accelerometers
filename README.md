# Prediction-Assignment-Activity-Recognition-Using-Accelerometers
**Author:** Putri SN

## **Introduction**

In this task, accelerometer data is used to predict how subjects perform weightlifting exercises. The goal is to build a model that can accurately classify 20 test cases. This will use the Random Forest model, which is well suited for high-dimensional classification problems. The data comes from accelerometers on the belt, forearm, arm, and dumbbell of 6 participants.

Please reference the links below for the data sources:

https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv

https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv

## **Load Libraries and Data**

`library(caret)`

`library(randomForest)`

`library(rpart)`

`library(rpart.plot)`

`library(ggplot2)`

`set.seed(1234)`

`#Load data`
`trainUrl <- "https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv"`

`testUrl <- "https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv"`

training <- read.csv(url(trainUrl), na.strings = c("NA", "#DIV/0!", ""))
testing  <- read.csv(url(testUrl), na.strings = c("NA", "#DIV/0!", ""))
 
## **Data Cleaning**
 
`# Remove columns with mostly NA values`

`training <- training[, colSums(is.na(training)) == 0]`
 
`# Remove irrelevant columns (IDs, timestamps)`

`training <- training[, -(1:7)]`
 
`# Make sure classe is a factor`

`training$classe <- as.factor(training$classe)`
 
`# Same cleanup for testing`

`testing <- testing[, colnames(testing) %in% colnames(training)]`
 
## **Partition the Training Set**
 
`inTrain <- createDataPartition(training$classe, p = 0.7, list = FALSE)`

`trainSet <- training[inTrain, ]`

`testSet <- training[-inTrain, ]`
 
## **Train a Random Forest Model**
 
`control <- trainControl(method = "cv", number = 5)`

`modelRF <- train(classe ~ ., data = trainSet, method = "rf", trControl = control)`
 
## **Model Accuracy**
 
`predictions <- predict(modelRF, testSet)`

`confusionMatrix(predictions, testSet$classe)`
 
## **Predict 20 Test Cases**
 
`finalPredictions <- predict(modelRF, testing)`

`finalPredictions`
 
## **Out-of-Sample Error Estimate**

Based on cross-validation and evaluation on the holdout testSet, the out-of-sample error is approximately:
 
`1 - confusionMatrix(predictions, testSet$classe)$overall['Accuracy']`
 
**Conclusion**

Random Forest model to predict activity classes using accelerometer data. The model achieved high accuracy on the validation set and was used to predict 20 test cases for the final quiz submission.
