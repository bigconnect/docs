# Named Entity Recognition

## ner.teach

Collect the best possible training data for a named entity recognition model with the model in the loop. Based on your annotations, Prodigy will decide which questions to ask next. All annotations will be stored in the database. You can stream in examples from a file or a live API. If no stream source is specified, it defaults to `sys.stdin`, letting you pipe through data via the `pipe` recipe or your own scripts.

```text
$ prodigy ner.teach news_headlines en_core_web_sm headlines.jsonl

✨ Starting the web server on port 8080...
```

![](../../../../.gitbook/assets/image%20%28104%29.png)

## ner.batch-train

Batch train a Named Entity Recognition model from annotations. Prodigy will export the best result to the output directory, and include a JSONL file of the training and evaluation examples. You can either supply a dataset ID containing the evaluation data, or choose to split off a percentage of examples for evaluation. By default, 50% is split off for datasets under 1000 examples, and 20% for sets over 1000 examples. The `factor` specifies the portion of examples to train on – e.g. `0.2` for 20% or `1` for all examples. If no output directory is specified, no model will be exported.

```text
$ prodigy ner.batch-train news_headlines --output /tmp/model --n-iter 10 --eval-split 0.2 --dropout 0.2

Starting with model en_core_web_sm
Using 20% of examples (310) for evaluation
Correct    1281
Incorrect   269
Baseline   0.65
Accuracy   0.82
Model: /tmp/model
Training data: /tmp/model/training.jsonl
Evaluation data: /tmp/model/evaluation.jsonl
```

## ner.train-curve

Batch-train models with different portions of the training examples and print the accuracy figures and accuracy improvements. `n_samples` is the numer of sample models to train at different stages. For instance, `10` will train models for 10% of the examples, 20%, 30% and so on. This recipe is useful to determine the quality of the collected annotations, and whether more training examples will improve the accuracy. As a rule of thumb, if accuracy improves within the last 25%, training with more examples will likely result in better accuracy.

```text
$ prodigy ner.train-curve news_headlines en_core_web_sm --n-iter 10 --eval-split 0.2 --dropout 0.2 --n-samples 4

Starting with model en_core_web_sm
Using 20% of examples (310) for evaluation
 %         ACCURACY
25%        0.63       +0.63
50%        0.72       +0.09
75%        0.81       +0.09
100%       0.82       +0.01
```

## ner.match

Suggest phrases that match a given patterns file, and mark whether they are examples of the entity you're interested in. The patterns file can include exact strings, regular expressions, or token patterns for use with spaCy's `Matcher` class. You can bootstrap match patterns by creating a terminology list using the `terms.teach` recipe and converting it using `terms.to-patterns`.

```bash
$ prodigy terms.to-patterns fruits_terms /tmp/fruits_patterns.jsonl --label FRUIT
✨ Exported 49 patterns.

$ prodigy ner.match fruits_ner en_core_web_sm /tmp/food_texts.jsonl --patterns /tmp/fruits_patterns.jsonl
✨ Starting the web server on port 8080...
```

![](../../../../.gitbook/assets/image%20%28297%29.png)

## ner.manual

Mark entity spans in a text by highlighting them and selecting the respective labels. The model is used to tokenize the text to allow less sensitive highlighting, since the token boundaries are used to set the entity spans. The label set can be defined as a comma-separated list on the command line or as a path to a text file with one label per line. If no labels are specified, Prodigy will check if labels are present in the model. This recipe does not require an entity recognizer, and doesn't do any active learning.

```bash
$ prodigy ner.manual news_headlines en_core_web_sm news.jsonl --label "PERSON, ORG, GPE, LOC"

✨ Starting the web server on port 8080...
```

## ner.make-gold

Create gold-standard data for NER by correcting the model's suggestions. The spaCy model will be used to predict entities contained in the text, which the annotator can remove and correct if necessary.

```bash
prodigy ner.make-gold news_headlines_gold en_core_web_sm news.jsonl --label "PERSON, ORG, GPE, LOC"

✨ Starting the web server on port 8080...
```

![](../../../../.gitbook/assets/image%20%28122%29.png)

## ner.silver-to-gold

Take existing "silver" datasets with binary accept/reject annotations, merge the annotations to find the best possible analysis given the constraints defined in the annotations, and manually edit it to create a perfect and complete "gold" dataset.

```bash
$ prodigy ner.silver-to-gold news_headlines_gold news_headlines_org,news_headlines_product en_core_web_sm --label ORG,PRODUCT

✨ Starting the web server on port 8080...
```

