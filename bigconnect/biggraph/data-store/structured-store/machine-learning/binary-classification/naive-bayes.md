# Naive Bayes

Naive Bayes classifiers are a family of simple probabilistic classifiers based on applying Bayes' theorem with strong (naive) independence assumptions between the features. In all trainers, prior probabilities can be preset or calculated. Also, there is an option to use equal probabilities.

### Gaussian Naive Bayes

Gaussian Naive Bayes algorithm is based on [this information](https://en.wikipedia.org/wiki/Naive\_Bayes\_classifier#Gaussian\_naive\_Bayes).

When dealing with continuous data, a typical assumption is that the continuous values associated with each class are distributed according to a normal (or Gaussian) distribution

The model predicts the result value y belongs to a class C\_k, k in \[0..K] as

<figure><img src="https://ignite.apache.org/docs/2.16.0/images/naive-bayes.png" alt=""><figcaption></figcaption></figure>

Where

<figure><img src="https://ignite.apache.org/docs/2.16.0/images/naive-bayes2.png" alt=""><figcaption></figcaption></figure>

The model returns the number (index) of the most possible class. The trainer counts means and variances for each class.

```
GaussianNaiveBayesTrainer trainer = new GaussianNaiveBayesTrainer();

GaussianNaiveBayesModel mdl = trainer.fit(ignite, dataCache, vectorizer);
```

### Discrete (Bernoulli) Naive Bayes

Naive Bayes algorithm over Bernoulli or multinomial distribution based on next [information](https://en.wikipedia.org/wiki/Naive\_Bayes\_classifier#Multinomial\_naive\_Bayes).

It can be used for non-continuous features. The thresholds to convert a feature to a discrete value should be set to a trainer. If the features are binary, the discrete Bayes becomes Bernoulli.

The model predicts the result value y belongs to a class C\_k, k in \[0..K] as

<figure><img src="https://ignite.apache.org/docs/2.16.0/images/naive-bayes3.png" alt="" width="563"><figcaption></figcaption></figure>

Where x\_i is a discrete feature, p\_ki is a prior probability of class p(C\_k).

The model returns the number (index) of the most possible class.

```java
double[][] thresholds = new double[][] {{.5}, {.5}, {.5}, {.5}, {.5}};

DiscreteNaiveBayesTrainer trainer = new DiscreteNaiveBayesTrainer()
  .setBucketThresholds(thresholds);

 DiscreteNaiveBayesModel mdl = trainer.fit(ignite, dataCache, vectorizer);
```

### Compound Naive Bayes

Compound Naive Bayes is a composition of several Naive Bayes classifiers where each classifier represents subset of features of one type.

The model contains both Gaussian and Discrete Bayes. A user can select which set of features will be trained on each model.

The model returns the number (index) of the most possible class.

```java
double[] priorProbabilities = new double[] {.5, .5};

double[][] thresholds = new double[][] {{.5}, {.5}, {.5}, {.5}, {.5}};

CompoundNaiveBayesTrainer trainer = new CompoundNaiveBayesTrainer()
  .withPriorProbabilities(priorProbabilities)
  .withGaussianNaiveBayesTrainer(new GaussianNaiveBayesTrainer())
  .withGaussianFeatureIdsToSkip(asList(3, 4, 5, 6, 7))
  .withDiscreteNaiveBayesTrainer(new DiscreteNaiveBayesTrainer()
                                 .setBucketThresholds(thresholds))
  .withDiscreteFeatureIdsToSkip(asList(0, 1, 2));

  CompoundNaiveBayesModel mdl = trainer.fit(ignite, dataCache, vectorizer);
```
