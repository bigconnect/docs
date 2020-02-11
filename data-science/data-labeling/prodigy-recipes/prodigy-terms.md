# Terms

## terms.train-vectors

Train a [Word2vec](https://en.wikipedia.org/wiki/Word2vec) or [Sense2vec](https://explosion.ai/blog/sense2vec-with-spacy) semantic similarity model, using [spaCy](https://spacy.io/) and [Gensim](https://radimrehurek.com/gensim/). For best results, you should provide a lot of input text \(at least 10 million words\). Because training is a batch process, the input stream must be fixed-length.

```text
$ prodigy terms.train-vectors /tmp/model en_core_web_sm /tmp/reddit.bz2 --loader reddit

✨  Trained Word2Vec model
/tmp/model/word2vec.bin
```

Word2Vec trains a lookup table that provides a "meaning" vector for each term in your vocabulary. Terms which occur in similar contexts will be mapped to similar vectors. Given enough examples of a word, the model is able to build an accurate picture of its usage in context, which reveals a lot \(but not all\) about its meaning. The vectors can then be used like a thesaurus, or as input to a neural network model.

Because word2vec models are look-up tables, each term is assigned a single meaning. The `--merge-ents` and `--merge-nps` flags are particularly useful, because they let you assign vectors to long multi-word expressions, with more predictable results than the standard mutual information-based strategy.

## terms.teach

Build a terminology list interactively using a model's word vectors and seed terms, either a comma-separated list or a text file containing one term per line.

![](../../../.gitbook/assets/image%20%28355%29.png)

## terms.to-patterns

Convert a list of seed terms to a list of match patterns that can be used with [`ner.match`](prodigy-named-entity-recognition.md#ner-match), for example to collect annotations for training a new entity type. If no output file is specified, each pattern is printed so the recipe's output can be piped forward.

```text
$ prodigy terms.to-patterns programming_langs /tmp/patterns.jsonl --label PROG_LANG

✨ Exported 59 patterns.
```

Match patterns files are JSONL files with one pattern per line. Patterns can be both exact string matches, as well as token patterns supported by [spaCy's matcher](https://spacy.io/usage/linguistic-features#rule-based-matching).

```text
PATTERNS.JSONL

{"label": "PROG_LANG", "pattern": [{"lower": "javascript"}]}
{"label": "PROG_LANG", "pattern": [{"lower": "c++"}]}
{"label": "PROG_LANG", "pattern": [{"lower": "ruby"}]}
```

