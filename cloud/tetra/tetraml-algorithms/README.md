# Algorithms

This section provides an overview of each algorithm available in Tetra ML.

### Supported Data Types

When building models, the supported data types varies per algorithm.

* All TetraML algos accept data as numerical and categorical.
* Word2Vec accepts data as text.

If your data includes timestamps, we recommend that you either convert the data to numeric (if you plan to use the data) or ignore timestamp columns.

### Supervised

In supervised learning, the dataset is labeled with the answer that algorithm should come up with. Supervised learning takes input variables (x) along with an output variable (y). The output variable represents the column that you want to predict on. The algorithm then uses these variables to learn and approximate the mapping function from the input to the output. Supervised learning algorithms support classification and regression problems.

TetraML supports the following supervised algorithms:

* [AutoML](tetraml-automl.md): Automatic Machine Learning
* [Cox Proportional Hazards (CoxPH)](tetraml-coxph.md)
* Deep Learning (Neural Networks)
* Distributed Random Forest (DRF)
* Generalized Linear Model (GLM)
* Generalized Additive Models (GAM)
* Gradient Boosting Machine (GBM)
* Naive Bayes Classifier
* RuleFit
* Stacked Ensembles
* Support Vector Machine (SVM)
* XGBoost

### Unsupervised

In unsupervised learning, the model is provided with a dataset that isn’t labeled - i.e., without an explicit outcome that the algorithm should return. In this case, the algorithm attempts to find patterns and structure in the data by extracting useful features. The model organizes the data in different ways, depending on the algorithm (clustering, anomaly detection, autoencoders, etc).

* Aggregator
* Generalized Low Rank Models (GLRM)
* Isolation Forest
* K-Means Clustering
* Principal Component Analysis (PCA)

### Miscellaneous

* Target Encoding
* TF-IDF
* Word2vec
