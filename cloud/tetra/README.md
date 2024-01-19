# BigConnect Tetra ML

Tetra is an in-memory, distributed, fast, and scalable machine learning and predictive analytics platform that allows you to build machine learning models on big data and provides easy productionalization of those models in an enterprise environment.

Inside Tetra, a Distributed Key/Value store is used to access and reference data, models, objects, etc., across all nodes and machines. The algorithms are implemented on top of H2O’s distributed Map/Reduce framework and utilize the Java Fork/Join framework for multi-threading. The data is read in parallel and is distributed across the cluster and stored in memory in a columnar format in a compressed way. H2O’s data parser has built-in intelligence to guess the schema of the incoming dataset and supports data ingest from multiple sources in various formats.

Tetra’s REST API allows access to all the capabilities of Tetra from an external program or script via JSON over HTTP. The Rest API is used by Tetra’s web interface (Tetra Flow UI), R binding, and Python binding.

The speed, quality, ease-of-use, and model-deployment for the various cutting edge Supervised and Unsupervised algorithms like Deep Learning, Tree Ensembles, and GLRM make Tetra a highly sought after API for big data data science.

{% content-ref url="tetraml-algorithms/" %}
[tetraml-algorithms](tetraml-algorithms/)
{% endcontent-ref %}

{% content-ref url="using-tetraflow/" %}
[using-tetraflow](using-tetraflow/)
{% endcontent-ref %}

