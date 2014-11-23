
Project for Practical Machine Learning course
==============================================

We comment ou R program, which is located in the same repo.

This is a simple R script.
(May be it lacks to declare some libraries)

We choose to suppress some variable that where computed from other (e.g min or max).
here is the code used to achieve this suppression :

```
suppressed_features<-grep("var_*|avg_*|stddev_*|max_*|*amplitude_*|min_*|skewness_*|kurtosis_*", names(projtrain))
# this command will suppress 100 variables in the dataset.
projtrain2<-projtrain[,-suppressed_features]
```

Same for the test set.

We create a Data Partition in order to minimize the volume of data.
In fact as we use cross validation later this data partition is useless.

We use repeated cross validation to train our Random Forest algorithm.

We use the parallel library Snow to parallelize the computation which takes around 40-50'.

```
cluster <- makeCluster(2)
registerDoSNOW(cluster)
```

Our Random Forest algo is limited to 100 trees. The code used is
```
modFit<-train(classe ~ ., data=projtrain2,method="rf",
              trControl= ctrl,prox=TRUE,metric="ROC",ntree=100)
```

Finally we do some classical confusion matrix computation.
```
Call:
 randomForest(x = x, y = y, ntree = 100, mtry = param$mtry, proximity = TRUE) 
               Type of random forest: classification
                     Number of trees: 100
No. of variables tried at each split: 41

        OOB estimate of  error rate: 0.01%
Confusion matrix:
     A    B    C    D    E  class.error
A 3912    0    0    0    0 0.0000000000
B    0 2644    0    0    0 0.0000000000
C    0    1 2400    0    0 0.0004164931
D    0    0    0 2243    0 0.0000000000
E    0    0    0    1 2537 0.0003940110
```

But in fact I get only 7/20 at the submission. It seems my model is not efficient ! :-((


