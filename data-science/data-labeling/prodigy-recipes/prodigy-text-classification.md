# Text Classification

## textcat.teach

Collect the best possible training data for a text classification model with the model in the loop. Based on your annotations, Prodigy will decide which questions to ask next. All annotations will be stored in the database. You can stream in examples from a file or a live API. If no stream source is specified, it defaults to `sys.stdin`, letting you pipe through data via the `pipe` recipe.

```bash
$ prodigy textcat.teach news_headlines en_core_web_sm headlines.jsonl --label POSITIVE

✨ Starting the web server on port 8080...
```

![](../../../.gitbook/assets/image%20%28161%29.png)

## textcat.manual

Manually annotate categories that apply to a text. If more than one label is specified, categories are added as multiple choice options. If the `--exclusive` flag is set, categories become mutually exclusive, meaning that only one can be selected during annotation.

```bash
$ prodigy textcat.manual news_headlines en_core_web_sm headlines.jsonl --label POSITIVE,NEGATIVE,NEUTRAL --exclusive

✨ Starting the web server on port 8080...
```

![](../../../.gitbook/assets/image%20%28239%29.png)

## textcat.batch-train

Batch train a new text classification model from annotations. Prodigy will export the best result to the output directory, and include a JSONL file of the training and evaluation examples. You can either supply a dataset ID containing the evaluation data, or choose to split off a percentage of examples for evaluation. By default, 50% is split off for datasets under 1000 examples, and 20% for sets over 1000 examples. The `factor` specifies the portion of examples to train on – e.g. `0.2` for 20% or `1` for all examples. If no input model is specified, a blank spaCy model is used. If no output directory is specified, no model will be exported.

```bash
$ prodigy textcat.batch-train gh_issues /tmp/model --n-iter 10 --eval-split 0.2 --dropout 0.2

Starting with blank model
Using 20% of examples (156) for evaluation

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

## textcat.train-curve

Batch-train models with different portions of the training examples and print the accuracy figures and accuracy improvements. `n_samples` is the numer of sample models to train at different stages. For instance, `10` will train models for 10% of the examples, 20%, 30% and so on. This recipe is useful to determine the quality of the collected annotations, and whether more training examples will improve the accuracy. As a rule of thumb, if accuracy improves within the last 25%, training with more examples will likely result in better accuracy.

```bash
$ prodigy textcat.train-curve gh_issues --n-iter 10 --eval-split 0.2 --dropout 0.2 --n-samples 4

Starting with blank model
Using 20% of examples (156) for evaluation

 %         ACCURACY
25%        0.73       +0.73
50%        0.82       +0.09
75%        0.84       +0.02
100%       0.89       +0.05
```

## textcat.eval

Evaluate a text classification model and build an evaluation set from a stream. If no stream source is specified, it defaults to `sys.stdin`, letting you pipe through data via the `pipe` recipe. After you're done annotating, Prodigy will print detailed evaluation stats, based on all evaluation examples in the evaluation dataset. This means you can always restart the process to add more examples to the set.

```bash
$ prodigy textcat.eval gh_issues_eval /tmp/model docs.jsonl --label DOCUMENTATION

✨ Starting the web server on port 8080...

MODEL      USER       COUNT
accept     accept     47
accept     reject     7
reject     reject     95
reject     accept     7

Correct    142
Incorrect  14

Baseline   0.65
Precision  0.87
Recall     0.87
F-score    0.87
```

## textcat.print-stream

Pretty-print annotations from a stream on the command line. When piping the stream to `less` to view it, remember to use the `-r` flag so that the color displays correctly. If no stream source is specified, it defaults to `sys.stdin`, letting you pipe through data via the [`pipe`](https://prodi.gy/docs/recipes#pipe) recipe.

```bash
prodigy textcat.print-stream headlines.jsonl | less -r

0.34   POSITIVE Yahoo Decides to Release a Rosy Forecast
0.67   POSITIVE Facebook’s Developer Conference Kicks Off
0.12   POSITIVE Economy Has Become a Drag on Silicon Valley
```

## textcat.print-dataset

Pretty-print annotations from a given dataset on the command line. When piping the stream to `less` to view it, remember to use the `-r` flag so that the color displays correctly.

```bash
$ prodigy textcat.print-dataset news_headlines | less -r

0.34   POSITIVE Yahoo Decides to Release a Rosy Forecast
0.67   POSITIVE Facebook’s Developer Conference Kicks Off
0.12   POSITIVE Economy Has Become a Drag on Silicon Valley
```



