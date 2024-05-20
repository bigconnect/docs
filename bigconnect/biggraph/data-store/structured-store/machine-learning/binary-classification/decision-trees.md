# Decision Trees

Decision trees and their ensembles are popular methods for the machine learning tasks of classification and regression. Decision trees are widely used since they are easy to interpret, handle categorical features, extend to the multiclass classification setting, do not require feature scaling, and are able to capture non-linearities and feature interactions. Tree ensemble algorithms such as random forests and boosting are among the top performers for classification and regression tasks.

### Overview

Decision trees are a simple yet powerful model in supervised machine learning. The main idea is to split a feature space into regions such as that the value in each region varies a little. The measure of the values' variation in a region is called the impurity of the region.

Apache Ignite provides an implementation of the algorithm optimized for data stored in rows (see Partition Based Dataset).

Splits are done recursively and every region created from a split can be split further. Therefore, the whole process can be described by a binary tree, where each node is a particular region and its children are the regions derived from it by another split.

Let each sample from a training set belong to some space `S` and let `p_i` be a projection on a feature with index `i`, then a split by continuous feature with index `i` has the form:

![555](https://ignite.apache.org/docs/2.16.0/images/555.gif)

and a split by categorical feature with values from some set `X` has the form:

![666](https://ignite.apache.org/docs/2.16.0/images/666.gif)

Here `X_0` is a subset of `X`.

The model works this way - the split process stops when either the algorithm has reached the configured maximal depth, or splitting of any region has not resulted in significant impurity loss. Prediction of a value for point `s` from `S` is a traversal of the tree down to the node that corresponds to the region containing `s` and getting back a value associated with this leaf.

### Model

The Model in a decision tree classification is represented by the class `DecisionTreeModel`. We can make a prediction for a given vector of features in the following way:

```
DecisionTreeModel mdl = ...;

double prediction = mdl.apply(observation);
```

The model is a fully independent object and after the training it can be saved, serialized and restored.

### Trainer

A Decision Tree algorithm can be used for classification and regression depending upon the impurity measure and node instantiation approach.

#### Classification

The Classification Decision Tree uses the [Gini](https://en.wikipedia.org/wiki/Decision\_tree\_learning#Gini\_impurity) impurity measure and you can use it in the following way:

```
// Create decision tree classification trainer.
DecisionTreeClassificationTrainer trainer = new DecisionTreeClassificationTrainer(
    4, // Max deep.
    0  // Min impurity decrease.
);

// Train model.
DecisionTreeModel mdl = trainer.fit(ignite, dataCache, vectorizer);
```
