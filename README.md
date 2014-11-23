
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
