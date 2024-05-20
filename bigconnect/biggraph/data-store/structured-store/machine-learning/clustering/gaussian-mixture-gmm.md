# Gaussian mixture (GMM)

A Gaussian mixture model is a probabilistic model that assumes all the data points are generated from a mixture of a finite number of Gaussian distributions with unknown parameters.

### Model

This algorithm represents a soft clustering model where each cluster is a Gaussian distribution with its own mean value and covariation matrix. Such a model can predict a cluster using the maximum likelihood principle.

It defines the labels by the following way:

```java
KMeansModel mdl = trainer.fit(
    ignite,
    dataCache,
    vectorizer
);

double clusterLabel = mdl.predict(inputVector);
```

### Trainer

GMM is a unsupervised learning algorithm. The GaussianMixture object implements the expectation-maximization (EM) algorithm for fitting mixture-of-Gaussian models. It can compute the Bayesian Information Criterion to assess the number of clusters in the data.

Presently, Ignite ML supports a few parameters for the GMM classification algorithm:

* \`maxCountOfClusters \` - the number of possible clusters
* \`maxCountOfIterations \` - one stop criteria (the other one is epsilon)
* `epsilon` - delta of convergence(delta between old and new centroid’s values)
* `countOfComponents` - the number of components
* `maxLikelihoodDivergence` - maximum divergence between maximum of likelihood of vector in dataset and other for anomalies identification
* `minElementsForNewCluster` - minimum required anomalies in terms of maxLikelihoodDivergence for creating new cluster
* `minClusterProbability` - minimum cluster probability

```java
// Set up the trainer
GmmTrainer trainer = new GmmTrainer(COUNT_OF_COMPONENTS);

// Build the model
GmmModel mdl = trainer
    .withMaxCountIterations(MAX_COUNT_ITERATIONS)
    .withMaxCountOfClusters(MAX_AMOUNT_OF_CLUSTERS)
    .fit(ignite, dataCache, vectorizer);
```
