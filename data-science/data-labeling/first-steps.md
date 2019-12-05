# First Steps

## Glossary

In the documentation, you'll come across a variety of terms specific to collecting annotations with Prodigy. Here's a quick overview of the most common ones, including a short description and links for more information.

| TERM | DESCRIPTION |
| :--- | :--- |
| **annotation task** | A single question you're collecting feedback on from the annotator. For example, whether an entity is correct or whether a label applies to a text. Internally, annotation tasks are simple dictionaries containing the task properties like the text, the entity spans or the labels. Annotation tasks are also often referred to as "\(annotation\) examples". |
| **annotation interface** | The visual presentation of the annotation task. For example, text with highlighted entities, text with a category label, an image or a multiple-choice question. In the code, this is also often referred to as the `view_id`.  |
| **dataset** | A named collection of annotated tasks. A new dataset is usually created for each project or experiment. The data can be exported or used to train a model later on. |
| **session** | A single annotation session, from starting the Prodigy server to exiting it. You can start multiple sessions that add data to the same dataset. The annotations of each sessions will also be stored as a separate dataset, named after the timestamp. This lets you inspect or delete individual sessions. |
| **database** | The storage backend used to save your datasets. Prodigy currently supports SQLite \(default\), PostgreSQL and MySQL out-of-the-box, but also lets you integrate custom solutions. |
| **recipe** | A Python function that can be executed from the command line and starts the Prodigy server for a specific task – for example, correcting entity predictions or annotating text classification labels. Prodigy comes with a range of [built-in recipes](prodigy-recipes/), but also allows you to write your own. |
| **stream** | An iterable of annotation tasks, e.g. a generator that yields dictionaries. When you load in your data from a file or an API, Prodigy will convert it to a stream. Streams can be annotated in order, or be filtered and reordered to only show the most relevant examples. |
| **loader** | A function that loads data and returns a stream of annotation tasks. Prodigy comes with built-in loaders for the most common file types and a selection of live APIs, but you can also create your own functions. |
| **sorter** | A function that takes a stream of `(score, example)` tuples and yields the examples in a different order, based on the score. For example, to prefer uncertain or high scores. Prodigy comes with several built-in sorters that are used in the active learning-powered recipes. |
| **spaCy model** | One of the available pre-trained [statistical language models](https://spacy.io/models) for spaCy. Models can be installed as Python packages and are avalable in different sizes and for different languages. They can be used as the basis for training your own model with Prodigy. |
| **active learning** | Using the model to select examples for annotation based on the current state of the model. Prodigy, the selection is usually based on examples the model is most uncertain about, i.e. the ones with a prediction closest to 50/50. |
| **batch training** | Training a new model from a dataset of collected annotations. Using larger batches of data and multiple iterations usually leads to better results than just updating the model in the loop. This is why you usually want to collect annotations first, and then use them to batch train a model from scratch. |

## Create a new dataset

Datasets let you group annotations together. It's recommended to create a new dataset for each annotation project, evaluation run or experiment. If you add a description or author, this info will also be displayed in the web application

```text
$ prodigy dataset my_set "A new dataset" --author Me

✨ Created dataset 'my_set'.
```

A single annotation can be part of several datasets. Prodigy will also create a session dataset for each individual annotation session, using the timestamp as the dataset name. For a list of all datasets and sessions, use the `prodigy stats -ls` command.

## Annotate data

To start annotating, you need a source of examples. You can either load in your own data, or use one of the sample datasets below.

\*\*\*\*[**News headlines**](https://prodi.gy/assets/data/news_headlines.jsonl)  
****200 headlines from stories about Silicon Valley from the [The New York Times](https://www.nytimes.com/).

\*\*\*\*[**GitHub issues**](https://prodi.gy/assets/data/github_issues.jsonl)  
****830 [GitHub](https://github.com/) issue titles for search queries related to documentation and instructions.

## Named Entity Recognition

Looking to get started with named entity recognition or just want to refresh your memory? Download this [**handy flowchart**](https://prodi.gy/docs/pdfs/prodigy_flowchart_ner.pdf) with the most common tips, tricks and best practices.

The `ner.teach` recipe uses a spaCy model to detect entities in the stream of examples. It then starts the web server so you can ACCEPT or REJECT the entity suggestions. As you annotate, the model is updated and Prodigy will use the updated predictions to suggest the most relevant entities for annotation. All annotations you collect will be stored in your dataset.

```text
$ prodigy ner.teach my_set en_core_web_sm news_headlines.jsonl

✨ Starting the web server on port 8080...
```

![](../../.gitbook/assets/image%20%2850%29.png)

By default, all entities will be shown. To only annotate one or more specific entity labels, use the `--label` option – for example, `--label ORG` or `--label ORG,PERSON`. Keep in mind that this recipe only works for entity types already present in the model. [See here](http://spacy.io/api/annotation#named-entities) for an overview of spaCy's NER scheme.

## Text Classification

The `textcat.teach` lets you start off with an existing spaCy model to or a blank model. It starts the web server so you can ACCEPT or REJECT texts with a category label. As you annotate, the model is updated and Prodigy will use the updated predictions to suggest the most relevant texts for annotation. All annotations you collect will be stored in your dataset.

```text
$ prodigy textcat.teach my_set en_core_web_sm news_headlines.jsonl --label POLITICS

✨ Starting the web server on port 8080...
```

![](../../.gitbook/assets/image%20%28266%29.png)

## Train a model

Once all of the annotations are corrected, the best accuracy can usually be achieved by retraining from scratch. You can choose to supply an evaluation set, create an evaluation set interactively, or split off a percentage of the collected annotations for evaluation, the `--eval-split`. If you've specified an output directory, Prodigy will export the best model, together with the training and evaluation data.

```text
$ prodigy textcat.batch-train gh_issues --output-model /tmp/model --eval-split 0.2

Loaded blank model
Using 20% of examples (156) for evaluation
Using 100% of remaining examples for training

Correct    142
Incorrect  14

Baseline   0.65
Precision  0.87
Recall     0.87
F-score    0.87

Model: /tmp/model
Training data: /tmp/model/training.jsonl
Evaluation data: /tmp/model/evaluation.jsonl
```

## Use a model

After training the model, Prodigy outputs a ready-to-use [spaCy model](https://spacy.io/models), making it easy to put into production. It's recommended to use spaCy's [`package` command](https://spacy.io/api/cli#package) to turn the model into a loadable Python package.

```text
$ spacy package model /tmp --create-meta
$ python /tmp/en_model/setup.py sdist
$ pip install /tmp/en_model/dist/en_model-1.0.0.tar.gz
```

```python
import spacy

nlp = spacy.load('en_model')
doc = nlp(u"As New Zealand Courts Tech Talent, Isolation Becomes a Draw")

# Named entities
print([(ent.text, ent.label_) for ent in doc.ents])

# Text classification
print(doc.cats)
```



