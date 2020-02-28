# Ludwig

Ludwig is a toolbox that allows to train and test deep learning models without the need to write code.

All you need to provide is a CSV file containing your data, a list of columns to use as inputs, and a list of columns to use as outputs, Ludwig will do the rest. Simple commands can be used to train models both locally and in a distributed way, and to use them to predict on new data.

A programmatic API is also available in order to use Ludwig from your python code. A suite of visualization tools allows you to analyze models' training and test performance and to compare them.

Ludwig is built with extensibility principles in mind and is based on data type abstractions, making it easy to add support for new data types as well as new model architectures.

It can be used by practitioners to quickly train and test deep learning models as well as by researchers to obtain strong baselines to compare against and have an experimentation setting that ensures comparability by performing standard data preprocessing and visualization.

Ludwig provides a set of model architectures that can be combined together to create an end-to-end model for a given use case. As an analogy, if deep learning libraries provide the building blocks to make your building, Ludwig provides the buildings to make your city, and you can chose among the available buildings or add your own building to the set of available ones.

The core design principles we baked into the toolbox are: - No coding required: no coding skills are required to train a model and use it for obtaining predictions. - Generality: a new data type-based approach to deep learning model design that makes the tool usable across many different use cases. - Flexibility: experienced users have extensive control over model building and training, while newcomers will find it easy to use. - Extensibility: easy to add new model architecture and new feature data types. - Understandability: deep learning model internals are often considered black boxes, but we provide standard visualizations to understand their performance and compare their predictions.

{% page-ref page="ludwig-basic-principles.md" %}

{% page-ref page="ludwig-exmples.md" %}

{% page-ref page="ludwig-user-guide/" %}