## ner.eval

Evaluate an NER model and build an evaluation set from a stream. If no stream source is specified, it defaults to `sys.stdin`, letting you pipe through data via the `pipe` recipe or your own scripts.. After you're done annotating, Prodigy will print detailed evaluation stats, based on all evaluation examples in the evaluation dataset. This means you can always restart the process to add more examples to the set.

```bash
$prodigy ner.eval news_headlines_eval /tmp/model headlines.jsonl

✨ Starting the web server on port 8080...

Correct    1281
Incorrect   269
Total      1550
Accuracy   0.82
```

## ner.eval-ab

Evaluate an NER model by comparing it to another model, for example, the model before training to the model after training. Both models will be used to make predictions on a stream of text, and the results will be shown side by side. Similar to the `compare` recipe, Prodigy will randomly select which example to suggest as the correct answer, to prevent bias during annotation. If no stream source is specified, it defaults to `sys.stdin`, letting you pipe through data via the `pipe` recipe or your own scripts.

```bash
$ prodigy ner.eval-ab news_headlines_ab en_core_web_sm /tmp/model headlines.jsonl

✨ Starting the web server on port 8080...

A           82         Before
B          217         After
Ignore      25
Total      324
```

![](../../../../.gitbook/assets/image%20%28475%29.png)

## ner.print-stream

Pretty-print annotations from a stream on the command line. When piping the stream to `less` to view it, remember to use the `-r` flag so that the color displays correctly. If no stream source is specified, it defaults to `sys.stdin`, letting you pipe through data via the `pipe` recipe or your own scripts.

```bash
$ prodigy ner.print-stream en_core_web_sm headlines.jsonl | less -r

0.61   Australia’s Impressionists NORP review
0.54   Linda Burney attacks Bolt PERSON's use of 'apartheid'
0.70   Morning TIME mail: Labor wants inquiry on cladding
```

## ner.print-best

Predict the highest-scoring parse for examples in a dataset. Scores are calculated using the annotations in the dataset, and the statistical model. The annotation examples will be printed on the command line, so they can be piped forward to a recipe like `mark`, or saved to a file.

```bash
$ prodigy ner.print-best raw_set en_core_web_sm | less
$ prodigy ner.print-best raw_set en_core_web_sm | prodigy mark annotated_set --label PERSON
```

## ner.print-dataset

Pretty-print annotations from a given dataset on the command line. When piping the stream to `less` to view it, remember to use the `-r` flag so that the color displays correctly.

```bash
prodigy ner.print-dataset news_headlines | less -r

0.61   Australia’s Impressionists NORP review
0.54   Linda Burney attacks Bolt PERSON's use of 'apartheid'
0.70   Morning TIME mail: Labor wants inquiry on cladding
```

## ner.gold-to-spacy

Convert a dataset of gold-standard NER annotations \(created with `ner.manual` or `ner.make-gold` into training data for spaCy. See the [spaCy training documentation](https://spacy.io/usage/training#ner) for more details. Will export a JSONL file with one entry per line.

```bash
DEFAULT FORMAT
["I like London", {"entities": [[7, 13, "LOC"]]}]
```

```bash
BILUO FORMAT
["I like London", ["O", "O", "U-LOC", "O"]]
```

Exporting annotations in [BILUO](https://spacy.io/api/annotation#biluo) format requires a spaCy model for tokenization, which should be the same model used during annotation. The recipe will export a JSONL file or print to `stdout` if no output file is specified.

```bash
$ prodigy ner.gold-to-spacy news_headlines_gold /tmp/ner_gold.jsonl

✨ Exported 429 examples.
```

## ner.iob-to-gold

Convert a file with IOB tags into JSONL format for use in Prodigy. The input format should have one line per text, with whitespace-delimited tokens. Each token should have two or more fields delimited by the `|` character. The first field should be the text, and the last an IOB or IOB2 formatted NER tag. If no output is specified, the output is printed to `stdout`.

```bash
EXAMPLE (IOB)
Then|RB|O ,|,|O the|DT|I-MISC International|NNP|I-MISC became|VBD|O polarised|VBN|O
```

```bash
EXAMPLE (IOB2)
Then|RB|O ,|,|O the|DT|B-MISC International|NNP|I-MISC became|VBD|O polarised|VBN|O
```

```bash
$ prodigy ner.iob-to-gold /annotations.iob /annotations.iob.jsonl
```



