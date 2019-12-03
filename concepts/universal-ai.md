# Universal AI

The Universal AI engine provides an in-memory, distributed, fast, and scalable AI platform that allows you to build AI models on Big Data in enterprise environments.

BigConnect provides the necessary tools to engage in developing and productionalizing AI products based on Machine Learning or Deep Learning algorithms. Training and using models however require a lot of data and processing power. The Universal AI runtime is designed in such a way to fully leverage the power of the Universal Data Model, by bringing in data using Universal Queries. This means AI developers can get their data from BigConnect using [SQL](../reference/sql-language.md), [Cypher](../reference/cypher-language.md) of [Textual](../reference/search.md) queries using simple APIs, allowing them to focus more on Data Science and not needing to worry about data integration workflows.

Training AI models require a lot of hardware resources and is very CPU/GPU intensive. Having the ability to distribute training workloads among multiple machines is a must. This is why we chose Spark as the primary option for all distributed data processing tasks. The Universal AI engine fully leverages the power of Spark for distributing training workloads across multiple machines.

![Universal AI in BigConnect](../.gitbook/assets/image%20%2883%29.png)

We carefully chose the best Machine Learning and Deep Learning tools and frameworks that can handle almost every task. BigConnect fully supports and integrates:

* [Jupyter Notebooks](https://jupyter-notebook.readthedocs.io/en/stable/) with Scala, R and Python kernels
* [H2O](https://github.com/h2oai/h2o-3)
* [Spark MLib](https://spark.apache.org/mllib/)
* [TensorFlow](https://www.tensorflow.org/)
* [Ludwig](https://uber.github.io/ludwig/)

We also provide an annotation tool for labeling data for training. Supported data types are text, images, tabular data.

![Data Labeling tool in BigConnect](../.gitbook/assets/image%20%2851%29.png)

