# Part-Of-Speech

## pos.teach

Collect the best possible training data for a part-of-speech tagging model with the model in the loop. Based on your annotations, Prodigy will decide which questions to ask next. All annotations will be stored in the database. You can stream in examples from a file or a live API. If no stream source is specified, it defaults to `sys.stdin`, letting you pipe through data via the `pipe` recipe or your own scripts.

![](../../../.gitbook/assets/image%20%28182%29.png)

## pos.make-gold

Create gold-standard data for part-of-speech tagging by correcting the model's suggestions. The spaCy model will be used to predict part-of-speech tags, which annotator can remove and correct if necessary. It's often more efficient to focus on a few labels at a time, instead of annotating all labels jointly.

```bash
$ prodigy pos.make-gold news_headlines_gold en_core_web_sm news.jsonl --label "VERB, NOUN, PROPN" --exclude news_headlines

✨ Starting the web server on port 8080...
```

![](../../../.gitbook/assets/image%20%28432%29.png)

## pos.gold-to-spacy

Convert a dataset with annotated part-of-speech tags to the format required to train spaCy's part-of-speech tagger. The data will be formatted in the "simple training style" and can be read in and used to update the tagger. See the [spaCy training documentation](https://spacy.io/usage/training#example-train-tagger) for more details.

The recipe will export a JSONL file or print to `stdout` if no output file is specified. Each entry is a line that looks like this:

```bash
["I like eggs", {"tags": ["NOUN", "VERB", "NOUN"]}]
["I like ripe bananas", {"tags": ["NOUN", "VERB", "-", "NOUN"]}]
```

spaCy's tagger can also be updated from partial annotations, which will be exported with the tag `-`. Note that this converter currently expects all annotated spans to be single tokens. If multiple tokens were highlighted as one tag, the example will be skipped.

```bash
$ prodigy pos.gold-to-spacy news_headlines_gold /tmp/pos-tags.jsonl

✨ Exported 429 examples.
```

## pos.batch-train

Batch train a part-of-speech tagging model from annotations. Prodigy will export the best result to the output directory, and include a JSONL file of the training and evaluation examples. You can either supply a dataset ID containing the evaluation data, or choose to split off a percentage of examples for evaluation. By default, 50% is split off for datasets under 1000 examples, and 20% for sets over 1000 examples. The `factor` specifies the portion of examples to train on – e.g. `0.2` for 20% or `1` for all examples. If no output directory is specified, no model will be exported.

```bash
$ prodigy pos.batch-train news_headlines --output /tmp/model --label PROPN --n-iter 10 --eval-split 0.2 --dropout 0.2

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

## pos.train-curve

Batch-train models with different portions of the training examples and print the accuracy figures and accuracy improvements. `n_samples` is the numer of sample models to train at different stages. For instance, `10` will train models for 10% of the examples, 20%, 30% and so on. This recipe is useful to determine the quality of the collected annotations, and whether more training examples will improve the accuracy. As a rule of thumb, if accuracy improves within the last 25%, training with more examples will likely result in better accuracy.

```bash
$ prodigy pos.train-curve news_headlines en_core_web_sm --label PROPN --n-iter 10 --eval-split 0.2 --dropout 0.2 --n-samples 4

Starting with model en_core_web_sm
Using 20% of examples (310) for evaluation

 %         ACCURACY
25%        0.63       +0.63
50%        0.72       +0.09
75%        0.81       +0.09
100%       0.82       +0.01
```

